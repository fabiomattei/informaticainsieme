---
title: 'Ruby on Rails: la stessa applicazione, scritta in PHP puro'
date: '2026-08-27T08:19:00+02:00'
author: Fabio Mattei
layout: page
---

## Perché questo confronto

Nelle pagine precedenti abbiamo costruito, pezzo dopo pezzo, un piccolo blog
con Rails: articoli, commenti, autenticazione, validazioni. Per capire
davvero **quanto lavoro fa Rails al posto nostro**, proviamo a scrivere la
stessa identica applicazione (elenco articoli, dettaglio, creazione, login)
in **PHP puro**, senza framework, usando solo PDO per il database.

Non è un esercizio per dire che un approccio è "migliore" dell'altro: è un
modo concreto per vedere, riga per riga, cosa fanno automaticamente il
routing, ActiveRecord e le validazioni di Rails, quando tocca scriverlo a
mano.

## La struttura dei file

{% highlight text %}
mio_blog_php/
├── config/
│   └── database.php
├── includes/
│   ├── db.php
│   └── funzioni.php
├── articoli.php     (elenco + creazione)
├── articolo.php     (dettaglio + commenti)
├── login.php
├── logout.php
└── register.php
{% endhighlight %}

A differenza di Rails, qui non esiste un router centrale: **ogni pagina
visibile dal browser corrisponde ad un file PHP**, ed è il nome del file
stesso a fare da "rotta".

## Il database

{% highlight sql %}
CREATE TABLE utenti (
  id INT AUTO_INCREMENT PRIMARY KEY,
  email VARCHAR(255) UNIQUE NOT NULL,
  password_hash VARCHAR(255) NOT NULL
);

CREATE TABLE articoli (
  id INT AUTO_INCREMENT PRIMARY KEY,
  titolo VARCHAR(255) NOT NULL,
  testo TEXT NOT NULL,
  created_at DATETIME NOT NULL
);

CREATE TABLE commenti (
  id INT AUTO_INCREMENT PRIMARY KEY,
  articolo_id INT NOT NULL,
  testo TEXT NOT NULL,
  FOREIGN KEY (articolo_id) REFERENCES articoli(id) ON DELETE CASCADE
);
{% endhighlight %}

In Rails queste tre tabelle (con relativi indici e chiave esterna) sarebbero
il risultato di tre migrazioni generate automaticamente; qui vanno scritte
e mantenute a mano, ed è compito nostro ricordarsi `ON DELETE CASCADE` per
ottenere lo stesso comportamento di `dependent: :destroy`.

## La connessione al database (l'equivalente "manuale" di ActiveRecord)

{% highlight php %}
<?php
// includes/db.php
function ottieni_connessione(): PDO {
    static $pdo = null;

    if ($pdo === null) {
        $pdo = new PDO(
            "mysql:host=localhost;dbname=mio_blog;charset=utf8mb4",
            "root",
            "",
            [PDO::ATTR_ERRMODE => PDO::ERRMODE_EXCEPTION]
        );
    }

    return $pdo;
}
{% endhighlight %}

Da notare l'uso dei **parametri preparati** (`?` nelle query, valorizzati
con `execute([...])`): è il modo con cui, in PHP, ci si protegge a mano
dalla SQL injection. In Rails questa protezione è automatica ogni volta che
si usano i metodi di ActiveRecord (`where`, `find_by`...).

## Elenco e creazione articoli: articoli.php

{% highlight php %}
<?php
require __DIR__ . "/includes/db.php";
require __DIR__ . "/includes/funzioni.php";
session_start();

$errori = [];

// gestione dell'invio del form (equivalente dell'azione "create")
if ($_SERVER["REQUEST_METHOD"] === "POST") {
    $titolo = trim($_POST["titolo"] ?? "");
    $testo  = trim($_POST["testo"] ?? "");

    // validazione manuale: qui tocca a noi scrivere quello che
    // in Rails sarebbe "validates :titolo, presence: true"
    if ($titolo === "") {
        $errori[] = "Il titolo non può essere vuoto";
    }
    if ($testo === "") {
        $errori[] = "Il testo non può essere vuoto";
    }

    if (empty($errori)) {
        $pdo = ottieni_connessione();
        $stmt = $pdo->prepare(
            "INSERT INTO articoli (titolo, testo, created_at) VALUES (?, ?, NOW())"
        );
        $stmt->execute([$titolo, $testo]);

        // equivalente di "redirect_to" con un messaggio flash
        $_SESSION["messaggio"] = "Articolo creato con successo";
        header("Location: articoli.php");
        exit;
    }
}

// equivalente dell'azione "index": leggere tutti gli articoli
$pdo = ottieni_connessione();
$articoli = $pdo->query("SELECT * FROM articoli ORDER BY created_at DESC")->fetchAll();
?>
<!DOCTYPE html>
<html lang="it">
<head>
  <meta charset="UTF-8">
  <title>Il mio blog</title>
</head>
<body>

<?php if (isset($_SESSION["messaggio"])): ?>
  <div class="flash"><?= htmlspecialchars($_SESSION["messaggio"]) ?></div>
  <?php unset($_SESSION["messaggio"]); ?>
<?php endif; ?>

<h1>Articoli</h1>

<ul>
  <?php foreach ($articoli as $articolo): ?>
    <li>
      <a href="articolo.php?id=<?= (int) $articolo["id"] ?>">
        <?= htmlspecialchars($articolo["titolo"]) ?>
      </a>
    </li>
  <?php endforeach; ?>
</ul>

<h2>Nuovo articolo</h2>

<?php if (!empty($errori)): ?>
  <ul class="errori">
    <?php foreach ($errori as $errore): ?>
      <li><?= htmlspecialchars($errore) ?></li>
    <?php endforeach; ?>
  </ul>
<?php endif; ?>

<form method="post" action="articoli.php">
  <label>Titolo <input type="text" name="titolo"></label>
  <label>Testo <textarea name="testo"></textarea></label>
  <button type="submit">Crea</button>
</form>

</body>
</html>
{% endhighlight %}

Confrontando questo unico file con le pagine viste in precedenza, si vede
bene quanto lavoro viene svolto qui manualmente e che in Rails è già
pronto: il routing (qui è il nome del file stesso), lo strato ORM (qui è
SQL scritto a mano), la validazione, i messaggi flash (qui simulati con
`$_SESSION`), la conversione automatica in HTML sicuro (qui è
`htmlspecialchars`, l'equivalente dell'escape automatico che Rails applica
di default in ogni `<%= %>`).

## Dettaglio articolo e commenti: articolo.php

{% highlight php %}
<?php
require __DIR__ . "/includes/db.php";
session_start();

$id = (int) ($_GET["id"] ?? 0);
$pdo = ottieni_connessione();

$stmt = $pdo->prepare("SELECT * FROM articoli WHERE id = ?");
$stmt->execute([$id]);
$articolo = $stmt->fetch();

if (!$articolo) {
    http_response_code(404);
    echo "Articolo non trovato";
    exit;
}

// aggiunta di un nuovo commento
if ($_SERVER["REQUEST_METHOD"] === "POST") {
    $testo = trim($_POST["testo"] ?? "");
    if ($testo !== "") {
        $stmt = $pdo->prepare("INSERT INTO commenti (articolo_id, testo) VALUES (?, ?)");
        $stmt->execute([$id, $testo]);
        header("Location: articolo.php?id=" . $id);
        exit;
    }
}

$stmt = $pdo->prepare("SELECT * FROM commenti WHERE articolo_id = ?");
$stmt->execute([$id]);
$commenti = $stmt->fetchAll();
?>
<!DOCTYPE html>
<html lang="it">
<head><meta charset="UTF-8"><title><?= htmlspecialchars($articolo["titolo"]) ?></title></head>
<body>

<h1><?= htmlspecialchars($articolo["titolo"]) ?></h1>
<p><?= nl2br(htmlspecialchars($articolo["testo"])) ?></p>

<h2>Commenti</h2>
<ul>
  <?php foreach ($commenti as $commento): ?>
    <li><?= htmlspecialchars($commento["testo"]) ?></li>
  <?php endforeach; ?>
</ul>

<form method="post">
  <textarea name="testo" placeholder="Scrivi un commento..."></textarea>
  <button type="submit">Invia</button>
</form>

<a href="articoli.php">Torna all'elenco</a>

</body>
</html>
{% endhighlight %}

## Login: login.php e register.php

{% highlight php %}
<?php
// register.php
require __DIR__ . "/includes/db.php";
session_start();

$errori = [];

if ($_SERVER["REQUEST_METHOD"] === "POST") {
    $email = trim($_POST["email"] ?? "");
    $password = $_POST["password"] ?? "";

    if ($email === "" || $password === "") {
        $errori[] = "Email e password sono obbligatorie";
    }

    if (empty($errori)) {
        $pdo = ottieni_connessione();

        // equivalente manuale di "validates :email, uniqueness: true"
        $stmt = $pdo->prepare("SELECT id FROM utenti WHERE email = ?");
        $stmt->execute([$email]);
        if ($stmt->fetch()) {
            $errori[] = "Questa email è già registrata";
        }
    }

    if (empty($errori)) {
        // equivalente manuale di has_secure_password: mai salvare la
        // password in chiaro, si salva solo il suo hash
        $hash = password_hash($password, PASSWORD_BCRYPT);

        $stmt = $pdo->prepare("INSERT INTO utenti (email, password_hash) VALUES (?, ?)");
        $stmt->execute([$email, $hash]);

        $_SESSION["utente_id"] = $pdo->lastInsertId();
        header("Location: articoli.php");
        exit;
    }
}
?>
<!-- form di registrazione, analogo a quello degli articoli -->
{% endhighlight %}

{% highlight php %}
<?php
// login.php
require __DIR__ . "/includes/db.php";
session_start();

$errore = null;

if ($_SERVER["REQUEST_METHOD"] === "POST") {
    $email = trim($_POST["email"] ?? "");
    $password = $_POST["password"] ?? "";

    $pdo = ottieni_connessione();
    $stmt = $pdo->prepare("SELECT * FROM utenti WHERE email = ?");
    $stmt->execute([$email]);
    $utente = $stmt->fetch();

    // equivalente manuale di utente.authenticate(password)
    if ($utente && password_verify($password, $utente["password_hash"])) {
        $_SESSION["utente_id"] = $utente["id"];
        header("Location: articoli.php");
        exit;
    }

    $errore = "Email o password non corretti";
}
?>
<!-- form di login -->
{% endhighlight %}

{% highlight php %}
<?php
// logout.php
session_start();
session_destroy();
header("Location: articoli.php");
exit;
{% endhighlight %}

## Cosa cambia davvero

| Compito | Rails | PHP puro |
|---|---|---|
| Routing | `resources :articoli` in una riga | un file `.php` per ogni pagina |
| Accesso al database | ActiveRecord (`Articolo.all`, `.where`...) | SQL scritto a mano con PDO |
| Validazioni | `validates :titolo, presence: true` | `if` scritti a mano, ripetuti in ogni form |
| Escape HTML automatico | sì, in ogni `<%= %>` | manuale, con `htmlspecialchars` |
| Messaggi flash | `flash[:notice]`, un solo hash gestito da Rails | `$_SESSION`, da svuotare a mano |
| Password sicure | `has_secure_password` (bcrypt integrato) | `password_hash` / `password_verify` (bcrypt di PHP) |
| Struttura MVC | imposta dal framework | da inventare e mantenere da soli |

Il codice PHP funziona, ma si vede chiaramente come, crescendo
l'applicazione (più tabelle, più pagine, più regole di validazione), il
codice ripetuto tenda ad aumentare rapidamente: è esattamente questo il
problema che framework come Rails sono nati per risolvere, offrendo
strutture e convenzioni condivise al posto di soluzioni scritte ogni volta
da zero.

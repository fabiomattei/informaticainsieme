---
title: 'CRUD: leggere i dati (elenco libri e autori)'
date: '2026-08-17T20:05:00+01:00'
author: Fabio Mattei
layout: page
---

Cominciamo dall'operazione più semplice: la **R** di CRUD, Read. In questa pagina scriviamo `index.php`, che mostra l'elenco di tutti i libri insieme al nome del loro autore, e `autore.php`, che mostra il dettaglio di un singolo autore con tutti i suoi libri.

Questa è anche la pagina in cui la **relazione** tra `libri` e `autori` diventa visibile all'utente: senza una JOIN vedremmo solo un numero (`autore_id`), non un nome.

### L'elenco dei libri con il loro autore

`index.php` interroga entrambe le tabelle con una `JOIN`, esattamente come spiegato in [La relazione]({{ site.baseurl }}{% link _database/la-relazione.md %}.html#relazione-1n-uno-a-molti), e stampa il risultato in una tabella HTML.

{% highlight php %}
<?php
require "config.php";

$stmt = $pdo->query("
    SELECT libri.id, libri.titolo, libri.anno, autori.nome, autori.cognome
    FROM libri
    JOIN autori ON libri.autore_id = autori.id
    ORDER BY libri.titolo
");
$libri = $stmt->fetchAll();
?>
<!doctype html>
<html>
<head><title>Biblioteca</title></head>
<body>
    <h1>Elenco libri</h1>
    <p><a href="crea-libro.php">Aggiungi un libro</a> | <a href="crea-autore.php">Aggiungi un autore</a></p>

    <table border="1">
        <tr>
            <th>Titolo</th>
            <th>Anno</th>
            <th>Autore</th>
            <th>Azioni</th>
        </tr>
        <?php foreach ($libri as $libro): ?>
        <tr>
            <td><?= htmlspecialchars($libro["titolo"]) ?></td>
            <td><?= htmlspecialchars($libro["anno"]) ?></td>
            <td><?= htmlspecialchars($libro["nome"] . " " . $libro["cognome"]) ?></td>
            <td>
                <a href="modifica-libro.php?id=<?= $libro["id"] ?>">modifica</a>
                <a href="elimina-libro.php?id=<?= $libro["id"] ?>">elimina</a>
            </td>
        </tr>
        <?php endforeach; ?>
    </table>
</body>
</html>
{% endhighlight %}

Qualche dettaglio importante:

* `$pdo->query(...)` va bene qui perché la query **non contiene nessun valore fornito dall'utente**: non c'è alcun rischio di SQL injection. Quando invece un valore arriva dall'utente (lo vedremo nelle prossime pagine), va sempre usata una query **parametrizzata**.
* `htmlspecialchars()` converte caratteri come `<` e `>` nel loro equivalente HTML (`&lt;`, `&gt;`), evitando che un titolo malizioso contenente codice HTML venga eseguito dal browser: è l'equivalente, sul lato output, della protezione dall'SQL injection sul lato input.
* La sintassi `<?= $variabile ?>` è una scorciatoia per `<?php echo $variabile; ?>`, molto comoda quando si mescola PHP e HTML.

### Il dettaglio di un autore

`autore.php` riceve l'id dell'autore dalla query string (`autore.php?id=1`) e mostra i suoi dati insieme a **tutti** i suoi libri: è il verso "1" della relazione 1:N che guarda verso il verso "N".

{% highlight php %}
<?php
require "config.php";

$id = $_GET["id"];

$stmt = $pdo->prepare("SELECT * FROM autori WHERE id = ?");
$stmt->execute([$id]);
$autore = $stmt->fetch();

if (!$autore) {
    die("Autore non trovato");
}

$stmt = $pdo->prepare("SELECT * FROM libri WHERE autore_id = ? ORDER BY anno");
$stmt->execute([$id]);
$libri = $stmt->fetchAll();
?>
<!doctype html>
<html>
<head><title><?= htmlspecialchars($autore["nome"] . " " . $autore["cognome"]) ?></title></head>
<body>
    <h1><?= htmlspecialchars($autore["nome"] . " " . $autore["cognome"]) ?></h1>

    <h2>Libri scritti</h2>
    <ul>
        <?php foreach ($libri as $libro): ?>
        <li><?= htmlspecialchars($libro["titolo"]) ?> (<?= $libro["anno"] ?>)</li>
        <?php endforeach; ?>
    </ul>

    <p><a href="index.php">Torna all'elenco libri</a></p>
</body>
</html>
{% endhighlight %}

Qui, a differenza di `index.php`, l'id dell'autore arriva dall'utente tramite `$_GET["id"]`: per questo la query **non** viene costruita concatenando `$id` dentro la stringa SQL, ma usa `prepare()` + `execute([$id])`, che tratta `$id` sempre e solo come un dato, mai come codice SQL.

### Esercizi

**Esercizio 1** — Modifica `index.php` in modo che, invece di elencare tutti i libri, elenchi solo quelli pubblicati dopo un certo anno (passato come parametro `?dal=2000` nella URL).

**Esercizio 2** — Scrivi una pagina `autori.php` che mostri l'elenco di tutti gli autori, ciascuno come link verso `autore.php?id=...`.

**Esercizio 3** — Nella pagina `index.php`, aggiungi accanto a ogni autore anche il **numero di libri** che ha scritto, usando `COUNT()` e `GROUP BY` (vedi [Il linguaggio SQL]({{ site.baseurl }}{% link _database/il-linguaggio-sql.md %}.html)).

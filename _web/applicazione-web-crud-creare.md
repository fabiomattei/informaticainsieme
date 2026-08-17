---
title: 'CRUD: creare autori e libri'
date: '2026-08-17T20:10:00+01:00'
author: Fabio Mattei
layout: page
---

Passiamo alla **C** di CRUD: Create. Ogni pagina di creazione è fatta di due parti che vivono nello stesso file PHP:

1. un **form HTML** che mostra i campi da compilare (visualizzato quando la pagina riceve una richiesta GET);
2. il codice che **inserisce i dati nel database** (eseguito quando il form viene inviato con una richiesta POST).

### Creare un autore

`crea-autore.php` è il caso più semplice, perché `autori` non ha nessuna chiave esterna verso l'altra tabella.

{% highlight php %}
<?php
require "config.php";

if ($_SERVER["REQUEST_METHOD"] === "POST") {
    $stmt = $pdo->prepare("INSERT INTO autori (nome, cognome) VALUES (?, ?)");
    $stmt->execute([$_POST["nome"], $_POST["cognome"]]);

    header("Location: index.php");
    exit;
}
?>
<!doctype html>
<html>
<head><title>Nuovo autore</title></head>
<body>
    <h1>Aggiungi un autore</h1>
    <form method="POST">
        Nome: <input type="text" name="nome" required><br>
        Cognome: <input type="text" name="cognome" required><br>
        <input type="submit" value="Salva">
    </form>
</body>
</html>
{% endhighlight %}

Nota come **lo stesso file** gestisca sia la visualizzazione del form sia il salvataggio: `$_SERVER["REQUEST_METHOD"]` ci dice se il browser ha chiesto la pagina normalmente (GET, prima visita) o ha inviato il form (POST). Dopo l'inserimento, `header("Location: index.php")` reindirizza l'utente all'elenco, così un aggiornamento accidentale della pagina (F5) non inserisce l'autore una seconda volta.

### Creare un libro

`crea-libro.php` è leggermente più interessante perché deve gestire la **relazione**: un libro non può esistere senza un autore, quindi il form deve permettere di sceglierne uno tra quelli già presenti nel database, con un menu a tendina (`<select>`).

{% highlight php %}
<?php
require "config.php";

if ($_SERVER["REQUEST_METHOD"] === "POST") {
    $stmt = $pdo->prepare("INSERT INTO libri (titolo, anno, autore_id) VALUES (?, ?, ?)");
    $stmt->execute([$_POST["titolo"], $_POST["anno"], $_POST["autore_id"]]);

    header("Location: index.php");
    exit;
}

$autori = $pdo->query("SELECT id, nome, cognome FROM autori ORDER BY cognome")->fetchAll();
?>
<!doctype html>
<html>
<head><title>Nuovo libro</title></head>
<body>
    <h1>Aggiungi un libro</h1>

    <?php if (count($autori) === 0): ?>
        <p>Non c'è ancora nessun autore. <a href="crea-autore.php">Aggiungine uno</a> prima di creare un libro.</p>
    <?php else: ?>
    <form method="POST">
        Titolo: <input type="text" name="titolo" required><br>
        Anno: <input type="number" name="anno"><br>
        Autore:
        <select name="autore_id">
            <?php foreach ($autori as $autore): ?>
            <option value="<?= $autore["id"] ?>">
                <?= htmlspecialchars($autore["cognome"] . " " . $autore["nome"]) ?>
            </option>
            <?php endforeach; ?>
        </select><br>
        <input type="submit" value="Salva">
    </form>
    <?php endif; ?>
</body>
</html>
{% endhighlight %}

Il punto chiave è questo: il `<select>` mostra `nome` e `cognome` (leggibili da una persona), ma il valore effettivamente inviato al server (`value="<?= $autore["id"] ?>"`) è l'**id numerico**, perché è quello che finisce dentro la colonna `autore_id`, la chiave esterna. L'utente sceglie un nome, ma nel database viene salvato un numero che punta a una riga della tabella `autori`: è esattamente il meccanismo descritto in [La relazione]({{ site.baseurl }}{% link _database/la-relazione.md %}.html).

Se la tabella `autori` fosse vuota, mostrare comunque il form produrrebbe un `<select>` vuoto e, di conseguenza, un libro senza autore valido: per questo il controllo `count($autori) === 0` guida l'utente a creare prima un autore.

### Esercizi

**Esercizio 1** — Aggiungi al form di `crea-libro.php` un controllo che, se il titolo è vuoto, mostri un messaggio di errore invece di inserire una riga nel database (non basarti solo su `required` in HTML, che l'utente può aggirare).

**Esercizio 2** — Modifica `crea-autore.php` in modo che, dopo l'inserimento, l'utente venga reindirizzato non a `index.php` ma direttamente a `crea-libro.php`, per poter aggiungere subito un libro al nuovo autore.

**Esercizio 3** — Cosa succederebbe se in `crea-libro.php` scrivessimo la query così: `"INSERT INTO libri (titolo, anno, autore_id) VALUES ('" . $_POST["titolo"] . "', ...)"`? Motiva la risposta collegandoti a quanto visto in [MySQL: la sicurezza]({{ site.baseurl }}{% link _database/mysql.md %}.html#sicurezza-lsql-injection).

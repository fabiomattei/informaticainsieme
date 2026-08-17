---
title: 'CRUD: modificare un libro'
date: '2026-08-17T11:15:00+01:00'
author: Fabio Mattei
layout: page
---

La **U** di CRUD, Update, assomiglia molto alla creazione vista nella pagina precedente: c'è ancora un form e ancora un inserimento (questa volta con `UPDATE` invece di `INSERT`). La differenza è che il form deve arrivare **già compilato** con i dati attuali del libro, non vuoto.

`modifica-libro.php` riceve l'id del libro da modificare tramite `?id=...` nella URL (lo stesso link scritto in `index.php` nella pagina su [Read]({{ site.baseurl }}{% link _web/applicazione-web-crud-leggere.md %}.html)).

{% highlight php %}
<?php
require "config.php";

$id = $_GET["id"] ?? $_POST["id"];

if ($_SERVER["REQUEST_METHOD"] === "POST") {
    $stmt = $pdo->prepare("UPDATE libri SET titolo = ?, anno = ?, autore_id = ? WHERE id = ?");
    $stmt->execute([$_POST["titolo"], $_POST["anno"], $_POST["autore_id"], $id]);

    header("Location: index.php");
    exit;
}

$stmt = $pdo->prepare("SELECT * FROM libri WHERE id = ?");
$stmt->execute([$id]);
$libro = $stmt->fetch();

if (!$libro) {
    die("Libro non trovato");
}

$autori = $pdo->query("SELECT id, nome, cognome FROM autori ORDER BY cognome")->fetchAll();
?>
<!doctype html>
<html>
<head><title>Modifica libro</title></head>
<body>
    <h1>Modifica libro</h1>
    <form method="POST">
        <input type="hidden" name="id" value="<?= $libro["id"] ?>">

        Titolo: <input type="text" name="titolo" value="<?= htmlspecialchars($libro["titolo"]) ?>" required><br>
        Anno: <input type="number" name="anno" value="<?= htmlspecialchars($libro["anno"]) ?>"><br>
        Autore:
        <select name="autore_id">
            <?php foreach ($autori as $autore): ?>
            <option value="<?= $autore["id"] ?>" <?= $autore["id"] == $libro["autore_id"] ? "selected" : "" ?>>
                <?= htmlspecialchars($autore["cognome"] . " " . $autore["nome"]) ?>
            </option>
            <?php endforeach; ?>
        </select><br>

        <input type="submit" value="Salva modifiche">
    </form>
</body>
</html>
{% endhighlight %}

Tre punti da notare rispetto a `crea-libro.php`:

* **L'id viaggia due volte**: la prima volta arriva via GET quando si clicca "modifica" nell'elenco, la seconda volta deve viaggiare di nuovo, dentro il form stesso, tramite un campo nascosto (`<input type="hidden" name="id" ...>`), perché quando il browser invia il form in POST la query string originale (`?id=...`) non viene più letta da `$_GET`. La riga `$id = $_GET["id"] ?? $_POST["id"];` gestisce entrambi i casi: prende l'id da `$_GET` se presente (prima visita), altrimenti da `$_POST` (dopo l'invio del form).
* **I campi del form sono precompilati** con `value="<?= ... ?>"`, leggendo i valori attuali dal database con una `SELECT` prima di generare l'HTML.
* **Il `<select>` ricorda l'autore attuale** grazie al confronto `$autore["id"] == $libro["autore_id"]`, che aggiunge l'attributo `selected` all'opzione giusta: senza questo confronto, il menu a tendina mostrerebbe sempre il primo autore della lista, anche se il libro appartiene a un altro.

### Esercizi

**Esercizio 1** — Scrivi `modifica-autore.php`, seguendo lo stesso schema, per modificare nome e cognome di un autore esistente.

**Esercizio 2** — Cosa succede, con il codice di questa pagina, se un utente visita `modifica-libro.php?id=999` e quel libro non esiste? Il controllo `if (!$libro)` è già presente: prova a rimuoverlo temporaneamente e osserva quale errore PHP genera al suo posto.

**Esercizio 3** — Aggiungi un link "Annulla" nel form che riporti a `index.php` senza salvare nulla.

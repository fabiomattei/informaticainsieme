---
title: 'CRUD: eliminare libri e autori'
date: '2026-08-17T20:20:00+01:00'
author: Fabio Mattei
layout: page
---

Chiudiamo con la **D** di CRUD: Delete. È l'operazione più semplice dal punto di vista del codice, ma è anche quella in cui la **relazione** tra `libri` e `autori` si fa sentire più chiaramente, perché il database si rifiuta di lasciar rompere il legame tra le due tabelle senza permesso esplicito.

### Eliminare un libro

Cancellare un libro è semplice: nessun'altra riga del database dipende da un libro, quindi basta una `DELETE`.

{% highlight php %}
<?php
require "config.php";

$id = $_GET["id"];

$stmt = $pdo->prepare("DELETE FROM libri WHERE id = ?");
$stmt->execute([$id]);

header("Location: index.php");
exit;
{% endhighlight %}

Notiamo che, anche qui, l'id arriva dall'utente tramite `$_GET["id"]` e viene passato con `prepare()` + `execute([$id])`, mai concatenato dentro la stringa SQL.

In una applicazione reale sarebbe opportuno chiedere conferma prima di cancellare (ad esempio con una pagina intermedia "sei sicuro?"): lo lasciamo come esercizio.

### Eliminare un autore

Qui la cosa si complica. Ricordiamo lo schema della tabella `libri` definito nella pagina sulla [progettazione]({{ site.baseurl }}{% link _web/applicazione-web-crud-progettazione.md %}.html):

{% highlight sql %}
FOREIGN KEY (autore_id) REFERENCES autori(id) ON DELETE RESTRICT
{% endhighlight %}

`ON DELETE RESTRICT` dice esplicitamente a MySQL: **non permettere** la cancellazione di una riga di `autori` se esiste ancora almeno una riga di `libri` che la referenzia. Se proviamo comunque a farlo, MySQL non cancella nulla e restituisce un errore di violazione del vincolo di integrità referenziale. Il codice PHP deve intercettare questo errore invece di lasciar "esplodere" l'applicazione:

{% highlight php %}
<?php
require "config.php";

$id = $_GET["id"];

try {
    $stmt = $pdo->prepare("DELETE FROM autori WHERE id = ?");
    $stmt->execute([$id]);

    header("Location: index.php");
    exit;
} catch (PDOException $e) {
    $errore = "Impossibile eliminare l'autore: ha ancora dei libri collegati. Elimina prima quelli.";
}
?>
<!doctype html>
<html>
<head><title>Errore</title></head>
<body>
    <p><?= htmlspecialchars($errore) ?></p>
    <p><a href="autore.php?id=<?= $id ?>">Torna alla scheda dell'autore</a></p>
</body>
</html>
{% endhighlight %}

Questo comportamento non è un difetto dell'applicazione: è **il database che fa esattamente il suo lavoro**. Se lasciassimo cancellare un autore che ha ancora libri, quei libri finirebbero con un `autore_id` che punta a una riga ormai inesistente — dati orfani, esattamente il problema che le chiavi esterne servono a evitare (vedi [La relazione]({{ site.baseurl }}{% link _database/la-relazione.md %}.html)).

### L'alternativa: ON DELETE CASCADE

In fase di progettazione avremmo potuto scegliere una politica diversa, `ON DELETE CASCADE` invece di `RESTRICT`:

{% highlight sql %}
FOREIGN KEY (autore_id) REFERENCES autori(id) ON DELETE CASCADE
{% endhighlight %}

Con `CASCADE`, cancellare un autore cancellerebbe **automaticamente** anche tutti i suoi libri, senza bisogno di alcun controllo lato PHP. È comodo, ma pericoloso: un singolo click su "elimina autore" farebbe sparire silenziosamente un numero imprecisato di libri. Per una biblioteca, `RESTRICT` (che costringe l'utente a decidere esplicitamente cosa fare dei libri, uno per uno) è quasi sempre la scelta più sicura; `CASCADE` è più adatto a relazioni dove le righe "figlie" non hanno senso di esistere senza il "genitore" (ad esempio le righe di un ordine quando si cancella l'intero ordine).

### Esercizi

**Esercizio 1** — Aggiungi in `index.php` un link "elimina" accanto a ogni autore in `autori.php` (creata come esercizio nella pagina sulla lettura), che punti a `elimina-autore.php?id=...`. Prova a cancellare un autore che ha ancora libri e verifica che compaia il messaggio di errore.

**Esercizio 2** — Modifica `elimina-libro.php` per mostrare, prima di cancellare davvero, una pagina di conferma con il titolo del libro e due link: "conferma" (che esegue la DELETE) e "annulla" (che torna a `index.php`).

**Esercizio 3** — Riprova l'esercizio 4 di [La relazione]({{ site.baseurl }}{% link _database/la-relazione.md %}.html#esercizi) (libro/casa editrice, nazione/capitale, attore/film, cittadino/codice fiscale) e per ciascun caso stabilisci se useresti `ON DELETE CASCADE` o `ON DELETE RESTRICT`, motivando la scelta.

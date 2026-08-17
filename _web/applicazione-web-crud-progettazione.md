---
title: 'Una applicazione web CRUD: progettiamo libri e autori'
date: '2026-08-17T11:00:00+01:00'
author: Fabio Mattei
layout: page
---

Fino ad ora abbiamo visto separatamente come costruire pagine con [HTML e CSS]({{ site.baseurl }}{% link _web/html.md %}.html), come programmare in [PHP]({{ site.baseurl }}{% link paginemenu/php.md %}), e come funziona un database relazionale come [MySQL]({{ site.baseurl }}{% link _database/mysql.md %}.html). In questa piccola serie di pagine mettiamo insieme questi tre pezzi per costruire una vera applicazione web: una **applicazione CRUD**.

**CRUD** è un acronimo che riassume le quattro operazioni che (quasi) ogni applicazione che gestisce dati deve saper fare:

* **C**reate — creare un nuovo dato (INSERT)
* **R**ead — leggere i dati esistenti (SELECT)
* **U**pdate — modificare un dato esistente (UPDATE)
* **D**elete — cancellare un dato (DELETE)

Se guardi con attenzione, queste sono esattamente le quattro istruzioni SQL principali viste in [Il linguaggio SQL]({{ site.baseurl }}{% link _database/il-linguaggio-sql.md %}.html). Una applicazione CRUD, in fondo, è solo un'interfaccia web (fatta di pagine HTML e form) che permette a un utente di eseguire queste quattro operazioni senza dover scrivere SQL a mano.

### Il progetto: una mini biblioteca

Per tenere le cose essenziali ma complete, costruiamo una piccola applicazione che gestisce una biblioteca con due tabelle collegate da una **relazione**, esattamente come descritto in [La relazione]({{ site.baseurl }}{% link _database/la-relazione.md %}.html):

* **autori** — chi scrive i libri;
* **libri** — i libri, ognuno scritto da un autore.

Un autore può aver scritto più libri, ma ogni libro ha un solo autore: è una relazione **1:N**, e la chiave esterna va quindi nella tabella dal lato "N" (`libri`).

![Schema della mini biblioteca: la tabella libri contiene autore_id, chiave esterna che punta a id nella tabella autori](/images/web/crud-libri-autori/schema.svg){:class="aside-image"}

{% highlight sql %}
CREATE TABLE autori (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(100) NOT NULL,
    cognome VARCHAR(100) NOT NULL
);

CREATE TABLE libri (
    id INT AUTO_INCREMENT PRIMARY KEY,
    titolo VARCHAR(200) NOT NULL,
    anno INT,
    autore_id INT NOT NULL,
    FOREIGN KEY (autore_id) REFERENCES autori(id) ON DELETE RESTRICT
);
{% endhighlight %}

`ON DELETE RESTRICT` dice a MySQL di **rifiutare** la cancellazione di un autore finché esistono ancora suoi libri: torneremo su questo dettaglio nella pagina sulla cancellazione, perché è una diretta conseguenza del fatto che `autore_id` è una chiave esterna.

### La connessione al database

Tutte le pagine PHP dell'applicazione avranno bisogno di parlare con MySQL, quindi conviene scrivere la connessione **una sola volta**, in un file a parte chiamato `config.php`, e richiamarlo ovunque serva con `require`. Usiamo **PDO** (PHP Data Objects), che ci permette di scrivere query parametrizzate in modo semplice e sicuro, come già raccomandato in [MySQL: la sicurezza]({{ site.baseurl }}{% link _database/mysql.md %}.html#sicurezza-lsql-injection).

{% highlight php %}
<?php
// config.php
$host = "localhost";
$dbname = "biblioteca";
$user = "root";
$password = "";

try {
    $pdo = new PDO("mysql:host=$host;dbname=$dbname;charset=utf8mb4", $user, $password);
    $pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
} catch (PDOException $e) {
    die("Connessione al database fallita: " . $e->getMessage());
}
{% endhighlight %}

`PDO::ATTR_ERRMODE` impostato su `ERRMODE_EXCEPTION` fa sì che ogni errore SQL (una tabella che non esiste, un vincolo violato...) lanci un'eccezione PHP invece di fallire silenziosamente: è molto più facile da individuare durante lo sviluppo.

### I file dell'applicazione

Nelle prossime pagine costruiremo, un pezzo alla volta, i file che compongono l'applicazione:

| File | Operazione CRUD | Cosa fa |
|---|---|---|
| `config.php` | — | connessione al database (appena visto) |
| `index.php` | Read | elenco dei libri con il nome del loro autore |
| `autore.php` | Read | dettaglio di un autore e dei suoi libri |
| `crea-autore.php` | Create | form e inserimento di un nuovo autore |
| `crea-libro.php` | Create | form e inserimento di un nuovo libro |
| `modifica-libro.php` | Update | modifica dei dati di un libro esistente |
| `elimina-libro.php` | Delete | cancellazione di un libro |
| `elimina-autore.php` | Delete | cancellazione di un autore (e gestione dei suoi libri) |

### Esercizi

**Esercizio 1** — Prima di scrivere una riga di codice, disegna (anche su carta) lo schema entità-relazione di questa mini biblioteca, seguendo la notazione vista in [Il modello entità relazione]({{ site.baseurl }}{% link _database/modello-entita-relazione.md %}.html).

**Esercizio 2** — Immagina di voler aggiungere anche il concetto di **casa editrice**. Una casa editrice pubblica molti libri, un libro ha una sola casa editrice. Che tipo di relazione è? Dove va inserita la nuova chiave esterna?

**Esercizio 3** — Scrivi il comando `CREATE DATABASE biblioteca;` e poi esegui le due `CREATE TABLE` di questa pagina dentro MySQL (con phpMyAdmin oppure da riga di comando, come spiegato in [MySQL]({{ site.baseurl }}{% link _database/mysql.md %}.html)).

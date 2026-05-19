---
title: 'Il linguaggio SQL'
date: '2020-02-10T06:22:07+01:00'
author: Fabio Mattei
layout: page
---

Il linguaggio SQL permette al database administrator di operare sui database. Il linguaggio è di per sé molto semplice e conta pochi comandi. Le possibili combinazioni di comandi danno però luogo ad una ricchissima potenzialità espressiva.

#### CREATE TABLE 

Le **entità** e le **relazioni** si concretizzano in tabelle e campi contenute all’interno di un database. Il comando create table consente di creare una tabella in un database. Sintassi:


{% highlight sql %}
CREATE TABLE nome_tabella 
(<nome_colonna><tipo_dato>[attributi],
<nome_colonna><tipo_dato>[attributi],...);
{% endhighlight %}

Gli attributi sono di inserimento facoltativo e i più frequentemente usati sono:

* PRIMARY KEY, che rende il campo la chiave primaria della tabella (quella che occorre per instaurare relazioni con altre tabelle);
* NOT NULL, che rende il campo obbligatoriamente destinato a contenere un dato, contrariamente a quella che sarebbe l’impostazione di default;
* AUTOINCREMENT, destinato solo a campi contenenti un numero per fare in modo che il contenuto del campo si incrementi automaticamente di una unità a partire dal più grande valore già presente. Attributo molto utile per la primary key.

Nel comando per creare una tabella abbiamo visto che occorre indicare, per ciascuna colonna destinata a scandire i campi dei record del nostro database, il tipo del dato che vi può essere inserito.

I tipi di dato variano da un sistema di database all'altro. Database come MySQL e PostgreSQL offrono decine di tipi (VARCHAR, INT, BOOLEAN, DATE, DECIMAL, ecc.). **SQLite**, che utilizzeremo negli esempi pratici, semplifica tutto a soli cinque tipi:

* INTEGER: numero intero
* REAL: numero reale o a virgola mobile
* TEXT: testo
* BLOB: informazione in forma binaria (immagine contenuta in un file)
* NULL: rappresenta un dato mancante

Esempio:

{% highlight sql %}
CREATE TABLE alunni (
id INTEGER PRIMARY KEY AUTOINCREMENT,
nome TEXT,
cognome TEXT);
{% endhighlight %}

#### DROP TABLE 

Il comando consente di cancellare una tabella dal database.


{% highlight sql %}
DROP TABLE nome_tabella;
{% endhighlight %}

Esempio:


{% highlight sql %}
DROP TABLE alunni;
{% endhighlight %}

#### INSERT 

Il comando consente di inserire dati in una tabella esistente

{% highlight sql %}
INSERT INTO nome_tabella
(colonna1, colonna2, colonna3) 
VALUES (valore1, valore2, valore3);
{% endhighlight %}

Attenzione: i valori da inserire vanno elencati nello stesso ordine delle colonne. I valori numerici si inseriscono senza particolari accortezze, mentre i valori di tipo stringa vanno racchiusi tra semplici apici ‘ oppure doppi apici “.

Esempio:


{% highlight sql %}
INSERT INTO alunni 
(nome, cognome) 
VALUES ("Annalisa", "De Pamphilis");
{% endhighlight %}

A questo punto la tabella alunni conterrà una tupla avente id = 1, il campo nome conterrà la stringa “Annalisa” e il campo cognome conterrà la stringa “De Pamphilis”. Dato che l’id non è stato specificato all’interno del comando INSERT e che questo è stato inizializzato come autoincrement questo viene posto automaticamente a 1.

| id | nome | cognome |
|---|---|---|
| 1 | Annalisa | De Pamphilis |

#### UPDATE 

Il comando consente di modificare i dati già presenti in una tabella.

{% highlight sql %}
UPDATE nome_tabella
SET colonna1=valore1, colonna2=valore2, ... 
WHERE predicato;
{% endhighlight %}

I valori numerici si inseriscono senza particolari accortezze, mentre i valori di tipo stringa vanno racchiusi tra semplici apici ’ oppure doppi apici “.

Esempio:

{% highlight sql %}
UPDATE alunni SET nome = "Annabella" WHERE id = 1;
{% endhighlight %}

#### DELETE 

Il comando consente di cancellare i dati da una tabella


{% highlight sql %}
DELETE FROM nome_tabella WHERE predicato;
{% endhighlight %}

I valori numerici si inseriscono senza particolari accortezze, mentre i valori di tipo stringa vanno racchiusi tra semplici apici ’ oppure doppi apici “.

Esempio:

{% highlight sql %}
DELETE FROM alunni WHERE id = 1;
{% endhighlight %}

#### SELECT 

Il comando consente di andare a leggere i dati dalle tabelle del database


{% highlight sql %}
SELECT colonna1, colonna2, ... 
FROM nome_tabella
WHERE predicato;
{% endhighlight %}

Questo comando permette di reperire dati da una o più tabelle.

* dopo SELECT va inserito l’elenco dei campi richiesti, separati da virgola. Per selezionare tutte le colonne si usa l’asterisco (`SELECT *`). In caso di ambiguità tra campi omonimi il campo va indicato nella forma `tabella.nome_colonna`;
* dopo FROM va inserito l’elenco delle tabelle coinvolte nella ricerca. Possono essere coinvolte perché contengono campi con i contenuti che cerchiamo o perché partecipano nei predicati che ci consentono di filtrare i dati;
* dopo WHERE, separate eventualmente da AND o da OR, si indicano i predicati che utilizziamo per selezionare tra i dati contenuti nelle tabelle soltanto quelli che ci occorrono. Anche in questo caso, in caso di ambiguità (due campi con lo stesso nome) il campo va indicato non semplicemente come nome\_colonna ma nella forma più completa tabella.nome\_colonna;

 I predicati possono essere:

* di confronto: =, &lt;&gt;, &lt;=, &gt;=, &lt;, &gt;
* pattern matching (confronto di schema) tra stringhe: LIKE seguita da caratteri jolly \_ per un solo carattere, % per uno o più caratteri.

I predicati si combinano tra loro utilizzando gli operatori logici AND, OR, NOT.

Esempio di uso di AND:

{% highlight sql %}
SELECT colonna1, colonna2
FROM nome_tabella
WHERE predicato1 AND predicato2 AND predicato3;
{% endhighlight %}

Esempio di uso di OR:

{% highlight sql %}
SELECT colonna1, colonna2
FROM nome_tabella
WHERE predicato1 OR predicato2 OR predicato3;
{% endhighlight %}

Esempio di uso di NOT:

{% highlight sql %}
SELECT colonna1, colonna2
FROM nome_tabella
WHERE NOT predicato;
{% endhighlight %}

#### ORDER BY e DISTINCT

La clausola **ORDER BY** ordina i risultati della query. L’ordinamento è crescente per default (ASC); si usa DESC per l’ordinamento decrescente.

{% highlight sql %}
SELECT colonna1, colonna2
FROM nome_tabella
ORDER BY colonna1 ASC;
{% endhighlight %}

Esempio: elencare gli alunni in ordine alfabetico per cognome:

{% highlight sql %}
SELECT nome, cognome FROM alunni ORDER BY cognome ASC;
{% endhighlight %}

La parola chiave **DISTINCT** elimina i duplicati dal risultato:

{% highlight sql %}
SELECT DISTINCT colonna1 FROM nome_tabella;
{% endhighlight %}

Esempio: ottenere l’elenco delle nazionalità distinte dei film presenti nel database:

{% highlight sql %}
SELECT DISTINCT Nazionalità FROM film;
{% endhighlight %}

#### Funzioni aggregate

Le funzioni aggregate operano su un insieme di righe e restituiscono un unico valore riepilogativo.

| Funzione | Significato |
|---|---|
| COUNT(*) | conta il numero di righe |
| SUM(colonna) | somma i valori di una colonna numerica |
| AVG(colonna) | calcola la media dei valori |
| MIN(colonna) | restituisce il valore minimo |
| MAX(colonna) | restituisce il valore massimo |

Esempi:

{% highlight sql %}
SELECT COUNT(*) FROM film;
SELECT SUM(Incasso) FROM proiezioni;
SELECT AVG(Incasso) FROM proiezioni WHERE IdSala = 1;
{% endhighlight %}

#### GROUP BY e HAVING

**GROUP BY** raggruppa le righe che condividono lo stesso valore in una o più colonne, permettendo di applicare le funzioni aggregate a ciascun gruppo separatamente.

{% highlight sql %}
SELECT colonna, COUNT(*)
FROM nome_tabella
GROUP BY colonna;
{% endhighlight %}

Esempio: contare quanti film ci sono per ciascuna nazionalità:

{% highlight sql %}
SELECT Nazionalità, COUNT(*)
FROM film
GROUP BY Nazionalità;
{% endhighlight %}

**HAVING** filtra i gruppi prodotti da GROUP BY. La differenza rispetto a WHERE è che WHERE filtra le singole righe *prima* del raggruppamento, HAVING filtra i gruppi *dopo*.

{% highlight sql %}
SELECT Nazionalità, COUNT(*)
FROM film
GROUP BY Nazionalità
HAVING COUNT(*) > 2;
{% endhighlight %}

Questo esempio restituisce solo le nazionalità con più di 2 film.

#### L’istruzione JOIN in SQL 

L’istruzione JOIN combina tuple di due o più tabelle basandosi su una colonna che le collega logicamente. Esistono due varianti principali.

**INNER JOIN** (o semplicemente JOIN) — restituisce solo le righe che hanno una corrispondenza in entrambe le tabelle. Le righe senza corrispondenza vengono escluse dal risultato.

**LEFT JOIN** — restituisce tutte le righe della tabella di sinistra, anche quelle senza corrispondenza nella tabella di destra. Le colonne mancanti della tabella di destra vengono riempite con NULL.

Vediamo un esempio con le seguenti tabelle:

**Tabella Clienti**

| IdCliente | NomeCliente | Nazione |
|---|---|---|
| 1 | Carlo Pedersoli | Germania |
| 2 | Mario Girotti | Italia |
| 3 | Sergio Leone | Messico |
| 4 | Luigi Diberti | Italia |

**Tabella Ordini**

| IdOrdine | IdCliente | DataOrdine |
|---|---|---|
| 10308 | 2 | 12/04/2019 |
| 10309 | 1 | 13/04/2019 |
| 10310 | 3 | 18/04/2019 |

Con **INNER JOIN** il cliente 4 (Luigi Diberti) non compare perché non ha ordini:

{% highlight sql %}
SELECT Clienti.NomeCliente, Ordini.IdOrdine, Ordini.DataOrdine
FROM Ordini
JOIN Clienti ON Ordini.IdCliente = Clienti.IdCliente;
{% endhighlight %}

| NomeCliente | IdOrdine | DataOrdine |
|---|---|---|
| Mario Girotti | 10308 | 12/04/2019 |
| Carlo Pedersoli | 10309 | 13/04/2019 |
| Sergio Leone | 10310 | 18/04/2019 |

Con **LEFT JOIN** tutti i clienti compaiono, e chi non ha ordini mostra NULL:

{% highlight sql %}
SELECT Clienti.NomeCliente, Ordini.IdOrdine
FROM Clienti
LEFT JOIN Ordini ON Clienti.IdCliente = Ordini.IdCliente;
{% endhighlight %}

| NomeCliente | IdOrdine |
|---|---|
| Carlo Pedersoli | 10309 |
| Mario Girotti | 10308 |
| Sergio Leone | 10310 |
| Luigi Diberti | NULL |

### Esercizi

**Esercizio 1:**

Schema relazionale:

ATTORI (IdAttore, Nome, AnnoNascita, Nazionalità);  
RECITA (IdAttore\*, IdFilm\*)  
FILM (IdFilm, Titolo, AnnoProduzione, Nazionalità, Regista, Genere)   
PROIEZIONI (IdProiezione, IdFilm\*, IdSala\*, Incasso, DataProiezione)   
SALE (IdSala, Posti, Nome, Città)

Scrivi le seguenti query SQL:

* Inserisci nella tabella attori: “Terence Hill”, 1939, “Italiana”; “Margot Robbie”, 1990, “Australiana”
* Inserisci il film: “Lo chiamavano Trinità”, 1970, “Italiana”, “Enzo Barboni”
* Inserisci la relazione tra Terence Hill e Lo chiamavano Trinità
* Inserisci le sale: 120, “Cinema Italia”, “Castel di Sangro”; 90, “Igioland”, “Corfinio”
* Inserisci la proiezione del film “Lo chiamavano Trinità” di ieri a Castel di Sangro, incasso 1000€ del 05/02/2020

**Esercizio 2**

Basandoti sul relazionale dell’esercizio uno, scrivi le seguenti query SQL:

* Correggi la proiezione di ieri fatta a Castel di Sangro: l’incasso è stato di 1200€ e non di 1000€
* Correggi la capienza della sala del cinema di Corfinio: può contenere 96 persone e non 90
* Inserisci il film “Birds of Prey”, 2020, “USA”, “Cathy Yan” in cui recita Margot Robbie
* Inserisci la relazione tra Margot Robbie e Birds of Prey
* Visualizza con una SELECT tutti gli attori presenti nel database

**Esercizio 3**

Basandoti sul relazionale dell'esercizio uno, scrivi le seguenti query SQL:

* Visualizza tutti i film in ordine alfabetico per Titolo
* Visualizza le nazionalità distinte dei film presenti nel database
* Conta quanti film sono presenti nel database
* Calcola l'incasso totale di tutte le proiezioni
* Trova l'incasso massimo registrato tra tutte le proiezioni
* Visualizza quanti film ci sono per ciascuna nazionalità
* Visualizza solo le nazionalità che hanno più di un film
* Visualizza il nome di ciascun attore insieme ai titoli dei film in cui recita (usa JOIN)

**Esercizio 4 — CREATE TABLE**

Una biblioteca vuole gestire i propri libri e i prestiti agli utenti. Lo schema è il seguente:

LIBRI (IdLibro, Titolo, Autore, Genere, AnnoPubblicazione)  
UTENTI (IdUtente, Nome, Cognome, Email)  
PRESTITI (IdPrestito, IdLibro\*, IdUtente\*, DataPrestito, DataRestituzione)

Scrivi le seguenti query SQL:

* Scrivi i tre comandi CREATE TABLE per creare lo schema (usa INTEGER PRIMARY KEY AUTOINCREMENT per gli id, TEXT per i campi stringa)
* Inserisci 3 libri a scelta nella tabella libri
* Inserisci 2 utenti nella tabella utenti
* Registra il prestito di un libro a un utente con data di oggi
* Registra la restituzione di quel libro aggiornando DataRestituzione
* Cancella uno degli utenti inseriti

**Esercizio 5 — SELECT con predicati complessi**

Basandoti sul relazionale dell'esercizio 1 (cinema), scrivi le seguenti query SQL:

* Visualizza i film il cui titolo contiene la parola "chiamavano"
* Visualizza i film prodotti prima del 1980 ordinati per anno di produzione crescente
* Visualizza gli attori nati dopo il 1970 di nazionalità italiana o australiana
* Visualizza i film il cui regista ha un cognome che inizia con la lettera B
* Visualizza le proiezioni con incasso superiore a 1000€ ordinate per incasso decrescente
* Visualizza i film di genere "Western" prodotti in Italia

**Esercizio 6 — Query avanzate**

Basandoti sul relazionale dell'esercizio 1 (cinema), scrivi le seguenti query SQL:

* Per ogni sala visualizza il nome e l'incasso totale di tutte le sue proiezioni, ordinato per incasso totale decrescente
* Visualizza i registi che hanno diretto più di un film, con il numero di film per ciascuno
* Visualizza il titolo di ciascun film e il numero di proiezioni effettuate; includi anche i film mai proiettati (usa LEFT JOIN)
* Visualizza le città in cui la media degli incassi delle proiezioni supera 500€
* Visualizza il nome di ciascun attore e il numero di film in cui recita, ordinato per numero di film decrescente; includi gli attori che non recitano in nessun film (usa LEFT JOIN)

**Esercizio 7 — Schema voti scolastici**

Si vuole gestire i voti degli studenti di una scuola. Lo schema è il seguente:

STUDENTI (IdStudente, Nome, Cognome, Classe, AnnoNascita)  
MATERIE (IdMateria, Nome, Docente)  
VOTI (IdVoto, IdStudente\*, IdMateria\*, Voto, Data)

Scrivi le seguenti query SQL:

* Scrivi i tre comandi CREATE TABLE (usa REAL per il campo Voto)
* Inserisci 4 studenti distribuiti in due classi diverse
* Inserisci 3 materie con i rispettivi docenti
* Inserisci almeno 6 voti distribuiti tra studenti e materie
* Visualizza nome, cognome e media dei voti di ciascuno studente
* Visualizza il nome di ciascuna materia e la media dei voti ottenuti, ordinata per media decrescente
* Visualizza nome e cognome degli studenti con media superiore a 7
* Visualizza, per ciascuna materia, il voto massimo e il voto minimo ottenuti
* Visualizza gli studenti che non hanno ancora ricevuto nessun voto (usa LEFT JOIN)

**Esercizio 8 — Schema discografia**

Si vuole gestire una collezione musicale. Lo schema è il seguente:

ARTISTI (IdArtista, Nome, Genere, NazioneOrigine)  
ALBUM (IdAlbum, Titolo, Anno, IdArtista\*)  
BRANI (IdBrano, Titolo, DurataSecondi, IdAlbum\*)

Scrivi le seguenti query SQL:

* Scrivi i tre comandi CREATE TABLE
* Inserisci 3 artisti, 4 album e almeno 10 brani distribuiti tra gli album
* Visualizza il titolo di tutti i brani di un artista a scelta (JOIN su tre tabelle: BRANI → ALBUM → ARTISTI)
* Visualizza il titolo di ciascun album con il numero di brani che contiene
* Visualizza gli album che contengono più di 3 brani
* Visualizza la durata totale in secondi di ciascun album, ordinata dalla più lunga alla più corta
* Visualizza gli artisti che hanno pubblicato più di un album, con il numero di album per ciascuno
* Visualizza gli artisti che non hanno ancora pubblicato nessun album (usa LEFT JOIN)

**Esercizio 9 — Il grande esercizio**

Basandoti sul relazionale dell'esercizio 1 (cinema), scrivi query che combinano più clausole contemporaneamente:

* Per ciascun genere di film visualizza il genere e il numero di attori distinti che vi recitano; escludi i generi con meno di 2 attori (JOIN su tre tabelle FILM → RECITA → ATTORI, GROUP BY Genere, HAVING)
* Visualizza il nome di ciascuna città con il numero di proiezioni e l'incasso medio; mostra solo le città con almeno 2 proiezioni e incasso medio superiore a 800€, ordinate per incasso medio decrescente (JOIN SALE → PROIEZIONI, GROUP BY Città)
* Visualizza il titolo dei film proiettati più di una volta con il numero totale di proiezioni e l'incasso complessivo, ordinati per incasso complessivo decrescente
* Per ciascun regista visualizza il nome e la somma degli incassi di tutte le proiezioni dei suoi film; includi i registi i cui film non sono mai stati proiettati mostrando 0 come incasso (LEFT JOIN su FILM → PROIEZIONI, GROUP BY Regista)

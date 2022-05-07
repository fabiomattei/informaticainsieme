---
title: 'Il linguaggio SQL'
date: '2020-02-10T06:22:07+01:00'
author: fabio
layout: page
---

Il linguaggio SQL permette al database administrator di operare sui database. Il linguaggio è di per se molto semplice e conta pochi comandi. Le possibili combinazioni di comandi danno però luogo ad una ricchissima potenzialità espressiva.

#### CREATE TABLE 

Le **entità** e le **relazioni** si concretizzano in tabelle e campi contenute all’interno di un database. Il comando create table consente di creare una tabella in un database. Sintassi:


{% highlight sql %}
CREATE TABLE nome_tabella 
(<nome_colonna><tipo_dato>[attributi],
<nome_colonna><tipo_dato>[attributi],...);
{% endhighlight %}

</div>Gli attributi sono di inserimento facoltativo e i più frequentemente usati sono:

- PRIMARY KEY, che rende il campo la chiave primaria della tabella (quella che occorre per instaurare relazioni con altre tabelle);
- NOT NULL, che rende il campo obbligatoriamente destinato a contenere un dato, contrariamente a quella che sarebbe l’impostazione di default;
- AUTOINCREMENT, destinato solo a campi contenenti un numero per fare in modo che il contenuto del campo si incrementi automaticamente di una unità a partire dal più grande valore già presente. Attributo molto utile per la primary key.

Nel comando per creare una tabella abbiamo visto che occorre indicare, per ciascuna colonna destinata a scandire i campi dei record del nostro database, il tipo del dato che vi può essere inserito. In SQLite i tipi di dato gestiti sono solo cinque:

- INTEGER: numero intero
- REAL: numero reale o a virgola mobile
- TEXT: testo
- BLOB: informazione in forma binaria (immagine contenuta in un file)
- NULL: rappresenta un dato mancante

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

</div>Esempio:


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

</div>Attenzione: i valori da inserire vanno elencati nello stesso ordine delle colonne. I valori numerici si inseriscono senza particolari accortezze, mentre i valori di tipo stringa vanno racchiusi tra semplici apici ’ oppure doppi apici “.

Esempio:


{% highlight sql %}
INSERT INTO alunni 
(nome, cognome) 
VALUES ("Annalisa", "De Pamphilis");
{% endhighlight %}

</div>A questo punto la tabella alunni conterrà una tupla avente id = 1, il campo nome conterrà la stringa “Annalisa” e il campo cognome conterrà la stringa “De Pamphilis”. Dato che l’id non è stato specificato all’interno del comando INSERT e che questo è stato inizializzato come autoincrement questo viene posto automaticamente a 1.

<figure class="wp-block-table">| id | nome | cognome |
|---|---|---|
| 1 | Annalisa | De Pamphilis |

</figure>#### UPDATE 

{% highlight sql %}
UPDATE nome_tabella
SET colonna1=valore1, colonna2=valore2, ... 
WHERE predicato;
{% endhighlight %}

</div>I valori numerici si inseriscono senza particolari accortezze, mentre i valori di tipo stringa vanno racchiusi tra semplici apici ’ oppure doppi apici “.

Esempio:

{% highlight sql %}
UPDATE alunni SET nome = "Annabella" WHERE id = 1;
{% endhighlight %}

#### DELETE 

Il comando consente di cancellare i dati da una tabella


{% highlight sql %}
DELETE FROM nome_tabella WHERE predicato;
{% endhighlight %}

</div>I valori numerici si inseriscono senza particolari accortezze, mentre i valori di tipo stringa vanno racchiusi tra semplici apici ’ oppure doppi apici “.

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

</div>Questo comando permette di reperire dati da una o più tabelle.

- dopo SELECT va inserito, attraverso il nome di colonna, l’elenco dei campi richiesti dalla ricerca (in gergo la query), separati da virgola e nell’ordine in cui si vogliono ricevere; in caso di ambiguità (due campi con lo stesso nome) il campo va indicato non semplicemente come nome\_colonna ma nella forma più completa tabella.nome\_colonna;
- dopo FROM va inserito l’elenco delle tabelle coinvolte nella ricerca. Possono essere coinvolte perché contengono campi con i contenuti che cerchiamo o perché partecipano nei predicati che ci consentono di filtrare i dati;
- dopo WHERE, separate eventualmente da AND o da OR, si indicano i predicati che utilizziamo per selezionare tra i dati contenuti nelle tabelle soltanto quelli che ci occorrono. Anche in questo caso, in caso di ambiguità (due campi con lo stesso nome) il campo va indicato non semplicemente come nome\_colonna ma nella forma più completa tabella.nome\_colonna;

 I predicati possono essere:

- di confronto: =, &lt;&gt;, &lt;=, &gt;=, &lt;, &gt;
- pattern matching (confronto di schema) tra stringhe: LIKE seguita da caratteri jolly \_ per un solo carattere, % per uno o più caratteri.

 I predicati si combinano tra loro utilizzando gli operatori logici AND, OR, NOT.


{% highlight sql %}
Esempio di uso di AND 
SELECT colonna1, colonna2, ...
FROM nome_tabella
WHERE predicato1 AND predicato2 AND predicato3 ...; 

Esempio di uso di OR

SELECT colonna1, colonna2, ...
FROM nome_tabella
WHERE predicato1 OR predicato2 OR predicato3 ...; 

Esempio di uso di NOT
SELECT colonna1, colonna2, ...
FROM nome_tabella
WHERE NOT predicato;
{% endhighlight %}

#### L’istruzione JOIN in SQL 

L’istruzione JOIN ha lo scopo di combinare tuple tra due o più tabelle, basandosi su una colonna che collega logicamente le tabelle stesse.

Vediamo un esempio:

**Tabella Clienti**

<figure class="wp-block-table">| IdCliente | NomeCliente | NomeContatto | Nazione |
|---|---|---|---|
| 1 | Carlo Pedersoli | Maria Anders | Germania |
| 2 | Mario Girotti | Elvira Bianchi | Italia |
| 3 | Sergio Leone | Antonio Moreno Taquería | Messico |

</figure>**Tabella Ordini**

<figure class="wp-block-table">| IdOrdine | IdCliente | DataOrdine |
|---|---|---|
| 10308 | 2 | 12/04/2019 |
| 10309 | 1 | 13/04/2019 |
| 10310 | 3 | 18/04/2019 |

</figure>
{% highlight sql %}
 SELECT Ordini.IdOrdine, Clienti.NomeCliente, Ordini.DataOrdine 
FROM Ordini
JOIN Clienti ON Ordini.IdCliente=Clienti.IdCliente;
{% endhighlight %}

</div><figure class="wp-block-table">| IdOrdine | NomeCliente | DataOrdine |
|---|---|---|
| 10308 | Mario Girotti | 12/04/2019 |
| 10309 | Carlo Pedersoli | 13/04/2019 |
| 10310 | Sergio Leone | 18/04/2019 |

</figure>Il risultato dell’istruzione è quello di collegare tra loro le tuple corrispondenti e visualizzare insieme i dati provenienti da una o più tabelle.

### Esercizi

**Esercizio 1:**

Schema relazionale:

ATTORI (IdAttore, Nome, AnnoNascita, Nazionalità);  
RECITA (IdAttore\*, IdFilm\*)  
FILM (IdFilm, Titolo, AnnoProduzione, Nazionalità, Regista, Genere)   
PROIEZIONI (IdProiezione, IdFilm\*, IdSala\*, Incasso, DataProiezione)   
SALE (IdSala, Posti, Nome, Città)

Scrivi le sequenti query SQL:

- Inserisci nella tabella attori: “Terence Hill”, 1939, “Italiana”; “Margot Robbie”, 1990, “Australiana”
- Inserisci il film: “Lo chiamavano Trinità”, 1970, “Italiana”, “Enzo Barboni”
- Inserisci la relazione tra Terence Hill e Lo chiamavano Trinità
- Inserici le sale: 120, “Cinema Italia”, “Castel di Sangro”; 90, “Igioland”, “Corfinio”
- Inserisci la proiezione del film “Lo chiamavano Trinità” di ieri a Castel di Sangro, incasso 1000€ del 05/02/2020

**Esercizio 2**

Basandoti sul relazionale dell’esercizio uno, scrivi le seguenti query SQL:

- Correggi la proiezione di ieri fatta a Castel di Sangro: l’incasso è stato di 1200€ e non di 1000€
- Correggi la capienza della sala del cinema di Corfinio: può contenere 96 persone e non 90
- Inserisci il film “Birds of Prey”, 2020, “USA”, “Cathy Yan” in cui recita Margot Robbie
- Inserisci la relazione tra Margot Robbie e Birds of Prey
- Visualizza con una SELECT tutti gli attori presenti nel database
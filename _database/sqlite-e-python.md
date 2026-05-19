---
title: 'SQLite e Python'
date: '2020-02-19T05:11:54+01:00'
author: Fabio Mattei
layout: page
---

SQLite è un database semplificato, utilizzato molto per le applicazioni su telefonino con il fine di salvare e accedere ai dati. È spesso utilizzato nei videogiochi, lo si utilizza in Adobe Lightroom, in Apple iTunes, in Dropbox, in Firefox e in molte altre applicazioni. SQLite è piccolo ma contiene molto del linguaggio SQL standard ed è ACID, questo significa che ogni query è Atomica, Consistente, Isolata e Durevole.

#### Sqlite e i file

SQLite memorizza un intero schema relazionale in un unico file. Questo significa che tutte le tabelle e le loro relazioni saranno contenute in un file.

Per aprire un database si usa `sqlite3.connect()` passando il percorso del file. Se il file non esiste SQLite lo crea automaticamente.

{% highlight python %}
import sqlite3
db = sqlite3.connect(‘dati/ilmiodatabase.db’) 
cursor = db.cursor() 
{% endhighlight %}

L’oggetto `db` rappresenta la connessione al database; `cursor` è lo strumento con cui si inviano le query. Quando si ha finito di lavorare con il database è importante chiudere la connessione:

{% highlight python %}
db.close()
{% endhighlight %}

Ritroveremo `sqlite3.connect` e `db.cursor()` in tutti gli esempi seguenti.

#### Creiamo una tabella con SQLite e Python

{% highlight python %}
import sqlite3
db = sqlite3.connect('dati/ilmiodatabase.db') 
cursor = db.cursor() 

cursor.execute('''
CREATE TABLE studenti(id INTEGER PRIMARY KEY, nome TEXT,
telefono TEXT, email TEXT unique, password TEXT)
''') 
db.commit()   # rende il comando persistente
{% endhighlight %}

I comandi inseriti sopra creano una tabella chiamata studenti con i campi: id, nome, telefono, email e password.

#### Inseriamo dei dati nel database 

{% highlight python %}
import sqlite3
db = sqlite3.connect('dati/ilmiodatabase.db')
cursor = db.cursor()

nome1 = 'Gino'
telefono1 = '123456789' 
email1 = 'user@example.com' 
password1 = '12345'          # una password molto sicura

nome2 = 'Roberto'
telefono2 = '987654321'
email2 = 'johndoe@example.com' 
password2 = 'abcdef'

# Inserisci il primo utente
cursor.execute('''
INSERT INTO studenti(nome, telefono, email, password) VALUES(?,?,?,?)
''', (nome1,telefono1,email1,password1))
print('Primo utente inserito')

# Inserisci il secondo utente
cursor.execute('''
INSERT INTO studenti(nome, telefono, email, password) VALUES(?,?,?,?)
''', (nome2,telefono2, email2, password2))
print('Secondo utente inserito')

db.commit()    # rende il comando persistente
{% endhighlight %}

Così facendo ho inserito i dati utilizzando delle tuple, ma posso anche utilizzare un dizionario, in modo da rendere il codice più leggibile:

{% highlight python %}
import sqlite3
db = sqlite3.connect('dati/ilmiodatabase.db') 
cursor = db.cursor()

studente = {'nome': 'Gino', 'telefono': '123456789', 'email': 'gino@example.com', 'password': '12345'}
cursor.execute('''
INSERT INTO studenti(nome, telefono, email, password) VALUES(:nome, :telefono, :email, :password)
''', studente)

db.commit()    # rende il comando persistente
{% endhighlight %}

Oppure posso utilizzare una lista di tuple con `executemany`, utile quando si devono inserire molti record in una sola operazione:

{% highlight python %}
import sqlite3
db = sqlite3.connect(‘dati/ilmiodatabase.db’) 
cursor = db.cursor()

studenti = [
    (‘Gino’,    ‘123456789’, ‘gino@example.com’,    ‘12345’),
    (‘Roberto’, ‘987654321’, ‘roberto@example.com’, ‘abcdef’),
    (‘Maria’,   ‘111222333’, ‘maria@example.com’,   ‘xyz789’),
]
cursor.executemany(‘’’
INSERT INTO studenti(nome, telefono, email, password) VALUES(?,?,?,?)
‘’’, studenti)

db.commit()    # rende il comando persistente
db.close()
{% endhighlight %}

Se hai bisogno di conoscere l’id dell’ultima riga inserita:

{% highlight python %}
ultimo_id = cursor.lastrowid
print(f’Ultimo id inserito: {ultimo_id}’)
{% endhighlight %}

#### Gestire gli errori con il context manager

Chiamare `db.commit()` manualmente funziona, ma non gestisce il caso in cui una query fallisca a metà: i dati rimangono in uno stato inconsistente. Il modo corretto è usare la connessione come **context manager** con `with`:

{% highlight python %}
import sqlite3

db = sqlite3.connect('dati/ilmiodatabase.db')
cursor = db.cursor()

with db:
    cursor.execute('''
    INSERT INTO studenti(nome, telefono, email, password) VALUES(?,?,?,?)
    ''', ('Luisa', '555666777', 'luisa@example.com', 'pass123'))

# se non ci sono errori, db.commit() viene chiamato automaticamente
# in caso di eccezione, db.rollback() viene chiamato automaticamente

db.close()
{% endhighlight %}

Con il blocco `with db:` non serve più chiamare `db.commit()` esplicitamente: SQLite lo fa da solo al termine del blocco se tutto è andato a buon fine, oppure annulla tutte le operazioni del blocco se si è verificata un'eccezione.

#### Accedere ai dati contenuti in un database

Per leggere tutte le righe in una volta si usa `fetchall()`, che restituisce una lista di tuple:

{% highlight python %}
import sqlite3
db = sqlite3.connect('dati/ilmiodatabase.db') 
cursor = db.cursor()

cursor.execute('''SELECT nome, email, telefono FROM studenti''')
righe = cursor.fetchall()

for riga in righe:
    # riga[0] è nome, riga[1] è email, riga[2] è telefono
    print(f'{riga[0]} : {riga[1]}, {riga[2]}')

db.close()
{% endhighlight %}

Per leggere una singola riga si usa `fetchone()`, utile quando ci si aspetta al massimo un risultato:

{% highlight python %}
import sqlite3
db = sqlite3.connect('dati/ilmiodatabase.db') 
cursor = db.cursor()

user_id = 3
cursor.execute('''
SELECT nome, email, telefono FROM studenti WHERE id = ?
''', (user_id,))
riga = cursor.fetchone()

if riga:
    print(f'{riga[0]} : {riga[1]}')

db.close()
{% endhighlight %}

#### Accesso per nome di colonna

Accedere alle colonne tramite indice numerico (`riga[0]`, `riga[1]`) funziona ma rende il codice difficile da leggere. Impostando `db.row_factory = sqlite3.Row` prima di creare il cursore, le righe diventano accessibili per nome di colonna:

{% highlight python %}
import sqlite3
db = sqlite3.connect('dati/ilmiodatabase.db')
db.row_factory = sqlite3.Row   # abilita l'accesso per nome
cursor = db.cursor()

cursor.execute('''SELECT nome, email, telefono FROM studenti''')
for riga in cursor.fetchall():
    print(f"{riga['nome']} : {riga['email']}, {riga['telefono']}")

db.close()
{% endhighlight %}

#### SELECT con JOIN

Le query con JOIN si scrivono esattamente come in SQL. Supponendo di avere anche una tabella `voti` collegata a `studenti`:

{% highlight python %}
import sqlite3
db = sqlite3.connect('dati/ilmiodatabase.db')
db.row_factory = sqlite3.Row
cursor = db.cursor()

cursor.execute('''
SELECT studenti.nome, studenti.cognome, voti.materia, voti.voto
FROM studenti
JOIN voti ON studenti.id = voti.id_studente
ORDER BY studenti.cognome ASC
''')

for riga in cursor.fetchall():
    print(f"{riga['nome']} {riga['cognome']} - {riga['materia']}: {riga['voto']}")

db.close()
{% endhighlight %}

Per filtrare con parametri:

{% highlight python %}
materia = 'Matematica'
cursor.execute('''
SELECT studenti.nome, voti.voto
FROM studenti
JOIN voti ON studenti.id = voti.id_studente
WHERE voti.materia = ?
ORDER BY voti.voto DESC
''', (materia,))
{% endhighlight %}

#### Aggiornare i dati nel database: 

Vediamo ora come passare a SQLite una query di tipo UPDATE. Inseriamo, come di consueto, la query in una stringa di tipo multilinea contrassegnata dalle tre virgolette `’’’`.

I parametri da passare alla query vengono memorizzati in variabili e passati come tupla. La funzione `cursor.execute` riceve due argomenti: la stringa con la query SQL e la tupla con i valori, nell’ordine in cui i segnaposto `?` compaiono nella query.

{% highlight python %}
import sqlite3
db = sqlite3.connect('dati/ilmiodatabase.db') 
cursor = db.cursor()

# aggiorna lo studente con id = 1
nuovotelefono = '3113093164'
userid = 1
cursor.execute('''
UPDATE studenti SET telefono = ? WHERE id = ? 
''', (nuovotelefono, userid) )

db.commit()    # rende il comando persistente
db.close()
{% endhighlight %}

#### Cancellare i dati dal database:

Vediamo ora come passare a SQLite una query di tipo DELETE.

La query richiede il passaggio di un parametro id di tipo numerico. Questo parametro viene passato attraverso una tupla: `(id_da_cancellare,)`. Fate attenzione alla virgola finale: è necessaria perché senza di essa Python interpreta l’espressione come un semplice intero anziché come una tupla, e la chiamata a `cursor.execute` fallisce.

{% highlight python %}
import sqlite3
db = sqlite3.connect('dati/ilmiodatabase.db') 
cursor = db.cursor()

# Cancella utente con id = 2
id_da_cancellare = 2
cursor.execute('''
DELETE FROM studenti WHERE id = ?
''', (id_da_cancellare,))

db.commit()    # rende il comando persistente
db.close()
{% endhighlight %}

### Esercizi

**Esercizio 1**

Crea un database `negozio.db` con la seguente tabella:

PRODOTTI (id INTEGER PRIMARY KEY AUTOINCREMENT, nome TEXT, prezzo REAL, quantita INTEGER)

Scrivi un programma Python che:

* Crea la tabella con CREATE TABLE
* Inserisce 5 prodotti usando `executemany` con una lista di tuple
* Visualizza tutti i prodotti ordinati per prezzo crescente
* Visualizza solo i prodotti con quantità maggiore di 10

**Esercizio 2**

Partendo dal database dell'esercizio 1, scrivi un programma Python che:

* Aggiorna il prezzo di un prodotto scelto per nome
* Cancella un prodotto scelto per id
* Conta quanti prodotti costano meno di 5€ (usa `fetchone` per leggere il risultato di COUNT)
* Usa `db.row_factory = sqlite3.Row` e accedi alle colonne per nome anziché per indice

**Esercizio 3**

Crea un database `biblioteca.db` con le seguenti tabelle:

LIBRI (id INTEGER PRIMARY KEY AUTOINCREMENT, titolo TEXT, autore TEXT)  
PRESTITI (id INTEGER PRIMARY KEY AUTOINCREMENT, id_libro INTEGER, data_prestito TEXT, data_restituzione TEXT)

Scrivi un programma Python che:

* Crea le due tabelle
* Inserisce 3 libri e 2 prestiti
* Usa una query con JOIN per visualizzare, per ogni prestito, il titolo del libro e la data di prestito
* Usa `db.row_factory = sqlite3.Row` e il context manager `with db:` per tutte le operazioni di scrittura

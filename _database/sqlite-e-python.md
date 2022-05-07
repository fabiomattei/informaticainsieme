---
id: 402
title: 'SQLite e Python'
date: '2020-02-19T05:11:54+01:00'
author: fabio
layout: page
guid: 'https://www.esercizidiinformatica.it/?page_id=402'
---

SQLite è un database semplificato, utilizzato molto per le applicazioni su telefonino con il fine di salvare e accedere ai dati. E’ spesso utilizzato nei videogiochi, lo si utilizza in Adobe Lightroom, in Apple iTunes, in Dropbox, in Firefox e in molte altre applicazioni. SQLite è piccolo ma contiene molto del linguaggio SQL standard ed è ACID, questo significa che ogni query è Atomica, Consistente, Isolata e Durevole.

#### Sqlite e i file

SQLite memorizza un intero schema relazionale in un unico file. Questo significa che tutte le tabelle e le loro relazioni saranno contenute in un file.

Per potersi collegare ad un file bisogna collegarsi a questo.

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">import sqlite3
db = sqlite3.connect('dati/ilmiodatabase.db') 
cursor = db.cursor() 
```

</div>Nell’esempio in alto ci siamo collegati al file dati/ilmiodatabase.db

Quando ci si collega ad un file database SQLite che non esiste, SQLite crea un nuovo file.

Ritroveremo queste righe in tutti gli esempi seguenti

#### Creiamo una tabella con SQLite e Python

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">import sqlite3
db = sqlite3.connect('dati/ilmiodatabase.db') 
cursor = db.cursor() 

cursor.execute('''
CREATE TABLE studenti(id INTEGER PRIMARY KEY, nome TEXT,
telefono TEXT, email TEXT unique, password TEXT)
''') 
db.commit()   # rende il comando persistente
```

</div>I comandi inseriti sopra creano una tabella chiamata studenti con i campi: id, nome, telefono, email e password.

#### Inseriamo dei dati nel database 

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">import sqlite3
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
```

</div>Così facendo ho inserito i dati utilizzando delle tuple, ma posso anche utilizzare un dizionario:

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">import sqlite3
db = sqlite3.connect('dati/ilmiodatabase.db') 
cursor = db.cursor()

cursor.execute('''
INSERT INTO studenti(nome, telefono, email, password) VALUES(:nome,:telefono, :email, :password)
''', {'nome':nome1, 'telefono':telefono1,'email':email1, 'password':password1})

db.commit()    # rende il comando persistente
```

</div>Oppure posso utilizzare una lista di tuple:

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">import sqlite3
db = sqlite3.connect('dati/ilmiodatabase.db') 
cursor = db.cursor()

studenti = [(nome1,telefono1, email1, password1), (nome2,telefono2, email2, password2), (nome3,telefono3, email3, password3)]
cursor.executemany('''
INSERT INTO studenti(nome, telefono, email, password) VALUES(?,?,?,?)
''', studenti)

db.commit()    # rende il comando persistente

```

</div>Se hai bisogno di sapere l’id dell’ultima riga inserita:

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">id = cursor.lastrowid 
print('Last row id: %d' % id)

```

</div>#### Accedere ai dati contenuti in un database

Se hai bisogno di caricare i dati dal database:

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">import sqlite3
db = sqlite3.connect('dati/ilmiodatabase.db') 
cursor = db.cursor()

cursor.execute('''SELECT nome, email, telefono FROM studenti''') 
user1 = cursor.fetchone() # carica la prima riga o tupla 
print(user1[0]) # scrive il primo campo della prima tupla 
all_rows = cursor.fetchall() # carica tutte le tuple

for row in all_rows:
    # row[0] ritorna la prima colonna (nome), row[1] ritorna la colonna email. 
    print('{0} : {1}, {2}'.format(row[0], row[1], row[2]))
    
```

</div>Se hai bisogno di caricare dei dati dal database passando dei parametri per la query:

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">import sqlite3
db = sqlite3.connect('dati/ilmiodatabase.db') 
cursor = db.cursor()

user_id = 3
cursor.execute('''
SELECT nome, email, telefono FROM studenti WHERE id=?
''',(user_id,)) 
user = cursor.fetchone()
```

</div>#### Aggiornare i dati nel database: 

Vediamo ora come passare a SQLite una query di tipo UPDATE. Inseriamo, come di consueto, la quesy in una stringa di tipo multilinea contrassegnata dalle tre virgolette ”’.

I parametri da passare alla query vengono prima memorizzati in due variabili: *nuovotelefono*, *userid*.

la funzione cursor.execute si aspetta di ricevere due parametri: una stringa che contiene la query SQL e una tupla che contiene tutti i parametri che la query attende inseriti nell’ordine in cui questa li attende.

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">import sqlite3
db = sqlite3.connect('dati/ilmiodatabase.db') 
cursor = db.cursor()

# aggiorna lo studente con id = 1
nuovotelefono = '3113093164'
userid = 1
cursor.execute('''
UPDATE studenti SET telefono = ? WHERE id = ? 
''', (nuovotelefono, userid) )

db.commit()    # rende il comando persistente
```

</div>#### Cancellare i dati dal database:

Vediamo ora come passare a SQLite una query di tipo DELETE. Notiamo che la inseriamo, come di consueto in una stringa di tipo multilinea contrassegnata dalle tre virgolette ”’.

La query richiede il passaggio di un parametro id di tipo numerico, questo parametro viene passato attraverso una tupla: *(iddacancellare,)*. Fate attenzione a questa scrittura, la virgola che segue la variabile *iddacancellare* è necessaria, senza di questa la tupla viene cambiata automaticamente di tipo a *int* e la chiamata a *cursor.excecute* fallisce.

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">import sqlite3
db = sqlite3.connect('dati/ilmiodatabase.db') 
cursor = db.cursor()

# Cancella utente con id = 2
iddacancellare = 2
cursor.execute('''
DELETE FROM studenti WHERE id = ? 
''', (iddacancellare,) ) 

db.commit()    # rende il comando persistente
```

</div>
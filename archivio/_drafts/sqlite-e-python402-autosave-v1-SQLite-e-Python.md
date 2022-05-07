---
id: 408
title: 'SQLite e Python'
date: '2020-02-19T05:25:40+01:00'
author: fabio
layout: revision
guid: 'https://www.esercizidiinformatica.it/blog/2020/02/19/402-autosave-v1/'
permalink: '/?p=408'
---

SQLite è un database semplificato, utilizzato molto per le applicazioni su telefonino con il fine di salvare e accedere ai dati. E’ spesso utilizzato nei videogiochi

Quando ci si collega ad un file database SQLite che non esiste, SQLite crea un nuovo database.

Creiamo una tabella con SQLite e Python

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">import sqlite3
db = sqlite3.connect('data/mydb.db') 
cursor = db.cursor() 
cursor.execute('''
CREATE TABLE studenti(id INTEGER PRIMARY KEY, nome TEXT,
telefono TEXT, email TEXT unique, password TEXT)
''') 
db.commit()
```

</div>I comandi inseriti sopra creano una tabella chiamata studenti con i campi: id, nome, telefono, email e password.

Inseriamo dei dati nel database

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">cursor = db.cursor()
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
db.commit()
```

</div>Così facendo ho inserito i dati utilizzando delle tuple, ma posso anche utilizzare un dizionario:

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">cursor.execute('''
INSERT INTO studenti(nome, telefono, email, password) VALUES(:nome,:telefono, :email, :password)
''', {'nome':nome1, 'telefono':telefono1,'email':email1, 'password':password1})
```

</div>Oppure posso utilizzare una lista di tuple:

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">studenti = [(nome1,telefono1, email1, password1), (nome2,telefono2, email2, password2), (nome3,telefono3, email3, password3)]
cursor.executemany('''
INSERT INTO studenti(nome, telefono, email, password) VALUES(?,?,?,?)
''', studenti)
db.commit()
```

</div>Se hai bisogno di sapere l’id dell’ultima riga inserita:

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">id = cursor.lastrowid 
print('Last row id: %d' % id)

```

</div>Se hai bisogno di caricare i dati dal database:

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">cursor.execute('''SELECT nome, email, telefono FROM studenti''') user1 = cursor.fetchone() # carica la prima riga o tupla 
print(user1[0]) # scrive il primo campo della prima tupla 
all_rows = cursor.fetchall() # carica tutte le tuple
for row in all_rows:
    # row[0] ritorna la prima colonna (nome), row[1] ritorna la colonna email. 
    print('{0} : {1}, {2}'.format(row[0], row[1], row[2]))
```

</div>Se hai bisogno di caricare dei dati dal database passando dei parametri per la query:

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">user_id = 3
cursor.execute('''
SELECT nome, email, telefono FROM studenti WHERE id=?
''',(user_id,)) user = cursor.fetchone()
```

</div>Aggiornare i dati nel database:

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0"># aggiorna lo studente con id = 1
nuovotelefono = '3113093164'
userid = 1
cursor.execute('''
UPDATE studenti SET telefono = ? WHERE id = ? 
''', (nuovotelefono, userid) )
```

</div>Cancellare i dati dal database:

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0"># Cancella utente con id = 2
iddacancellare = 2
cursor.execute('''
DELETE FROM studenti WHERE id = ? 
''', (iddacancellare,) ) 
db.commit()
```

</div>
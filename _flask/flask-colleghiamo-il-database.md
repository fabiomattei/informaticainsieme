---
id: 824
title: 'Flask: colleghiamo il database'
date: '2021-11-18T08:20:53+01:00'
author: fabio
layout: page
guid: 'https://www.esercizidiinformatica.it/?page_id=824'
---

#### Creazione del database

Una applicazione web ha bisogno di un database per poter salvare/reperire i dati, altrimenti è purtroppo poco utile. Utilizzeremo per la nostra esperienza un database che si chiama **SQlite** che è parte del core di Python.

Creiamo un database che salviamo nel file **database.db** e creiamo una tabella al suo interno.

{% highlight python %}
import sqlite3

conn = sqlite3.connect('database.db')
print "Opened database successfully";

conn.execute('CREATE TABLE students (name TEXT, addr TEXT, city TEXT, pin TEXT)')
print "Table created successfully";
conn.close()
{% endhighlight %}

</div>Il file va lanciato digitando: python creadb.py

#### Aggiungi uno studente al database

La nostra applicazione Flask ha tre funzioni all’interno del controller.

La prima si chiama **new\_student()** e si collega alla regola URL **(‘/addnew’)**. La funzione mostra un template che si chiama student.html.

{% highlight python %}
@app.route('/enternew')
def new_student():
   return render_template('student.html')
   
{% endhighlight %}

</div>Il contenuto del file **student.html**

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">{% endhighlight %}
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="html" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0"><html>
   <body>
      <form action = "{{ url_for('addrec') }}" method = "POST">
         <h3>Student Information</h3>
         Name<br>
         <input type = "text" name = "nm" /></br>
         
         Address<br>
         <textarea name = "add" ></textarea><br>
         
         City<br>
         <input type = "text" name = "city" /><br>
         
         <input type = "submit" value = "submit" /><br>
      </form>
   </body>
</html>
{% endhighlight %}

</div>Come si nota i dati della form vengono inviati in POST alla URL **/addrec** che è collegata alla funzione **addrec()**.

Questa funzione **addrec()** raccoglie le informazioni inviate in **POST** ed inserisce uno studente nella tabella. Se l’operazione va a buon fine fiene mostrato il messaggio “studente inserito con successo” in caso contrario viene mostrato il messaggio “errore nell’inserimento”.

{% highlight python %}
@app.route('/addrec',methods = ['POST', 'GET'])
def addrec():
   if request.method == 'POST':
      try:
         nm = request.form['nm']
         addr = request.form['add']
         city = request.form['city']
         pin = request.form['pin']
         
         with sql.connect("database.db") as con:
            cur = con.cursor()
            cur.execute("INSERT INTO students (name,addr,city,pin) 
               VALUES (?,?,?,?)",(nm,addr,city,pin) )
            
            con.commit()
            msg = "studente inserito con successo"
      except:
         con.rollback()
         msg = "errore nell'inserimento"
      
      finally:
         return render_template("result.html",msg = msg)
         con.close()
         
{% endhighlight %}

</div>Il template HTML che si chiama **result.html** mostra il contenuto della variabile **{{msg}}** che mostrail risultato dell’operazione di inserimento.

<figure class="wp-block-image size-large">![](https://www.esercizidiinformatica.it/wp-content/uploads/2021/11/aggiungistudente-1024x683.png)</figure><div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">{% endhighlight %}
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="html" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0"><!doctype html>
<html>
   <body>
      result of addition : {{ msg }}
      <h2><a href = "\">go back to home page</a></h2>
   </body>
</html>
{% endhighlight %}

</div><figure class="wp-block-image size-large">![](https://www.esercizidiinformatica.it/wp-content/uploads/2021/11/studenteaggiunto-1024x687.png)</figure>#### Mostra tutti gli studenti contenuti nel database

<figure class="wp-block-image size-large">![](https://www.esercizidiinformatica.it/wp-content/uploads/2021/11/listastudenti-1024x299.png)</figure>L’applicazione contiene un’altra funzione che si chiama **list()** che si collega alla URL **/list**. Questa interroga il database e carica i risultati in **rows** che saranno contenuti in una istanza di **MultiDict**. Questo conterrà tutti i record nella tabella students. Questo oggetto è passato al template **list.html**.

{% highlight python %}
@app.route('/list')
def list():
   con = sql.connect("database.db")
   con.row_factory = sql.Row
   
   cur = con.cursor()
   cur.execute("select * from students")
   
   rows = cur.fetchall(); 
   return render_template("list.html",rows = rows)
   
{% endhighlight %}

</div>Il template **list.html** contiene una iterazione sui questi records e inserisce i dati degli studenti in una tabella HTML.

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">{% endhighlight %}
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="html" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0"><!doctype html>
<html>
   <body>
      <table border = 1>
         <thead>
            <td>Name</td>
            <td>Address></td>
            <td>city</td>
            <td>Pincode</td>
         </thead>
         
         {% for row in rows %}
            <tr>
               <td>{{row["name"]}}</td>
               <td>{{row["addr"]}}</td>
               <td> {{ row["city"]}}</td>
               <td>{{row['pin']}}</td>	
            </tr>
         {% endfor %}
      </table>
      
      <a href = "/">Go back to home page</a>
   </body>
</html>
{% endhighlight %}

</div>#### La home del progetto

<figure class="wp-block-image size-full">![](https://www.esercizidiinformatica.it/wp-content/uploads/2021/11/studentihome.png)</figure>In fine la URL **/** renderizza un file **‘home.html’** che agisce da punto di accesso all’intera applicazione.

{% highlight python %}
@app.route('/')
def home():
   return render_template('home.html')
   
{% endhighlight %}

</div>#### Modifica le informazioni di uno studente

<figure class="wp-block-image size-large">![](https://www.esercizidiinformatica.it/wp-content/uploads/2021/11/modifiicastudente-1024x619.png)</figure>#### Cancella le informazioni di uno studente

<figure class="wp-block-image size-full">![](https://www.esercizidiinformatica.it/wp-content/uploads/2021/11/cancellastudente.png)</figure>Ecco il codice completo del controller.

{% highlight python %}
from flask import Flask, render_template, request
import sqlite3 as sql
app = Flask(__name__)

@app.route('/')
def home():
   return render_template('home.html')

@app.route('/enternew')
def new_student():
   return render_template('student.html')

@app.route('/addrec',methods = ['POST', 'GET'])
def addrec():
   if request.method == 'POST':
      try:
         nm = request.form['nm']
         addr = request.form['add']
         city = request.form['city']
         pin = request.form['pin']
         
         with sql.connect("database.db") as con:
            cur = con.cursor()
            
            cur.execute("INSERT INTO students (name,addr,city,pin) 
               VALUES (?,?,?,?)",(nm,addr,city,pin) )
            
            con.commit()
            msg = "Record successfully added"
      except:
         con.rollback()
         msg = "error in insert operation"
      
      finally:
         return render_template("result.html",msg = msg)
         con.close()

@app.route('/list')
def list():
   con = sql.connect("database.db")
   con.row_factory = sql.Row
   
   cur = con.cursor()
   cur.execute("select * from students")
   
   rows = cur.fetchall();
   return render_template("list.html",rows = rows)

if __name__ == '__main__':
   app.run(debug = True)
   
{% endhighlight %}

</div>Lancia questo script dalla shell di python quindi visita la URL **http://localhost:5000/** nella barra degli indirizzi del browser per vedere l’applicazione funzionare.

</body></html>
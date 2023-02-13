---
title: 'Flask: colleghiamo il database'
date: '2021-11-18T08:20:53+01:00'
author: Fabio Mattei
layout: page
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

Il file va lanciato digitando: python creadb.py

#### Aggiungi uno studente al database

La nostra applicazione Flask ha tre funzioni all’interno del controller.

La prima si chiama **new\_student()** e si collega alla regola URL **(‘/addnew’)**. La funzione mostra un template che si chiama student.html.

{% highlight python %}
@app.route('/enternew')
def new_student():
   return render_template('student.html')
   
{% endhighlight %}

Il contenuto del file **student.html**


{% highlight html %}
{% raw %}
<html>
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
{% endraw %}
{% endhighlight %}

Come si nota i dati della form vengono inviati in POST alla URL **/addrec** che è collegata alla funzione **addrec()**.

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

Il template HTML che si chiama **result.html** mostra il contenuto della variabile **{{msg}}** che mostrail risultato dell’operazione di inserimento.

![Aggiungi studente](/images/python/flask/aggiungistudente.png){:class="aside-image"}
{% highlight html %}
<!doctype html>
<html>
   <body>
      result of addition : {{ msg }}
      <h2><a href = "\">go back to home page</a></h2>
   </body>
</html>
{% endhighlight %}


![Studente aggiunto](/images/python/flask/studenteaggiunto.png){:class="aside-image"}

#### Mostra tutti gli studenti contenuti nel database

![Lista studenti](/images/python/flask/listastudenti.png){:class="aside-image"}
L’applicazione contiene un’altra funzione che si chiama **list()** che si collega alla URL **/list**. Questa interroga il database e carica i risultati in **rows** che saranno contenuti in una istanza di **MultiDict**. Questo conterrà tutti i record nella tabella students. Questo oggetto è passato al template **list.html**.

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

Il template **list.html** contiene una iterazione sui questi records e inserisce i dati degli studenti in una tabella HTML.


{% highlight html %}
<!doctype html>
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

#### La home del progetto

![home studenti](/images/python/flask/studentihome.png){:class="aside-image"}
In fine la URL **/** renderizza un file **‘home.html’** che agisce da punto di accesso all’intera applicazione.

{% highlight python %}
@app.route('/')
def home():
   return render_template('home.html')
   
{% endhighlight %}

#### Modifica le informazioni di uno studente

![Modifica studente](/images/python/flask/modifiicastudente.png){:class="aside-image"}

#### Cancella le informazioni di uno studente

![Cancella studente](/images/python/flask/cancellastudente.png){:class="aside-image"}
Ecco il codice completo del controller.

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

Lancia questo script dalla shell di python quindi visita la URL **http://localhost:5000/** nella barra degli indirizzi del browser per vedere l’applicazione funzionare.


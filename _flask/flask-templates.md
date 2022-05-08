---
title: 'Flask: Templates'
date: '2021-10-21T00:36:43+02:00'
author: Fabio Mattei
layout: page
---

E’ comodo in una applicazione web, dividere la logica dall’interfaccia. Nella precedente lezione abbiamo scritto i tag HTML che andavano a comporre l’interfaccia come stringhe di testo da restituire al chiamante. Ora andremo a dividere le due parti.

Per seguire questa lezione scarica il [file contenente gli script](https://www.esercizidiinformatica.it/progetti/flask/miosito2.zip).

#### render\_template()

Introduciamo la funzione *render\_template()*. Questa funzione si accompagna di solito al return di una funzione nel file principale dell’applicazione. Gli argomenti che prende sono:

1. una stringa di testo contenente il nome di un file html
2. una o più variabili da passare al file html

{% highlight erb %}
from flask import Flask, request, render_template

app = Flask(__name__)

@app.route("/")
def hello_world():
    miavariabile = "Anna Bolena"
    return render_template('home.html', nomeutente=miavariabile)

{% endhighlight %}

</div>In questo esempio andiamo a definire come pagina, contenente la maschera da riempire, il file chiamato *home.html* e passiamo quindi una variabile a cui assegnamo il contenuto della variabile *miavariabile*.

#### Il primo template

Andiamo a vedere come è fatto il file *home.html*. Per convenzione tutti i file html sono posti all’interno della cartella **templates** dentro la cartella del progetto.


{% highlight erb %}
{% raw %}
{% extends 'base.html' %}

{% block header %}
  <h1>Benvenuto {{ nomeutente }} </h1>
{% endblock %}

{% block content %}
  <p>Il mio contenuto</p>
{% endblock %}
{% endraw %}
{% endhighlight %}

</div>Il file va ad estendere un file di nome *base.html* che vedremo più in avanti. Al suo interno si definiscono due blocchi: *header* e *content*. I blocchi contengono il primo un tag h1 specificante un titolo e il secondo un tag p specificante un paragrafo.

All’interno del blocco header troviamo un segnaposto di variabile: *{{ nomeutente }}*.

#### Il file base

Il file base contiene i buchi da riempire con i blocchi definiti in precedenza. Notiamo che questo contiene lo scheletro di una pagina html. All’interno della sezione header è contenuto il segnaposto pronto a contenere il blocco denominato header. In seguito è contenuto il segnaposto pronto ad accogliere un blocco content.

{% highlight html %}
{% raw %}
<!doctype html>
<title>{% block title %}{% endblock %} - la mia applicazione web</title>
<section class="content">
  <header>
    {% block header %}{% endblock %}
  </header>
  {% block content %}{% endblock %}
</section>
{% endraw %}
{% endhighlight %}

</div>Abbiamo visto oggi come creare una struttura che ci permette di lavorare ad una applicazione web in modo più ordinato.


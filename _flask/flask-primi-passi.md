---
title: 'Flask: primi passi'
date: '2021-10-21T00:34:31+02:00'
author: Fabio Mattei
layout: page
---

Per seguire questa lezione scarica il [file contenente gli script](https://www.esercizidiinformatica.it/progetti/flask/miosito.zip).

Apriamo il nostro editor python preferito e digitiamo il seguente script chiamandolo **hello.py**. E’ bene salvare questo script in una cartella che chiameremo **sitoweb** e che posizioneremo sulla scrivania desktop.

{% highlight python %}
from flask import Flask

app = Flask(__name__)

@app.route("/")
def hello_world():
    return "<p>Hello, World!</p>"
{% endhighlight %}

</div><figure class="wp-block-image size-full">![](https://www.esercizidiinformatica.it/wp-content/uploads/2021/10/editorflask.png)</figure>Apriamo l’interprete dei comandi **CMD** e posizionamoci nella cartella appena creata utlizzando il comando **cd**. Il comando cd rappresenta l’acronimo delle parole *change directory* che significa cambia cartella e ci permette di muoverci nel file system del computer.

<figure class="wp-block-image size-large">![](https://www.esercizidiinformatica.it/wp-content/uploads/2021/10/cambiadirectory-1024x596.png)</figure>Ora digitiamo i comandi:


{% highlight python %}
set FLASK_APP=hello
set FLASK_ENV=development
flask run
{% endhighlight %}

</div>I primi due comandi ci permettono di assegnare dei valori alle variabili di ambiente FLASK\_APP e FLASK\_ENV. Attraverso queste comunichiamo a flask il nome del file che contiene l’applicazione e il fatto che stiamo sviluppando l’applicazione e siamo in modalità sviluppatori. Il comando **flask run** avvia l’applicazione. Il server viene avviato e si mette in ascolto sulla porta 5000 del computer locale. Ogni computer identifica se stesso attraverso l’indirizzo 127.0.0.1 chiamato per convenzione *local host*.

<figure class="wp-block-image size-large">![](https://www.esercizidiinformatica.it/wp-content/uploads/2021/10/flaskstart-1024x542.png)</figure>Se ora apriamo il nostro browser e sulla barra degli indirizzi scriviamo 127.0.0.1:5000 vedremo apparire il nostro messaggio di benvenuto.

<figure class="wp-block-image size-large">![](https://www.esercizidiinformatica.it/wp-content/uploads/2021/10/flaskfunzionante-1024x417.png)</figure>E’ la nostra prima applicazione web funzionante!

#### Aggiungiamo una seconda pagina

Estendiamo il nostro script aggiungendo una seconda pagina.

{% highlight python %}
from flask import Flask

app = Flask(__name__)

@app.route("/")
def hello_world():
    return "<p>Hello, World!</p>"

@app.route("/citta")
def mia_citta():
    return "<p>Sono a Castel Di Sangro</p>"
{% endhighlight %}

</div>Ora salviamo il file, passiamo di nuovo al nostro browser scriviamo nella barra degli indirizzi: **127.0.0.1/citta**

<figure class="wp-block-image size-large">![](https://www.esercizidiinformatica.it/wp-content/uploads/2021/10/faskcastello-1024x678.png)</figure>Vedremo apparire la nostra seconda pagina.

#### Collegamento tra indirizzo (URL) e controller

<figure class="wp-block-image size-large">![](https://www.esercizidiinformatica.it/wp-content/uploads/2021/11/url-controller-1024x834.png)</figure>E’ bene soffermarci sul collegamento esistente tra l’indirizzo digitato e lo script python che abbiano sviluppato.

A ciascun indirizzo che digito nella barra degli indirizzi corrisponde una **route** nel mio **controller**. La route viene sempre seguita da una funzione che viene chiamata quando la route viene invocata.

La funzione restituirà sempre una stringa contenente il **codice html** che verrà **renderizzato e visualizzato lo browser dell’utente**.

Questo è il concetto chiave che rende possibile creare una pagina web dinamica e che collega il linguaggio di programmazione al server web.
---
id: 788
title: 'Flask: URL e variabili'
date: '2021-10-28T05:54:35+02:00'
author: fabio
layout: page
guid: 'https://www.esercizidiinformatica.it/?page_id=788'
---

Una **Uniform Resource Locator** (URL), di solito chiamata *indirizzo web*, è un riferimento ad una risorsa (file di testo, immagine, file audio/video) che si trova sul web e specifica la sua collocazione in una rete di computer. Una URL è un tipo specifico di **Uniform Resource Identifier** (URI), anche se in molti confondono i due termini. Le URL sono un riferimento per lo più di pagine web (http) ma posso essere usate per identificare una risorsa in un trasferimento di file (ftp), per identificare un indirizzo email (mailto), per ideentificare un punto di acceesso ad un database (JDBC) e per molto altro.

I browser mostrano la URL di una pagina web nella barra degli indirizzi. Una tipica URL potrebbe essere: http://www.example.com/index.html.

- http indica il **protocollo di trasferimento dati**
- www.example.com indica l’**hostname**, il nome del computer che ospita la risorsa
- index.html indica il **nome della risorsa**

Questa forma di URL è stata specificata da Tim Berners Lee nell’[RFC 1738](https://datatracker.ietf.org/doc/html/rfc1738).

#### Flask e le URL

Flask associa una funzione ad ogni URL e fa questo utilizzando la direttiva @app.route.

{% highlight python %}
from flask import Flask, request, render_template

app = Flask(__name__)

@app.route("/")
def hello_world():
    miavariabile = "Anna Bolena"
    return render_template('home.html', nomeutente=miavariabile)

@app.route("/primapagina")
def prima_pagina():
    numeri = [1, 3, 5]
    return render_template('primapagina.html', mieinumeri=numeri)

@app.route("/secondapagina")
def seconda_pagina():
    tupla = ("giovanni", "pasquale", "mirko")
    return render_template('secondapagina.html', miatupla=tupla)

{% endhighlight %}

</div>In questo esempio vengono esplicitate tre URL. La prima è composta da un semplice / e per convenzione punta alla risorsa richiesta quando non viene specificata alcuna risorsa. Di solito questa coincide con il file index.html.

#### Aggiungiamo le variabili

Per seguire questa lezione scarica il [file contenente gli script](https://www.esercizidiinformatica.it/progetti/flask/miosito3.zip).

E’ possibile associare le variabili alle risorse, è inoltre possibile specificarne il tipo. La variabile ricevuta verrà mandata come argomento alla funzione.

La sintassi per specificare una variabile è la seguente:

**&lt;tipo di variabile: nome di variabile&gt;**

Guardiamo un esempio

{% highlight python %}
from markupsafe import escape

@app.route('/utente/<nomeutente>')
def mostra_profilo_utente(nomeutente):
    return 'User '+escape(nomeutente)

@app.route('/post/<int:post_id>')
def mostra_post(post_id):
    return 'Post Numero '+post_id

@app.route('/path/<path:subpath>')
def mostra_subpath(subpath):
    return 'Subpath '+escape(subpath)

{% endhighlight %}

</div>La prima funzione è associata ad una URL del tipo: *http://www.lamiaapplicazioneweb.com/utente/mariorossi*. Se questa URL viene richiesta al server che ospita la mia applicazione viene chiamata la funzione *mostra\_profilo\_utente* passando come argomento *mariorossi* associato al parametro *nomeutente*. Viene quindi mostrata nel browser la scritta: User mariorossi.

La seconda funzione è associata ad una URL del tipo: *http://www.lamiaapplicazioneweb.com/post/123*. Viene chiamata la funzione *mostra\_post* passando come argomento *123* associato al parametro *post\_id*. Viene quindi mostrata nel browser la scritta: Post nummero 123.

La terza funzione è associata ad una URL del tipo: *http://www.lamiaapplicazioneweb.com/path/ilmiosottopath/cartella/cartella/risorsa*. Viene chiamata la funzione *mostra\_subpath* passando come argomento **ilmiosottopath/cartella/cartella/risorsa** associato al parametro *sub\_path*. Viene quindi mostrata nel browser la scritta: Subpath ilmiosottopath/cartella/cartella/risorsa.

Notiamo che i parametri sono sempre stati passati alla funzione escape. Si fa questo perché gli intenti degli utenti della nostra applicazione potrebbero essere malevoli dunque è bene filtrare le informazioni che arrivano dagli utenti per evitare problemi di sicurezza.

<figure class="wp-block-image size-large">![](https://www.esercizidiinformatica.it/wp-content/uploads/2021/11/flaskesempiourl-1024x352.png)</figure>#### I tipi accettati

<figure class="wp-block-table">| string (default) | accetta un qualsiasi testo senza / |
|---|---|
| int | accetta interi positivi |
| float | accetta numeri con la virgola positivi |
| path | come la stringa ma accetta anche gli / |
| uuid | accetta stringhe **uuid** identificatore universale per una risorsa. Es: `123e4567-e89b-12d3-a456-426614174000` |

</figure>
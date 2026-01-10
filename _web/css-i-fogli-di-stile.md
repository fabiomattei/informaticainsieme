---
title: 'CSS i fogli di stile'
date: '2021-05-21T11:44:47+02:00'
author: Fabio Mattei
layout: page
---

L’acronimo **CSS** ha come significato **Cascading Style Sheets** (fogli di stile a cascata). Questo indica un linguaggio che permette di formattare lo sitle cioè le decorazioni grafiche di un documento web (HTML). 

Se da un lato l’HTML suddivide e compone i contenuti dal punto di vista **semantico**, la sua sezione **CSS** contiene indicazioni su come ciascuno di questi contenuti debba essere composto dal punto di vista **grafico**, 

All’interno del CSS vengono definite informazioni su come devono apparire i font, i colori, le immagini di sfondo, il layout, il posizionamento delle colonne o di altri elementi sulla pagina.

#### L'attributo style

{% highlight html %}
<p style="font-size: 30px; color red;">L'Italia è un paese bellissimo</p>
{% endhighlight %}

Si può notare che per questo paragrafo viene specificato l'attributo style al fine di indicare che il testo deve avere un font dalle dimnesioni di 30 pixel e deve essere di colore rosso.

E' possibile utilizzare l'attributo style, al fine di indicare, per ciascun tag, le sue caratteristiche grafiche.

{% highlight html %}
<p style="font-size: 30px; color red;">L'Italia è un paese bellissimo.</p>

<p style="font-size: 30px; color yellow; background-color: black;">In Italia ci sono molti monumenti.</p>
{% endhighlight %}

Per il secondo paragrafo sono stati definiti:

* grandezza del carattare
* colore del carattare
* colore di sfondo

#### I selettori

Attraverso i selettori CSS è possibile **separare il codice CSS dai tag HTML**.

L'HTML dà la possibilità di inseire il codice CSS all'interno dell'attributo style di ciascun tag tuttavia questa 
modalità di lavoro non è tra le più raccomandate. 
Ci sono due aspetti da considerare:

* il codice css inserito come attributo in un tag HTML rende il codice **pesante** da leggere e da manutenere
* la modifica del codice css diventa laboriosa

I CSS definiscono **attributi** da applicare ai tag. Ciascun attributo interviene su di un aspetto specifico della presentazione grafica. Gli attributi vengono raggruppati tra loro ed associati ad un **selettore** in modo da poter essere richiamati all’interno del codice HTML.

#### Selettore id

Supponiamo di avere il seguente codice HTML:


{% highlight html %}
<p id="primoparagrafo">Questo è il testo del mio paragrafo</p>

<p id="secondoparagrafo">Ora ho un nuovo paragrafo</p>

<ul id="lamialissta">
    <li>Farina</li>
    <li>Lievito</li>
    <li>Uova</li>
</ul>
{% endhighlight %}

Notiamo come a ciascun tag utilizzato è stato associato un **attributo id**. Questo attributo mi permette di identificare in modo univoco un tag in una pagina. A questo punto posso iniziare a scrivere il codice CSS:


{% highlight css %}
#primoparagrafo {
    font-family: Georgia, "Times New Roman", serif;
    font-size: 12px;
    font-weight: bold;
    text-align: right;
    text-decoration: line-through;
}

#secondoparagrafo {
    font-family: Arial, Verdana, sans-serif;
    font-size: 16px;
    font-style: italic;
    text-align: justify;
    text-decoration: overline;
}

#lamialissta {
    font-family: Helvetica, sans-serif;
    font-size: 11px;
    text-align: center;
}
{% endhighlight %}

Come possiamo vedere il codice CSS crea aggregazioni di stili associati ad un attributo. Il paragrafo che ha per id *primoparagrafo* utilizza ili font Georgia, con una dimensione di 12 pixel, con un carattere grassetto, allineato a destra con una linea che attraversa il testo. Il paragrafo che ha per id *secondoparagrafo* utilizza un font Arial, con un testo di dimensione 16 pixel, uno stile italico, giustificato.

#### Selettore class

Supponiamo di avere il seguente codice HTML:


{% highlight html %}
<p class="coseimportanti">Questo è il testo del mio paragrafo</p>

<p class="coseimportanti">Ora ho un nuovo paragrafo</p>

<ul class="coseimportanti">
    <li>Farina</li>
    <li>Lievito</li>
    <li>Uova</li>
</ul>
{% endhighlight %}

Come possiamo notare tutti questi tag sono accumunati dall’appartenere alla classe *coseimportanti*.


{% highlight css %}
.coseimportanti {
    color: red;
    background-color: #FFFF00; /* Colore giallo */
}
{% endhighlight %}

Dunque a tutti gli elementi nella pagina che hanno l’attributo class contenente *coseimportanti* sarà applicato un colore del testo rosso ed uno sfondo giallo.

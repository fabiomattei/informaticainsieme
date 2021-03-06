---
title: 'CSS i fogli di stile'
date: '2021-05-21T11:44:47+02:00'
author: Fabio Mattei
layout: page
---

L’acronimo **CSS** sta per **Cascading Style Sheets** (fogli di stile a cascata) e indica un linguaggio che permette di formattare lo di un documento web (HTML). I CSS contengono indicazioni su come l’interfaccia debba essere composta dal punto di vista grafico, mentre l’HTML compone le informazioini dal punto di vista semantico.

All’interno del CSS vengono definite cose come i font, i colori, le immagini di sfondo, il layout, il posizionamento delle colonne o di altri elementi sulla pagina.

#### I selettori

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

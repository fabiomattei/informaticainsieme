---
title: 'Il box model'
date: '2021-12-11T06:03:23+01:00'
author: Fabio Mattei
layout: page
---

Ciascun TAG in una pagina web occupa un certo spazio.
Fino ad ora abbiamo imparato a conoscere diversi TAG tra cui ricordiamo <H1> ...<H6>, <P>, <UL> ecc.
Ciascuno di questi, quando inserito all'interno di una pagina, occupa uno spazio che dipende dal TAG stesso e dalle sue **proprietà CSS**.

Il box-model ci aiuta a specificare, in modo puntuale, come desideriamo impostare lo spazio che ciascun TAG va a coprire all'interno di una pagina.

All'interno del box.model distinguiamo: 

- **content:** contenuto del box, che sta al centro, delimitato dalle proprietà css width e height
- **padding:** distanza tra contenuto e bordo, delimitato dalla proprietà css padding ma anche da padding-top, padding-bottom, padding-left, padding-right
- **border:** bordo che circonda il box 
- **margin:** distanza tra un box e quelli che lo circondano, delimitato dalla proprietà css margin ma anche da margin-top, margin-bottom, margin-left, margin-right

![Box Model](/images/web/boxmodel.png){:class="half-image"}

### Esempio

Supponiamo di avere il seguente paragrafo appartenente alla classe mioparagrafo:

{% highlight html %}
<p class="mioparagrafo">Nel mezzo del cammin di nostra vita<br />
mi ritrovai per una selva oscura<br />
ché la diritta via era smarrita..</p>
{% endhighlight %}

La classe mioparagrafo è definita come segue:

{% highlight css %}
.mioparagrafo {
    margin: 12px;
    border: 1px dashed black;
    padding: 10px;
}
{% endhighlight %}

In questo esempio non ho specificato le proprietà css **width** e **height** quindi il contenuto del
paragrafo **è libero di prendere tutto lo spazio necessario**. Se avessi specificato ad 
esempio la larghezza il contenuto sarebbe stato vincolato a non superare la larghezza definita
nel css. 

Il padding è stato impostato a 10px, questo significa che lo spazio che intercorre tra il contenuto e il bordo è pari a 10px in ogni direzione. 

Il bordo è stato impostato a 1px dashed black questo significa che abbiamo un bordo spesso 1px tratteggiato di colore nero. 

Il margin è stato impostato a 12px, questo significa che lo spazio che intercorre tra il tag p e i tag che lo procedono o che lo seguono sono ad una distanza di **almeno 12px**. Diciamo "almeno" perché se un tag confinante avesse un margin maggiore il browser sceglierebbe quest'ultimo al posto di quello appena definito nel paragrafo. 
 

![Box Model](/images/web/boxmodel-esempio.png){:class="half-image"}

### Il bordo

Il bordo merita una trattazione specifica dato che è un elemento ricco di proprietà.

Un bordo può avere diversi stili di linea che si inseriscono nella proprietà border-style:

- dotted: punteggiato
- dashed: tratteggiato
- solid: continuo
- double: a doppia linea
- groove: effetto 3D con luce da sinistra in alto
- ridge: effetto 3D con luce da destra in basso
- inset: diverso effetto 3D con luce da sinistra in alto
- outset: diverso effetto 3D con luce da destra in basso
- none: nessun bordo
- hidden: bordo nascosto

Il **colore** si attribusce al bordo attraverso la proprietà border-color

{% highlight css %}
/* <color> values */
border-color: red;

/* top and bottom | left and right */
border-color: red #f015ca;
{% endhighlight %}

Lo **spessore** si attribuisce attraverso la proprità border-width

{% highlight css %}
border-width: 2px;
{% endhighlight %}

Si può anche riassumere il tutto attraverso la proprietà border:

{% highlight css %}
border: 1px solid red;
{% endhighlight %}

#### Esercizio 1:

Prova a disegnare il box model definito dal seguente codice poi scrivilo al computer per vedere il risultato finale:

{% highlight css %}
.mioparagrafo {
    margin: 12px;
    border: 1px double green;
    border-radius: 5px;
    padding-top: 10px;
    padding-bottom: 20px;
    padding-left: 30px;
    padding-right: 40px;
}
{% endhighlight %}

{% highlight html %}
<p class="mioparagrafo">Voi ch’ascoltate in rime sparse il suono<br />
di quei sospiri ond’io nudriva ’l core<br />
in sul mio primo giovenile errore<br />
quand’era in parte altr’uom da quel ch’i’ sono</p>
{% endhighlight %}

#### Esercizio 2:

Prova a disegnare il box model definito dal seguente codice poi scrivilo al computer per vedere il risultato finale:

{% highlight css %}
.mioparagrafo {
    margin-top: 12px;
    margin-bottom: 20px;
    border: 2px dotted red;
    border-radius: 7px;
    padding: 10px;
}
{% endhighlight %}

{% highlight html %}
<p class="mioparagrafo">Voi ch’ascoltate in rime sparse il suono<br />
di quei sospiri ond’io nudriva ’l core<br />
in sul mio primo giovenile errore<br />
quand’era in parte altr’uom da quel ch’i’ sono</p>

<p class="mioparagrafo">del vario stile in ch’io piango et ragiono<br />
fra le vane speranze e ’l van dolore,<br />
ove sia chi per prova intenda amore,<br />
spero trovar pietà, nonché perdono.</p>
{% endhighlight %}

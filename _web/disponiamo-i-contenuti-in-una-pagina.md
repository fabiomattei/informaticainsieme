---
title: 'Disponiamo i contenuti in una pagina'
date: '2021-12-11T06:03:23+01:00'
author: Fabio Mattei
layout: page
---

Il W3C ha definito il **grid system** in modo da dare un sistema per disporre contenuti in una pagina web. Il sito [The grid system](http://www.thegridsystem.org) contiene molti esempi da cui si può prendere spunto.

Il problema che risolve è il seguente: dividere la pagina in sezioni in modo da dare una struttura logica e grafica alla pagina.

![Il Grid System](/images/web/grid/grid-system.png){:class="aside-image"}

Questo risultato si raggiunge utilizzando un tag **div** come contenitore e definendo le sottoaree attraverso il codice css.


{% highlight css %}
.container {
  display: grid;
  grid-template-columns: 40px 50px auto 50px 40px;
  grid-template-rows: 25% 100px auto;
}
{% endhighlight %}

La prorietà css **grid-template-columns** e **grid-template-rows** mi permettono di definire il numero e le dimensioni di rige e colonne della griglia base. Le unità di misura per definire le dimensioni possono essere: percentuale, pixel, frazioni.

Una volta definita la griglia base devo disporre le aree al suo interno per creare per esempio qualcosa del genere.

![Il Grid System](/images/web/grid/grid-system-struttura.png){:class="aside-image"}

{% highlight css %}
.container {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr 1fr;
  grid-template-rows: auto;
  grid-template-areas: 
    "header header header header"
    "main main . sidebar"
    "footer footer footer footer";
}

.item-a {
  grid-area: header;
}
.item-b {
  grid-area: main;
}
.item-c {
  grid-area: sidebar;
}
.item-d {
  grid-area: footer;
}

{% endhighlight %}

In questo esempio la prima riga viene dedicata alla testata, le due colonne a sinistra ad una sezione di contenuti, segue uno spazio lasciato vuoto, una barra laterale ed in fine il piè di pagina.

L’html che si accompagna a questo css è il seguente:


{% highlight html %}
<div class="container" >
    <div class="item-a"></div>
    <div class="item-b"></div>
    <div class="item-c"></div>
    <div class="item-d"></div>
</div>
{% endhighlight %}

Posso inserire all’interno dei divisor (div) i contenuti che voglio essendo certo che finiranno nella giusta porzione di pagina.

Il grid sistem risolve un problema che per anni ha confuso i creatori di pagine web. E’ uno strumento che seppur tardivo si rivelò molto potente.

## Esercizi

#### Esercizio 1: 
Scrivi il codice HTML e il relativo CSS per ottenere una griglia fatta come in figura.

![Esercizio 1](/images/web/grid/esercizio1.png){:class="half-image"}

Scrivi poi il codice CSS affiché:

* l'header abbia un bordo di 1px tratteggiato di colore nero, padding di 10px, margin di 10px e colore di sfondo come in figura
* il main abbia un bordo di 3px continuo di colore verde scuro, padding di 10px, margin di 10px e colore di sfondo come in figura
* la sidebar abbia un bordo di 5px a puntini di colore blù scuro, padding di 30px, margin di 10px e colore di sfondo come in figura
* il footer abbia un bordo di 1px a puntini di colore viola scuro, padding di 10px, margin di 10px e colore di sfondo come in figura

#### Esercizio 2:
Scrivi il codice HTML e il relativo CSS per ottenere una griglia fatta come in figura.

![Esercizio 2](/images/web/grid/esercizio2.png){:class="half-image"}

Scrivi poi il codice CSS affiché:

* l'header abbia un bordo di 2px tratteggiato di colore nero, padding di 10px, margin di 20px e colore di sfondo come in figura
* il main abbia un bordo di 2px continuo di colore verde scuro, padding di 10px, margin di 20px e colore di sfondo come in figura
* la sidebar abbia un bordo di 1px a puntini di colore blù scuro, padding di 30px, margin di 20px e colore di sfondo come in figura
* il footer abbia un bordo di 3px a puntini di colore viola scuro, padding di 10px, margin di 20px e colore di sfondo come in figura



#### Esercizio 3:

![Esercizio](/images/web/esercizi/paginaesercizio1.png)

Scarica gli assets: [Scarica]({{ site.baseurl }} ../myassets/web/agenzia.zip)

#### Esercizio 4

Inserisci Il seguente codice nell'esercizio precedente al fine di ottenere un menù ben fatto. 

![Esercizio](/images/web/esercizi/menu.png)

{% highlight css %}
.menu {
  list-style-type: none;
  margin: 0;
  padding: 0;
  overflow: hidden;
  background-color: #333333;
}

.elementomenu {
  float: left;
}

.linkmenu {
  display: block;
  color: white;
  text-align: center;
  padding: 14px 16px;
  text-decoration: none;
}

.linkmenu:hover {
  background-color: #111111;
}
{% endhighlight %}


{% highlight html %}
<ul class="menu">
  <li class="elementomenu"><a href="index.html" class="linkmenu">Home</a></li>
  <li class="elementomenu"><a href="novita.html" class="linkmenu">Novità</a></li>
  <li class="elementomenu"><a href="contatti.html" class="linkmenu">Contatti</a></li>
  <li class="elementomenu"><a href="informazioni.html" class="linkmenu">Informazioni</a></li>
</ul>
{% endhighlight %}


#### Esercizio 5:

![Esercizio](/images/web/esercizi/paginaesercizio2.jpg)

Scarica gli assets: [Scarica]({{ site.baseurl }} ../myassets/web/assets2.zip)

###### Testi:

the good ritual

THE GOOD RITUAL

"Our vision is to empower busy women by providing convenient products that come with added health benefits."

We are launching with an instant, functional coffee that enhances our customer's everyday lite it's cate quality, in a convenient packet Were supplemented the collee with L-thonanine, which is an all-natural nootropic that improves cognitive funciton, roduces mental fatigue, and roduces stress. As wo expand our product line, all of our afferings will be supplemented with all-notural ingredients that enhance health

Colori sfondi: 

* testata: #F9F6E3 
* giallo: #FFD976 
* rosa: #FDCDC1 
* verdino: #C7D9A7 
* azzurrino: #D0EFE9

Colori testi: 

* verde scuro: #0D3032
* verde chiaro: #7B937B

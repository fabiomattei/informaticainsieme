---
title: Tag HTML
date: '2026-01-08T21:00:02+01:00'
author: Fabio Mattei
layout: page
---

Nell'[introduzione]({{ site.baseurl }}{% link _web/html.md %}.html) abbiamo cominciato a prendere coscienza della logica che guida la struttura del linguaggio html. Ora cominciamo a conoscere i TAG principali che vengono utilizzati per 
costruire le pagine.

![Pagina di una rivista etichettata](/images/web/html/pagina-taggata.jpg)

#### Intestazioni

Il TAG H va a indicare le intestazioni contenute all'interno di un documento HTML. 
H è inteso come acronimo di **heading**. Il numero 
che lo segue va a definire il peso che questa intestazione ricopre all'interno del testo. 
I numeri più bassi vanno a definire le intestazioni più importanti, i numeri più bassi quelle meno importanti. 

Notiamo nell'immagine in alto che il **peso** di una intestazione si rispecchia nella grandezza del carattere, 
nell'utilizzazione dello stile grassetto e, qualche volta, nel colore con cui il testo è scritto o nel colore 
del suo sfondo.

{% highlight html %}
<h1>Intestazione di livello più alto</h1>
<h2>Intestazione di livello due</h2>
<h3>Intestazione di livello tre</h3>
<h4>Intestazione di livello quattro</h4>
<h5>Intestazione di livello cinque</h5>
<h6>Intestazione di livello più basso</h6>
{% endhighlight %}

#### Paragrafi

Il paragrafo è una porzione di testo, formata da uno i più periodi, isolata visivamente dal testo che lo precede e da quello che lo segue.

{% highlight html %}
<p>Un paragrafo in HTML è una porzione di testo, formata da uno i più periodi, 
isolata visivamente dal testo che lo precede e da quello che lo segue.</p>
{% endhighlight %}

Possiamo notare che il testo nel paragrafo è circondato dalle etichette &lt;p&gt; e &lt;/p&gt;. Queste etichette determinano l’inizio e la fine del paragrafo.

Il tag **P** è acronimo di **paragraph**.

#### Elenchi puntati 

Gli elenchi puntati sono elenchi formati da elementi il cui ordine non ha particolare importanza.

Possiamo pensare alla lista della spesa come esempio di elenco puntato.

{% highlight html %}
<ul>
  <li>Carote</li>
  <li>Mele</li>
  <li>Pane</li>
</ul>
{% endhighlight %}

La renderizzazione, dal parte del browser, è la seguente:

* Carote
* Mele
* Pane

Il TAG **UL** è acronimo di **unordered list** il tag **LI** di **list item**

#### Elenchi  numerati 

Un elenco numerato è una lista all'interno della quale l'ordine degli elementi che la compongono è molto importante.

Un esempio di elenco numerato è l'arrivo di un gran premio di formula 1. 

{% highlight html %}
<ol>
  <li>Verstappen M.</li>
  <li>Piastri O.</li>
  <li>Norris L.</li>
</ol>
{% endhighlight %}

La renderizzazione, dal parte del browser, è la seguente:

1. Verstappen M.
2. Piastri O.
3. Norris L.

Il TAG **OL** è acronimo di **ordered list** il tag **LI** di **list item**

#### Formattazione del testo

E' possible formattare il testo scritto all'interno dei paragrafi, delle liste e in generale
all'interno di quasi tutti i tag utilizzando i seguenti tag.

| Tag | Descrizione | Resa di base |
|---|---|---|
| &lt;strong&gt; | Attribuisce al testo una forte importanza, serietà o urgenza | Grassetto |
| &lt;b&gt; | Offre una differenza stilistica rispetto al resto del contenuto, senza attribuire un’importanza specifica al testo | Grassetto |
| &lt;em&gt; | Enfatizza un testo | Corsivo |
| &lt;i&gt; | Offre una differenza stilistica rispetto al resto del contenuto, senza attribuire un’importanza specifica al testo | Corsivo |
| &lt;u&gt; | Rende un testo sottolineato | Sottolineato |

Esempio

{% highlight html %}
<p>Un paragrafo in <strong>HTML</strong> è una porzione di testo, formata da uno i più <em>periodi</em>, 
isolata visivamente dal testo che lo precede e da quello che lo segue.</p>
{% endhighlight %}

La renderizzazione, dal parte del browser, è la seguente:

Un paragrafo in **HTML** è una porzione di testo, formata da uno i più *periodi*, 
isolata visivamente dal testo che lo precede e da quello che lo segue.

#### Le tabelle 

La tabella è utile quando si vogliono inserire in una pagina web dati di qualsiasi genere.


{% highlight html %}
<table>
    <caption>
        <p>I miei dati</p>
    </caption>
    <thead>
        <tr><th>Colonna 1</th><th>Colonna 2</th></tr>
    </thead>
    <tbody>
        <tr><td>Dato 1,1</td><td>Dato 1,2</td></tr>
        <tr><td>Dato 2,1</td><td>Dato 2,2</td></tr>
        <tr><td>Dato 3,1</td><td>Dato 3,2</td></tr>
    </tbody>
    <tfoot>
        <tr><td>Totale 1</td><td>Totale 2</td></tr>
    </tfoot>
</table>
{% endhighlight %}

Ogni tabella può avere un testo descrittivo all’interno del tag &lt;caption&gt;. Normalmente questo si scrive all’interno di un paragrafo.

Una tabella è contrassegnata dal tag &lt;table&gt; ed è divisa in tre sezioni &lt;thead&gt;, &lt;tbody&gt; e &lt;tfoot&gt;.

- **thead**: sezione di testa della tabella, contiene i titoli delle colonne che formano la tabella
- **tbody**: sezione corpo, contiene i dati che voglio mostrare nella tabella
- **tfoot**: sezione ai piedi della tabella, contiene ciò che voglio mostrare a fine tabella, ad esempio una somma dei dati contenuti nella tabella

In una tabella le informazioni sono organizzate in righe e colonne. Ogni riga è contrassegnata da un &lt;tr&gt; e ogni colonna da un &lt;td&gt;. Al fine di scrivere un contenuto si apre un tag &lt;tr&gt;, si inserisce il dato all’interno di un tag &lt;td&gt; per ciascuno di questi avendo cura di chiuderlo &lt;/td&gt; subito dopo, in fine si chiude il tag &lt;/tr&gt;

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

La tabella è utile quando si ha la necessità di inserire in una pagina web dati di qualsiasi genere.

*Tabella dei voti della 3C*

| Studente | Italiano | Matematica |
|---|---|---|
| Francesco | 8 | 7 |
| Giulia | 7 | 9 |
| Michele | 6 | 6 |
| Angela | 8 | 9 |
|---|---|---|
| Nome | Decimi | Decimi |

Il codice per trascrivere in HTML la tabella in alto è il seguente

{% highlight html %}
<table>
    <caption>
        <p>Tabella dei voti della 3C</p>
    </caption>
    <thead>
        <tr><th>Studente</th><th>Italiano</th><th>Matematica</th></tr>
    </thead>
    <tbody>
        <tr><td>Francesco</td><td>8</td><td>7</td></tr>
        <tr><td>Giulia</td><td>7</td><td>9</td></tr>
        <tr><td>Michele</td><td>6</td><td>6</td></tr>
        <tr><td>Angela</td><td>8</td><td>9</td></tr>
    </tbody>
    <tfoot>
        <tr><th>Nome</th><th>Decimi</th><th>Decimi</th></tr>
    </tfoot>
</table>
{% endhighlight %}

Ogni tabella può avere un testo descrittivo all’interno del tag &lt;caption&gt;. 
Normalmente questo si scrive all’interno di un paragrafo.

Una tabella è contrassegnata dal tag &lt;table&gt; ed è divisa in tre sezioni &lt;thead&gt;, &lt;tbody&gt; e &lt;tfoot&gt;.

- **thead**: sezione di testa della tabella, contiene i titoli delle colonne che formano la tabella
- **tbody**: sezione corpo, contiene i dati che voglio mostrare nella tabella
- **tfoot**: sezione ai piedi della tabella, contiene ciò che voglio mostrare a fine tabella, ad esempio una somma dei dati contenuti nella tabella

All'interno di una sezione di una tabella le informazioni sono organizzate in primo luogo in righe e in seconda battuta in colonne.

Ogni riga è contrassegnata dal tag &lt;tr&gt; che è l'acronimo di **table row**.

All'interno delle righe, vanno delimitate le colonne. 
Se la colonna si trova nella sezione thead o tfoot questa è connotata dal tag &lt;th&gt; (table heading), 
se invece si trova nella sezione tbody questa è connotata dal tag &lt;td&gt; (table data). 










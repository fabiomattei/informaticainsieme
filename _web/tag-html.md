---
title: Tag HTML
date: '2026-01-08T21:00:02+01:00'
author: Fabio Mattei
layout: page
---

#### Titoli

{% highlight html %}
<h1>Titolo di livello più alto</h1>
<h2>Titolo di livello due</h2>
<h3>Titolo di livello tre</h3>
<h4>Titolo di livello quattro</h4>
<h5>Titolo di livello cinque</h5>
<h6>Titolo di livello più basso</h6>
{% endhighlight %}

I tag che vanno da H1 ad H6 permettono di scrivere i titoli. 
Ci sono 6 diversi livelli di titoli che permettono allo scrittore di sottolineare l’importanza che ciascuno di questi riveste all’interno del testo.

#### Paragrafi

{% highlight html %}
<p>Un paragrafo in HTML è una sezione di testo 
circondata dal tag p</p>
{% endhighlight %}

Possiamo notare che il testo nel paragrafo è circondato dalle etichette &lt;p&gt; e &lt;/p&gt;. Queste etichette determinano l’inizio e la fine del paragrafo.

#### Formattazione del testo

Si applica all’interno dei paragrafi &lt;p&gt; e di altre sezioni di testo.

| Tag | Descrizione | Resa di base |
|---|---|---|
| &lt;strong&gt; | Attribuisce al testo una forte importanza, serietà o urgenza | Grassetto |
| &lt;b&gt; | Offre una differenza stilistica rispetto al resto del contenuto, senza attribuire un’importanza specifica al testo | Grassetto |
| &lt;em&gt; | Enfatizza un testo | Corsivo |
| &lt;i&gt; | Offre una differenza stilistica rispetto al resto del contenuto, senza attribuire un’importanza specifica al testo | Corsivo |
| &lt;u&gt; | Rende un testo sottolineato | Sottolineato |

#### Elenchi puntati e numerati 

Elenco puntato


{% highlight html %}
<ul>
  <li>primo elemento</li>
  <li>secondo elemento</li>
  <li>terzo elemento</li>
</ul>
{% endhighlight %}

Elenco numerato


{% highlight html %}
<ol>
  <li>primo elemento</li>
  <li>secondo elemento</li>
  <li>terzo elemento</li>
</ol>
{% endhighlight %}

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

---
id: 635
title: 'HTML e CSS: organizziamo il codice'
date: '2021-05-22T06:08:50+02:00'
author: Fabio Mattei
layout: page
guid: 'https://www.esercizidiinformatica.it/?page_id=635'
---

Abbiamo imparato che gli stili css vanno inseriti nella sezione **STYLE** contenuto nell'**HEAD** del documento HTML.
In questo esempio abbiamo definito due selettori per ID (pagina, titolo) e un selettore di classe (conteuto).
I selettori definiti vengono poi utilizzati nel corpo della pagina (body).

{% highlight html %}
<html lang="it">
<head>
  <meta charset="utf-8">
  <title>L'atomo</title>
  <style>
     #pagina { background-color: powderblue; }
     #titolo { color: blue; } 
     .contenuto { color: red; }
  </style>
</head>
<body id="pagina">
  <h1 id="titolo">Il modello atomico di Bohr</h1>
  <p class="contenuto">Gli elettroni ruotano attorno al nucleo, e le orbite da loro descritte sono a 
     una distanza ben precisa dal nucleo, che dipende dalla quantità di energia, chiamati livelli energetici.</p>
  <p class="contenuto">Ogni elettrone segue una determinata traiettoria circolare, chiamata orbita stazionaria.</p>
</body>
</html>
{% endhighlight %}

### Organizziamo il CSS in un file esterno

E' possibile creare un file stili.css che andrà a contenere tutto quanto definito nella sezione STYLE.

{% highlight css %}
#pagina { background-color: powderblue; }
#titolo { color: blue; } 
.contenuto { color: red; }
{% endhighlight %}

Fatto questo andremo a **cancellare il tag STYLE** e al suo posto inseriremo un **LINK** che permetterà
al documento HTML di caricare gli stili definiti nel file appena creato.

{% highlight html %}
<html lang="it">
<head>
  <meta charset="utf-8">
  <title>L'atomo</title>
  <link rel="stylesheet" href="stili.css">
</head>
<body>
  <h1 id="titolo">Il modello atomico di Bohr</h1>
  <p class="contenuto">Gli elettroni ruotano attorno al nucleo, e le orbite da loro descritte sono a 
     una distanza ben precisa dal nucleo, che dipende dalla quantità di energia, chiamati livelli energetici.</p>
  <p class="contenuto">Ogni elettrone segue una determinata traiettoria circolare, chiamata orbita stazionaria.</p>
</body>
</html>
{% endhighlight %}

In basso è visibile il contenuto dei file stili.css che per poter funzionare dovrà trovarsi nella stessa cartella
in cui si trova il file HTML.

![Cartella](/images/web/cartella.png)

### Vantaggi

I vantaggi di questo approccio sono i seguenti
Definendo gli stili in un file esterno possiamo utilizzare gli stessi stili su più file HTML. 
In questo modo dobbiamo lavorare meno ma soprattutto avremo una grafica uniforme tra tutte le pagine
HTML che compongono il nostro sito.

Infatti dato che, per un sito web, il CSS è comune a più pagine, è bene metterlo in un file esterno in modo da 
poterlo riutilizzare e non essere cotretti a ripeterlo per ogni pagina.




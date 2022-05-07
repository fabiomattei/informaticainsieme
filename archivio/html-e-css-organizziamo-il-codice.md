---
id: 635
title: 'HTML e CSS: organizziamo il codice'
date: '2021-05-22T06:08:50+02:00'
author: fabio
layout: page
guid: 'https://www.esercizidiinformatica.it/?page_id=635'
---

Fino ad ora abbiamo visto gli elementi html separati gli uni dagli altri, ma in effetti ogni documento contiene una pagina completa e ciascuna pagina ha la struttura che vedete in alto contraddistinta dal titolo dalla testata (head) e dal corpo (body).

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="html" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">
<html lang="id">
<head>
  <meta charset="utf-8">
  <title></title>
</head>
<body>
  
</body>
</html>
```

</div>I tag H1..H6, P, UL ecc compongono il contenuto della pagina e vanno nella sezione body.

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="html" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">
<html lang="id">
<head>
  <meta charset="utf-8">
  <title>L'atomo</title>
</head>
<body>
  <h1>Il modello atomico di Bohr</h1>
  <p>Gli elettroni ruotano attorno al nucleo, e le orbite da loro descritte sono a una distanza ben precisa dal nucleo, che dipende dalla quantità di energia, chiamati livelli energetici.</p>
  <p>Ogni elettrone segue una determinata traiettoria circolare, chiamata orbita stazionaria.</p>
</body>
</html>
```

</div>Nella sezionie head invece vanno definite cose come il titolo della pagina, l’autore, la data di scadenza.

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="html" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">
<html lang="id">
<head>
  <meta charset="utf-8">
  <title>L'atomo</title>
  <style>
     body {background-color: powderblue;}
     h1   {color: blue;} 
     p    {color: red;}
  </style>
</head>
<body>
  <h1>Il modello atomico di Bohr</h1>
  <p>Gli elettroni ruotano attorno al nucleo, e le orbite da loro descritte sono a una distanza ben precisa dal nucleo, che dipende dalla quantità di energia, chiamati livelli energetici.</p>
  <p>Ogni elettrone segue una determinata traiettoria circolare, chiamata orbita stazionaria.</p>
</body>
</html>
```

</div>All’interno della sezione head posso definire gli stili CSS all’interno del tag **style**.

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="html" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">
<html lang="id">
<head>
  <meta charset="utf-8">
  <title>L'atomo</title>
  <link rel="stylesheet" href="stili.css">
</head>
<body>
  <h1>Il modello atomico di Bohr</h1>
  <p>Gli elettroni ruotano attorno al nucleo, e le orbite da loro descritte sono a una distanza ben precisa dal nucleo, che dipende dalla quantità di energia, chiamati livelli energetici.</p>
  <p>Ogni elettrone segue una determinata traiettoria circolare, chiamata orbita stazionaria.</p>
</body>
</html>
```

</div>Ma è più opportuno definire gli stili in un file esterno in modo da poter utilizzare gli stessi stili su più file HTML.

#### CSS in un file esterno

Dato che per un sito web, il CSS è comune a più pagine, è bene metterlo in un file esterno in modo da poterlo riutilizzare e non essere cotretti a ripeterlo per ogni pagina.

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="html" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">
<html lang="id">
<head>
  <meta charset="utf-8">
  <title>L'atomo</title>
  <link rel="stylesheet" href="miostile.css">
</head>
<body>
  <h1>Il modello atomico di Bohr</h1>
  <p>Gli elettroni ruotano attorno al nucleo, e le orbite da loro descritte sono a una distanza ben precisa dal nucleo, che dipende dalla quantità di energia, chiamati livelli energetici.</p>
  <p>Ogni elettrone segue una determinata traiettoria circolare, chiamata orbita stazionaria.</p>
</body>
</html>
```

</div>A questo proposito creiamo il file miostile.css e mettiamolo nella root del sito web.

</body></html>
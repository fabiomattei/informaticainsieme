---
title: HTML
date: '2020-02-10T21:29:02+01:00'
author: Fabio Mattei
layout: page
---

HTML (Hyper text markup language – Linguaggio ipertestuale a marcatori) è un linguaggio inventato da **Tim Berners Lee** nel 1989 e diffuso al pubblico nel 1993 per avere la *libertà di pubblicare le sue ricerche senza dover passare attraverso il controllo degli editori delle riviste scientifiche*. 
Tim era un ricercatore che lavorava al CERN di Ginevra ed aveva difficoltà a far pubblicare i suoi articoli. Nella sua ricerca di libertà inventò il WEB.

- Hyper text: indica il fatto che la fruizione dei contenuti non avviene necessariamente in modo lineare, come avviene in un testo scritto, ma cliccando sui link ogni lettore può creare un percorso unico diverso da quello di tutti gli altri lettori
- Markup: indica il fatto che il linguaggio **etichetta** le sezioni di testo utilizzando i **tag**
- Language: indica il fatto che è un linguaggio vero e proprio dotato di propri termini, grammatica e sintassi

Il linguaggio HTML si fonda su una idea semplice: etichettare sezioni di testo in un file testuale al fine di dare loro un livello semantico e al fine di visualizzarle propriamente.

### Documento HTML

Un documento HTML non è altro che un file di testo, cioè un file composto esclusivamente da caratteri ASCII 
privo di alcuna formattazione,

Si intende come **struttura base** di un documento HTML un file che contiene quanto necessario per iniziare a lavorare.
Al suo interno si trovano i TAG che devono essere necessariamente presenti in ogni pagina, 
i contenuti sono tutti vuoti ma tutto è pronto per iniziare la compilazione. 

{% highlight html %}
<html lang="it">
  <head>
    <meta charset="utf-8">
    <title></title>
  </head>
  <body>
  
  </body>
</html>
{% endhighlight %}

Se iniziamo a lavorare nella pagina inseriremo il titolo e qualche informazione nella sezione body.

{% highlight html %}

<html lang="it">
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
{% endhighlight %}



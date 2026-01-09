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

### I tag html

Il **tag html**, o etichetta, è riconosbili perché circondato dai simboli &lt;tag&gt;.

Esempi di tag sono: &lt;p&gt;, &lt;ul&gt;, &lt;h1&gt;

Dobbiamo pensare ai tag come ad etichette che "etichettano" i contenuti che dobbiamo inserire in una pagina.

##### Facciamo un esempio

Qui in basso potete vedere l'immagina di due pagine di una rivista.
Se guardiamo con attenzione possiamo riconoscere i vari elementi:

* titoli
* sottotili
* testi
* immagini

![Pagina di una rivista](/images/web/html/pagina-prima.jpg)

[Designed by Freepik](http://www.freepik.com)

In basso possiamo vedere una pagina che viene, come si dice in gergo, **"taggata"** in modo da individuare 
i vari elementi che ne determinano i contenuti.

![Pagina di una rivista etichettata](/images/web/html/pagina-taggata.jpg)

Si può notare che il **titolo** più importante viene individuato dal tag **H1**, il **sottotitolo** dall tag **H2**, 
il **sotto-sottotitolo** dal tag **H3**.
Il testo viene connotato dal tag **p** e l'immagine dal tag **img**.


La pagina appena descritta, **codificata in HTML**, diventa:

{% highlight html %}
<h1>LOREM IPSUM DOLOR SIT AMET.</h1>
<h2>LOREM IPSUM DOLOR</h2>
<h3>Lorem ipsum dolor sit amet.</h3>
<p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Praesent interdum turpis
eget scelerisque iaculis. Nam lacus erat, eu-ismod nec dolor eu, posuere lacinia leo.
Duis sit amet facilisis lorem, sed volutpat ligula. 
Curabitur dapibus sem non feugiat placerat. Aenean placerat dui eu condimen-tum dignissim. 
Phasellus id nulla nec sem ullamcorper sagittis.</p>
<img src="nomme-immagine.jpg"></img>
<p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Praesent interdum turpis
eget scelerisque iaculis. Nam lacus erat,
euismod nec dolor eu, posuere lacinia leo.
Duis sit amet facilisis lorem, sed volutpat
ligula. Curabitur dapibus sem non feugiat placerat. Aenean placerat dui eu condi-mentum dignissim. 
Phasellus id nulla nec sem ullamcorper sagittis. Curabitur pretium neque vitae faucibus gravida.
Nunc euismod tempus magna vel volutpat.
Morbi vel posuere mauris. Cras pellen-tesque dolor bibendum.</p>
{% endhighlight %}

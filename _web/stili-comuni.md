---
title: 'Stili comuni'
date: '2026-01-09T11:44:47+02:00'
author: Fabio Mattei
layout: page
---

Esistono centinaia le proprietà css che ci danno la possibilità di rendere il sito che vogliamo cotruire unico. 
Tra queste però sono poche decine quelei che vengono utilizzate dai grafici professionisti nella maggior parte dei
casi. Impariamo a conoscerle.

### Font family

Questa proprietà ci dà la possibilità di decidere con quale carattere voglia scrivere il testo.
Notiamo che quando viene impostata si utilizza più di un valore.

Dato che il font per visualizzare la scritta deve risiedere nella macchina che renderizza il sito potrebbe accadere,
se si esprimesse una sola proferenza, che il font selezionato non sia presente nella macchina e in quel caso
il browser andrebbe ad utilizzare il font di default del sistema. 

Per ovviare a questo problema viene data la possibilità di esprimere più preferenze in ordine di priorità. 
Questo significa che vengono indicati più font e viene utilizzato il primo tra questi che è presente nel sistema.

{% highlight html %}
.p1 {
  font-family: "Times New Roman", Times, serif;
}

.p2 {
  font-family: Arial, Helvetica, sans-serif;
}

.p3 {
  font-family: "Lucida Console", "Courier New", monospace;
}
{% endhighlight %}

Nel primo esempio viene scelto il font Times new roman, se questo non è presente nel sistem si utilizza il font 
times, se anche quest'ultimo non è presente nel sistema si utilizza un font sans-serif.

* **Serif**: Font dotati di grazie. Danno un senso di formalità ed eleganza. Spesso utilizzati nel corpo del testo.
* **Sans-serif**: Font che non hanno grazie. Per uno stile moderno e minimalista. Spesso utilizzati nei titoli.
* **Monospace**: Ogni carattere occupa sempre lo stesso spazio. Utilizzato per il codice per computer.
* **Cursive**: Caratteri che imitano la scrittura umana.
* **Fantasy**: Caratteri decorativi.

Lisa di alcuni tra i font più utilizzati in rete

* Arial (sans-serif)
* Verdana (sans-serif)
* Tahoma (sans-serif)
* Trebuchet MS (sans-serif)
* Times New Roman (serif)
* Georgia (serif)
* Garamond (serif)
* Courier New (monospace)
* Brush Script MT (cursive)

### Font size

Questa proprietà permette di indicare la dimensione del carattere di una scritta. 

{% highlight html %}
<p style="font-size: 30px;">L'Italia è un paese bellissimo</p>
{% endhighlight %}

Le dimensioni posso indicate in modo assoluto o relativo:

Unità di misura assolute:

* px: Pixel
* xx-small, x-small, small, medium, large, x-large, xx-large Queste parole chiave permettono di indicare una misura precisa impostata nel broser

Unità di misura relative:

* em: misura proporzionale relativa al tag contenitore (padre) del tag su cui stiamo lavorando 
* rem: misura proporzionale relativa al tag root (radice) della pagina HTML
* %: misura relativa al tag contenitore (padre) del tag su cui stiamo lavorando in percentuale
* smaller e larger: queste parole aggiustano la dimensione rispetto al tag contenitore

Come scegliere la giusta unità di misura?

Se si vuole un controllo precise si può utilizzare px.
Tuttavia utilizzando px la pagina non andrà ad aggiustare la dimensione del font se si usano schermi di dimensione differente da quello su cui è stato progettato il sito.
Se si vuole fare un design **scalabile** (che si adatta alla dimensione dello schermo meglio utilizzare em, rem o %. 

### Font weight

Permette di impostare lo spessore, o peso, di un carattere

{% highlight html %}
<p style="font-weight: bold;">L'Italia è un paese bellissimo</p>
{% endhighlight %}

Valori possibili

* normal: peso normale del carattere
* bold: carattere grassetto
* bolder: carattere "grassettissimo" :-)
* lighter: carattere meno pesante del normale

Si può anche impostare un valore numerico:

* 100
* 200
* 300
* 400: corrisponde al normal
* 500
* 600
* 700: corrisponde al bold
* 800
* 900

### Text align

Peremette di impostare l'allineamento del testo

{% highlight html %}
<p style="text-align: right;">L'Italia è un paese bellissimo</p>
{% endhighlight %}

Possibili valori: 

* left: allineamento a sinistra
* right: allineamento a destra
* center: allineamento al centro
* justify: allineamento giustificato

### Text decoration

Si può decorare il testo utilizzando quattro diverse modalità

{% highlight html %}
h1 {
  text-decoration: underline overline dotted red 30%;
}

h2 {
  text-decoration: underline wavy blue 5px;
}
{% endhighlight %}

Linea

* none: nessuna
* underline: linea sotto al testo
* overline: linea sopra al testo
* line-throughlinea attraverso il testo

Colore

* red 
* blue
* white
* .... tutti i colori

Stile

* solid: linea normale
* double: linea doppia
* dotted: linea punteggiata
* dashed: linea tratteggiata
* wavy: linea ondeggiante

Spessore (Spessore della linea che può essere espresso in):

* pixel
* percentuale

### Color

Permette di impostare il colore del testo

{% highlight html %}
<p style="color: red;">L'Italia è un paese bellissimo</p>
{% endhighlight %}

![Colors](/images/web/colors.png)

### Background-color

Permette di impostare il colore dello sfondo

{% highlight html %}
<p style="background-color: red;">L'Italia è un paese bellissimo</p>
{% endhighlight %}



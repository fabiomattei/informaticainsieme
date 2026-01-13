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

* **Serif fonts**: Font dotati di grazie. Danno un senso di formalità ed eleganza. Spesso utilizzati nel corpo del testo.
* **Sans-serif fonts**: Font che non hanno grazie. Per uno stile moderno e minimalista. Spesso utilizzati nei titoli.
* **Monospace fonts**: Ogni carattere occupa sempre lo stesso spazio. Utilizzato per il codice per computer.
* **Cursive fonts**: Caratteri che imitano la scrittura umana.
* **Fantasy fonts**: Caratteri decorativi.

### Font size

Questa proprietà permette di indicare la dimensione del carattere di una scritta. 

{% highlight html %}
<p style="font-size: 30px;">L'Italia è un paese bellissimo</p>
{% endhighlight %}

### Font weight

font-weight: bold;

### Text align

text-align: right;

### Text decoration

text-decoration: line-through;

### Color

### Background-color



---
title: 'Dragonruby: labels'
date: '2020-11-15T08:47:01+01:00'
author: Fabio Mattei
layout: page
---

Le labels sono utile per avere delle scritte sullo schermo. 

Vediamo subito un esempio:

{% highlight ruby %}
mylabel = {
    +x: 320,
    y: 640,
    text: "Text",
    font: "fonts/font.ttf",
    anchor_x: 0.5, # or alignment_enum: 0, 1, or 2
    anchor_y: 0.5, # or vertical_alignment_enum: 0, 1, or 2
    r: 0,
    g: 0,
    b: 0,
    a: 255,
    size_px: 20,   # or size_enum: -10 to 10 (0 means "ledgible on small devices" ie: 20px)
    blendmode_enum: 1 
}
{% endhighlight %}

* x e y rappresentano le coordinate sul piano cartesiano in cui posizionare la label
* text contiene il testo che andrà ad essere visualizzato
* font contiene il path, nella caretella mygame, del font che si utilizzerà per la scritta
* anchor_x e anchor_y contengono la posizione dell'ancora della scritta
* r g e b contengono i valori da 0 a 255 per determinare il colore della scritta
* a contiene l'opacità della scritta

## Labels e variabili

Le labels sono molto utili per mostrare ad esempio il punteggio di un giocatore 
o il tempo rimanente.
Per questo motivo dobbiamo essere in grado di mostrare delle variabili al loro interno.

{% highlight ruby %}
def tick args
  # mostra il numero di tick corrente
  args.outputs.labels << {
	  x: 640, 
	  y: 650, 
	  text: "frame: #{Kernel.tick_count}",
  }
end
{% endhighlight %}

Se il punteggio fosse contenuto nella variabile **args.state.punteggio**:

{% highlight ruby %}
def tick args
  args.state.punteggio ||= 0
  args.outputs.labels << {
	  x: 640, 
	  y: 650, 
	  text: "punteggio: #{args.state.punteggio}",
  }
end
{% endhighlight %}

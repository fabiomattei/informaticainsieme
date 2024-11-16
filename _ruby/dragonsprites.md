---
title: 'Gli sprites'
date: '2020-11-15T08:47:01+01:00'
author: Fabio Mattei
layout: page
---

Gli sprite sono gli elementi grafici che possiamo utilizzare nel nostro videogioco.

Uno sprite si definisce attraverso un dizionario, guardiamo l'esempio seguente

{% highlight ruby %}
mysprite = {
  # proprietà comuni
  x: 0,
  y: 0,
  w: 100,
  h: 100,
  path: "sprites/square/blue.png",
  angle: 90,
  a: 255,
  
  # anchoring (valore float che rappresenta una percentuale da compensare di w e h)
  anchor_x: 0,
  anchor_y: 0,
  angle_anchor_x: 0,
  angle_anchor_y: 0,

  # saturazione del colore
  r: 255,
  g: 255,
  b: 255,

  # capovolgi il rendering orizzontalmente o verticalmente
  flip_horizontally: false,
  flip_vertically: false

  # proprietà di uno sprite sheet per ritagliare un rettangolo (utilizzando l'angolo in alto a sinistra come origine)
  tile_x: 0,
  tile_y: 0,
  tile_w: 20,
  tile_h: 20

  # proprietà di uno sprite sheet per ritagliare un rettangolo (utilizzando l'angolo in basso a sinistra come origine)
  source_x: 0,
  source_y: 0,
  source_w: 20,
  source_h: 20,
}
{% endhighlight %}

Cominciamo dal descrivere le proprietà più comuni:

* x: ascissa della posizione dello sprite
* y: ordinata della posizione dello sprite
* w: larghezza dell'immagine dello sprite
* h: altezza dell'immagine dello sprite
* path: percorso del file in cui si trova l'immagine dello sprite a partire dalla cartella **my game**
* angle: angolo di rotazione dello sprite
* a: opacità dello sprite

## Anchoring

* anchor_x valore float compreso tra 0 e 1 per modificare il punto di ancoraggio rispetto all'asse x
* anchor_y valore float compreso tra 0 e 1 per modificare il punto di ancoraggio rispetto all'asse y
* angle_anchor_x
* angle_anchor_y

Ogni sprite che poniamo sullo schermo ha di default il suo punto di ancoraggio posizionato nell'estremo 
in basso a sinistra della sua immagine.

Questo punto di ancoraggio viene utilizzato come punto da tenere in considerazione quando vado a posizionare
lo sprite sullo schermo. 

![Il loop](/images/ruby/dragonruby/anchor.png)

![Il loop](/images/ruby/dragonruby/anchorrotation.png)

## Capovolgi (flip)


## Sprite sheet



  

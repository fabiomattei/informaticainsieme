---
title: 'Dragonruby collisioni tra sprites'
date: '2020-11-16T08:47:01+01:00'
author: Fabio Mattei
layout: page
---

Per monitorare l'interazione tra i vari sprite sullo schermo dobbiamo monitorare le loro **collisioni**.

Ad esempio se un proiettile **tocca**, oppure potremmo dire "ha una collisione", con un personaggio, il
personaggio poi muore. 

Dato che ogni elemento che sta sullo schermo è uno sprite, ed ogni sprite è descritto dai quattro valori:

* x
* y
* w
* h

quello che in effetti dobbiamo valutare è se il rettangolo che definisce lo sprite del proiettile va ad
intersecare il rettangolo che definisce il personaggio. 

Per velocizzare il tutto dragonruby ci fornisce il metodo **intersect_rect?**.

intersect_rect? è un metodo che troviamo nelle liste e che restituisce **true** se due rettangoli si toccano:


lista#intersect_rect?: una lista con almeno 4 valori è considerata un rettangolo

{% highlight ruby %}
def tick args
  # Rettangolo uno: x: 100, y: 100, w: 100, h: 100
  # Rettangolo due: x: 0, y: 0, w: 500, h: 500
  # Risultato:   true

  [100, 100, 100, 100].intersect_rect? [0, 0, 500, 500]
  
  # Rettangolo uno: x: 100, y: 100, w: 10, h: 10
  # Rettangolo due: x: 500, y: 500, w: 10, h: 10
  # Risultato:   false

  [100, 100, 10, 10].intersect_rect? [500, 500, 10, 10]
end
{% endhighlight %}

Un secondo modo di chiamarlo consiste nello sfruttare i dizionari. 

Nel codice che segue ci sono tre esempi:

{% highlight ruby %}
def tick args
  # definisci i rettangoli attraverso i dizionari
  rettangolo_1 = { x: 0, y: 0, w: 100, h: 100 }
  rettangolo_2 = { x: 50, y: 50, w: 100, h: 100 }

  # variante mixin
  # chiama il metodo intersect da una istanza di dizionario
  puts rettangolo_1.intersect_rect?(rettangolo_2)

  # oppure

  # varianti module 
  puts args.geometry.intersect_rect?(rettangolo_1, rettangolo_2)
  puts Geometry.intersect_rect?(rettangolo_1, rettangolo_2)
end
{% endhighlight %}

E' importante tenere a mente che ad ogni frame, quindi ogni sessantesimo di secondo, 
vanno ricarlcolate tutte le possibili interazioni tra tutti gli sprite prensenti nel videgioco.
Con l'aumentare degli sprite la cosa diventa computazionalmente onerosa.


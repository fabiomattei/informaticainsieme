---
title: L'algoritmo di Kruskal
date: '2021-09-30T10:58:13+02:00'
author: Fabio Mattei
layout: page
---

L'algoritmo di Kruskal è un algoritmo per calcolare l'albero di copertura minimo (minimum spanning tree o MST) di un un grafo non orientato.

Per collegare n nodi dobbiamo utilizzare n - 1 rami.

* All'inizio si sceglie il ramo con il peso più basso e si collegano i primi due nodi
* Si guardano tutti i rami uscenti dai nodi già collegati e si sceglie il più basso che crei una connessione ad un nodo ancora non connesso.

Si può dimostrare che utilizzando questo algoritmo si può trovare la soluzione ottima.

![Kruskal, grafo iniziale](/informaticainsieme/images/algoritmi/kruskal/kruskal01.png){:class="aside-image"}

![Kruskal, primo ramo](/informaticainsieme/images/algoritmi/kruskal/kruskal02.png){:class="aside-image"}

![Kruskal, secondo ramo](/informaticainsieme/images/algoritmi/kruskal/kruskal03.png){:class="aside-image"}

![Kruskal, terzo ramo](/informaticainsieme/images/algoritmi/kruskal/kruskal04.png){:class="aside-image"}

![Kruskal, quarto ramo](/informaticainsieme/images/algoritmi/kruskal/kruskal05.png){:class="aside-image"}

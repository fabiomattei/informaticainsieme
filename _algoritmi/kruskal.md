---
title: L'algoritmo di Kruskal
date: '2021-09-30T10:58:13+02:00'
author: Fabio Mattei
layout: page
---

L'algoritmo di Kruskal è un algoritmo per calcolare l'albero di copertura minimo (minimum spanning tree o MST) di un un grafo non orientato.

Per collegare n nodi dobbiamo utilizzare n - 1 rami.

1. Ordina tutti i rami in ordine di peso crescente. 
2. Prendi il ramo con peso minore. Controlla se forma un ciclo nel MST che hai formato fino a quel momento. Se il ciclo non si forma includi quel ramo altrimenti passa al successivo.
3. Ripeti il passo 2 fino a quando non ci sono n - 1 rami nel MST

Si può dimostrare che utilizzando questo algoritmo si può trovare la soluzione ottima.

![Kruskal, grafo iniziale](/informaticainsieme/images/algoritmi/kruskal/kruskal01.png){:class="aside-image"}

![Kruskal, primo ramo](/informaticainsieme/images/algoritmi/kruskal/kruskal02.png){:class="aside-image"}

![Kruskal, secondo ramo](/informaticainsieme/images/algoritmi/kruskal/kruskal03.png){:class="aside-image"}

![Kruskal, terzo ramo](/informaticainsieme/images/algoritmi/kruskal/kruskal04.png){:class="aside-image"}

![Kruskal, quarto ramo](/informaticainsieme/images/algoritmi/kruskal/kruskal05.png){:class="aside-image"}

---
title: L'algoritmo di Prim
date: '2021-09-30T10:58:13+02:00'
author: Fabio Mattei
layout: page
---

E' un algoritmo molto simile a quello di Dijkstra, la differenza nella meccanica è che al posto del costo complessivo si tiene conto del costo passo per passo. 

L'algoritmo di Prim serve a calcolare l'albero coprente minimo per un grafo pesato non orientato.

L'algoritmo segue queste regole:

* Ogni volta che vogliamo visitare un nuovo nodo si sceglie quello collegato alla minima distanza tra i nodi da visitare.
* Una volta scelto il nodo controlliamo tutti i nodi suoi vicini e la confrontiamo che le distanze minime che abbiamo ottenuto fino ad ora per arrivare a quei nodi.
* Se la distanza appena calcolata per arrivare ad un nodo è minore della distanza conosciuta in precedenza, aggiorneremo la distanza più corta.

Riprendiamo in esame il grafo precedente e immaginiamo di partire sempre dal nodo A.


| Nodo | Distanza minima | Predecessore | Visitato |
| ---- | --------------- | ------------ | -------- |
| A    | 0               | no           | Si       |
| B    | infinito        |              |          |
| C    | infinito        |              |          |
| D    | infinito        |              |          |
| E    | infinito        |              |          |

![Prim, grafo iniziale](prim01.png)

Consideriamo tutti i suoi vicini e calcoliamo la distanza da A.

| Nodo | Distanza minima | Predecessore | Visitato |
| ---- | --------------- | ------------ | -------- |
| A    | 0               | no           | Si       |
| B    | 6               |              |          |
| C    | infinito        |              |          |
| D    | infinito        |              |          |
| E    | 3               |              |          |

Scegliamo il ramo con distanza minore e passiamo al passo successivo. Dato che il ramo pù corto è quello che collega E.

![Prim, grafo iniziale](prim02.png)

A questo punto 



## Algoritmo di Kruskal

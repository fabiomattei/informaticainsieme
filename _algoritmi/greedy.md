---
title: L'approccio Greedy
date: '2021-09-30T10:58:13+02:00'
author: Fabio Mattei
layout: page
---

Gli algoritmi di tipo Greedy sono una classe di algoritmi che hanno una logica di fondo comune. Gli algoritmo di tipo Greedy mirano a trovare una soluzione ottima globale cercando di fare la scelta migliore ad ogni passo.
Quando si presenta una scelta l'algoritmo sceglie la strada che permette di avere il massimo vantaggio senza cercare di intepretare il futuro e senza rimettere in discussione
le scelte fatte in passato.

Sono importanti perché anche se non portano necessariamente ad una soluzione ottima portano a trovare una soluzione buona facendo un efficiente uso del tempo.

Si riconosce un algoritmo Greed da questo tipo di ragionamento:

__In questo momento esatto qual'è la scelta migliore da fare?__

Gli algoritmi Greedy sono voraci, a loro non interessa la visione globale, vogliono ottenere il massimo vantaggio ad ogni passo, sono interessati solo dalla soluzione ottima locale. Questo significa che una soluzione ottima globale potrebbe basarsi su scelte di tipo diverso.

Gli algoritmi Greedy non si guardano mai indietro e non si mettono mai in discussione.

Gli algoritmi Greedy fanno un uso del tempo efficiente, generalmente trovano una buona soluzione in un tempo accettabile.
Qualche volta accade che l'algoritmo Greedy trovi la soluzione ottima.

## Il problema di Knapsack


## Algoritmo di Prim

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
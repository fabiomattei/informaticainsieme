---
title: L'algoritmo di Prim
date: '2021-09-30T10:58:13+02:00'
author: Fabio Mattei
layout: page
---

L'algoritmmo di Prim è un algoritmo molto simile a quello di Dijkstra. A differenza di questo l'algoritmo di Prim vuole calcolare l'albero di copertura minimo (minimum spanning tree o MST) di un un grafo non orientato e con pesi non negativi.

La differenza nella meccanica è che al posto del costo complessivo tra ciascun nodo e il nodo radice si tiene conto del costo tra ciascun nodo e il più viciono nodo già coperto.

L'algoritmo segue queste regole:

* Ogni volta che vogliamo visitare un nuovo nodo si sceglie quello collegato alla minima distanza tra i nodi da visitare.
* Una volta scelto il nodo controlliamo tutti i nodi suoi vicini e la confrontiamo che le distanze minime che abbiamo ottenuto fino ad ora per arrivare a quei nodi.
* Se la distanza appena calcolata per arrivare ad un nodo è minore della distanza conosciuta in precedenza, aggiorneremo la distanza più corta.

Immaginiamo di partire sempre dal nodo A

| Nodo | Distanza minima | Predecessore | Visitato |
| ---- | --------------- | ------------ | -------- |
| A    | 0               | no           | Si       |
| B    | infinito        |              |          |
| C    | infinito        |              |          |
| D    | infinito        |              |          |
| E    | infinito        |              |          |

![Prim, grafo iniziale](/informaticainsieme/images/algoritmi/prim/prim01.png)

Consideriamo tutti i suoi vicini e calcoliamo la distanza da A.

| Nodo | Distanza minima | Predecessore | Visitato |
| ---- | --------------- | ------------ | -------- |
| A    | 0               | no           | Si       |
| B    | 6               |              |          |
| C    | infinito        |              |          |
| D    | infinito        |              |          |
| E    | 3               | A            | Si       |

Scegliamo il ramo con distanza minore che collega i nodi non ancora coperti dal nodo iniziale, questa è rappresentata dalla distanza tra A ed E, quindi scegliamo E, lo marchiamo come visitato nella tabella e passiamo al passo successivo.

![Prim, grafo iniziale](/informaticainsieme/images/algoritmi/prim/prim02.png)

A questo punto abbiamo visitato i nodi A ed E, calcoliamo le distanza che separano i nodi non ancora coperti da quelli coperti e aggiorniamo la tabella. Notiamo che B è a distanza 2 da E, il nodo C è a distanza 4 da E e il nodo B è a distanza 2 da E. Aggiorno la tabella di conseguenza. Ora bisogna aggiungere un nuovo nodo, il nodo non ancora visitato a distanza minima dal MST è B con distanza 2 da E.

| Nodo | Distanza minima | Predecessore | Visitato |
| ---- | --------------- | ------------ | -------- |
| A    | 0               | no           | Si       |
| B    | 2               | E            | Si       |
| C    | 4               |              |          |
| D    | 7               |              |          |
| E    | 3               | A            | Si       |

![Prim, grafo iniziale](/informaticainsieme/images/algoritmi/prim/prim03.png)

A questo punto sono nel MST i nodi A, B, E quindi vado a riaggiornare la distanze tra nodi non ancora visitati e questi nodi. Rivaluto il nodo C che è adiacente a B con distanza 6 ma C era già valutato a distanza 4 da E, quindi non aggiorno le distanza minime sulla tabella.
Scegliamo il ramo con distanza minore che collega i nodi non ancora coperti dal nodo iniziale e questo è rappresentato da C-E, quindi aggiorno la tabella.

| Nodo | Distanza minima | Predecessore | Visitato |
| ---- | --------------- | ------------ | -------- |
| A    | 0               | no           | Si       |
| B    | 2               | E            | Si       |
| C    | 4               | E            | Si       |
| D    | 7               |              |          |
| E    | 3               | A            | Si       |

![Prim, grafo iniziale](/informaticainsieme/images/algoritmi/prim/prim04.png)

A questo punto sono nel MST i nodi A, B, E e C quindi vado a riaggiornare la distanze tra nodi non ancora visitati e questi nodi.

Rivaluto il nodo D che è adiacente a C con distanza 3. In precedenza questo era valutato a distanza 7 quindi vado ad aggiornare la tabella.
Scegliamo il ramo con distanza minore che collega i nodi non ancora coperti dal nodo iniziale e questo è rappresentato da C-E, quindi aggiorno la tabella.

| Nodo | Distanza minima | Predecessore | Visitato |
| ---- | --------------- | ------------ | -------- |
| A    | 0               | no           | Si       |
| B    | 2               | E            | Si       |
| C    | 4               | E            | Si       |
| D    | 3               | C            | Si       |
| E    | 3               | A            | Si       |


![Prim, grafo iniziale](/informaticainsieme/images/algoritmi/prim/prim05.png)

L'albero di copertura minimo (minimum spanning tree o MST) è. ili seguente.

![Prim, grafo iniziale](/informaticainsieme/images/algoritmi/prim/prim06.png)




---
title: L'algoritmo di Prim
date: '2021-09-30T10:58:13+02:00'
author: Fabio Mattei
layout: page
---

L'algoritmo di Prim è un algoritmo molto simile a quello di Dijkstra. A differenza di questo l'algoritmo di Prim vuole calcolare l'albero di copertura minimo (minimum spanning tree o MST) di un un grafo non orientato e con pesi non negativi.

La differenza nella meccanica è che al posto del costo complessivo tra ciascun nodo e il nodo radice si tiene conto del costo tra ciascun nodo e il più viciono nodo già coperto.

L'algoritmo segue queste regole:

* Ogni volta che vogliamo visitare un nuovo nodo si sceglie quello collegato alla minima distanza tra i nodi da visitare.
* Una volta scelto il nodo controlliamo tutti i nodi suoi vicini e la confrontiamo che le distanze minime che abbiamo ottenuto fino ad ora per arrivare a quei nodi.
* Se la distanza appena calcolata per arrivare ad un nodo è minore della distanza conosciuta in precedenza, aggiorneremo la distanza più corta.

![Prim, grafo iniziale](/images/algoritmi/prim/prim01.png){:class="aside-image"}

Immaginiamo di partire dal nodo A, e inizializziamo la tabella che rappresenta le distanze minime dicopertura del grafo. Porremo A a distannza 0 e tutti gli altri nodi a distanza infinita. A è l'unico nodo visitato.

| Nodo | Distanza minima | Predecessore | Visitato |
| ---- | --------------- | ------------ | -------- |
| A    | 0               | no           | Si       |
| B    | infinito        |              |          |
| C    | infinito        |              |          |
| D    | infinito        |              |          |
| E    | infinito        |              |          |

![Prim, grafo iniziale](/images/algoritmi/prim/prim02.png){:class="aside-image"}

A questo punto è nel MST soltanto il nodo A. Vado a valutare la distanze tra nodi non ancora visitati adiacenti ad A e questi nodi sono B a distanza 6 da A ed E a distanza 3 da A.

Scegliamo il ramo con distanza minore che collega i nodi non ancora coperti dal nodo iniziale, questa è rappresentata dalla distanza tra A ed E, quindi scegliamo E, lo marchiamo come visitato nella tabella e passiamo al passo successivo.

| Nodo | Distanza minima | Predecessore | Visitato |
| ---- | --------------- | ------------ | -------- |
| A    | 0               | no           | Si       |
| B    | 6               |              |          |
| C    | infinito        |              |          |
| D    | infinito        |              |          |
| E    | 3               | A            | Si       |

![Prim, grafo iniziale](/images/algoritmi/prim/prim03.png){:class="aside-image"}

A questo punto abbiamo visitato i nodi A ed E, calcoliamo le distanza che separano i nodi non ancora coperti dal MST quelli coperti e aggiorniamo la tabella. Notiamo che B è a distanza 2 da E, il nodo C è a distanza 4 da E e il nodo B è a distanza 2 da E. Aggiorno la tabella di conseguenza. 

Scegliamo il ramo con distanza minore che collega i nodi non ancora coperti dal nodo iniziale e questo è rappresentato da B-E, quindi aggiorno la tabella.

| Nodo | Distanza minima | Predecessore | Visitato |
| ---- | --------------- | ------------ | -------- |
| A    | 0               | no           | Si       |
| B    | 2               | E            | Si       |
| C    | 4               |              |          |
| D    | 7               |              |          |
| E    | 3               | A            | Si       |

![Prim, grafo iniziale](/images/algoritmi/prim/prim04.png){:class="aside-image"}

A questo punto sono nel MST i nodi A, B, E quindi vado a riaggiornare la distanze tra nodi non ancora visitati e questi nodi. Rivaluto il nodo C che è adiacente a B con distanza 6 ma C era già valutato a distanza 4 da E, quindi non aggiorno le distanza minime sulla tabella.

Scegliamo il ramo con distanza minore che collega i nodi non ancora coperti dal nodo iniziale e questo è rappresentato da C-E, quindi aggiorno la tabella.

| Nodo | Distanza minima | Predecessore | Visitato |
| ---- | --------------- | ------------ | -------- |
| A    | 0               | no           | Si       |
| B    | 2               | E            | Si       |
| C    | 4               | E            | Si       |
| D    | 7               |              |          |
| E    | 3               | A            | Si       |

![Prim, grafo iniziale](/images/algoritmi/prim/prim05.png){:class="aside-image"}

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

L'albero di copertura minimo (minimum spanning tree o MST) è. ili seguente.

![Prim, grafo iniziale](/images/algoritmi/prim/prim06.png)

## Implementazione in python

{% highlight python %}  
grafo = [
    ('E', 'B', 2), 
    ('A', 'E', 3), 
    ('D', 'C', 3),
    ('B', 'D', 4), 
    ('E', 'C', 4), 
    ('B', 'C', 6), 
    ('A', 'B', 6), 
    ('E', 'D', 7)
]

nnodi = 5
inf = float('inf')

percorsi_minimi = { 
    'A': [  0, '', False, ''], 
    'B': [inf, '', False, ''], 
    'C': [inf, '', False, ''], 
    'D': [inf, '', False, ''], 
    'E': [inf, '', False, ''] 
}

nodi_visitati = []

while len(nodi_visitati) < nnodi:
    # inserisco il nodo a distanza minima
    nodo_da_inserire = ''
    min_dist = float('inf')
    for nodo in percorsi_minimi:
        if (percorsi_minimi[nodo][0] < min_dist) and (nodo not in nodi_visitati):
            nodo_da_inserire = nodo
            min_dist = percorsi_minimi[nodo][0]
            
    nodi_visitati.append( nodo_da_inserire )
    percorsi_minimi[nodo_da_inserire][2] = True
            
    # rivaluto le distanze
    indice = 0
    while indice < len(grafo):
        n1 = grafo[indice][0]
        n2 = grafo[indice][1]
        if (n1 == nodo_da_inserire):
            if grafo[indice][2] < percorsi_minimi[n2][0]:
                percorsi_minimi[n2][0] = grafo[indice][2]
                percorsi_minimi[n2][3] = nodo_da_inserire
        
        if (n2 == nodo_da_inserire):
            if grafo[indice][2] < percorsi_minimi[n1][0]:
                percorsi_minimi[n1][0] = grafo[indice][2]
                percorsi_minimi[n2][3] = nodo_da_inserire
                
        indice = indice + 1
    

print(percorsi_minimi)
{% endhighlight %}


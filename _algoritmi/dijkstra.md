---
title: Algoritmo di Dijkstra
date: '2021-09-30T10:58:13+02:00'
author: Fabio Mattei
layout: page
---

L'algoritmo di Dijkstra trova il cammino più breve che collega un nodo radice ad ogni altro nodo del grafo. Nel nostro esempio utilizzereemo un grafo orientato pesato, ogni arco ha una direzione ed un peso.

L'agoritmo di Dijkstra ha molti utilizzi. Può essere utilizzato per calcolare il percorso su di una mappa. Inoltre è utilizzato per:

* IP Routing
* A* Algorithm
* Network telefonici

L'algoritmo segue queste regole:

* Ogni volta che vogliamo visitare un nuovo nodo si sceglie quello collegato alla minima distanza tra i nodi da visitare.
* Una volta scelto il nodo controlliamo tutti i nodi suoi vicini e calcoliamo la distanza tra i vicini e la radice sommando i pesi dai percorsi che includono il nuovo nodo selezionato.
* Se la distanza appena calcolata è minore della distanza conosciuta in precedenza, aggiorneremo la distanza più corta.


Immaginiamo di trovarci di fronte il seguente grafo:

![Dijkstra, grafo iniziale](/images/algoritmi/dijkstra/dijkstra01.png){:class="aside-image"}

Per lavorare con l'algoritmo si crea una tabella che indica la distanza complessiva tra la radice e ciascun nodo e si pone il suo valore ad infinito per ciascun nodo. Il nodo di partenza è il nodo A che pertanto si considera a distanza 0.

| Nodo | Distanza minima | Predecessore | Visitato |
| ---- | --------------- | ------------ | -------- |
| A    | 0               | no           | Si       |
| B    | infinito        |              |          |
| C    | infinito        |              |          |
| D    | infinito        |              |          |
| E    | infinito        |              |          |

![Dijkstra: aggiungo nodo A](/images/algoritmi/dijkstra/dijkstra02.png){:class="aside-image"}

Iniziamo ad applicare l'algoritmo di Dijkstra a partire dal nodo A che considereremo **nodo radice**. **Consideriamo tutti i suoi vicini** e calcoliamo la distanza da A. Il fine ultimo dell'algoritmo è quello di calcolare la distanza minima che intercorre tra A e tutti i nodi del grafo.
I nodi raggiungibili da A sono B ed E. B ha distanza 6 da A ed E ha distanza 3 da A. Queste distanze rappresentano la distanza minima tra i nodi B, E ed A.

La tabella delle distanza aggiornata è la seguente:

| Nodo | Distanza minima | Predecessore | Visitato |
| ---- | --------------- | ------------ | -------- |
| A    | 0               | no           | Si       |
| B    | 6               | A            |          |
| C    | infinito        |              |          |
| D    | infinito        |              |          |
| E    | 3               | A            |          |

A questo punto selezioniamo un nodo da aggiungere ai nodi visitati. Dato che Il nodo a costo minimo da raggiungere è il nodo E vado a ad aggiungere E ai nodi visitati.

![Dijkstra: aggiungo nodo E](/images/algoritmi/dijkstra/dijkstra03.png){:class="aside-image"}

Una volta in E rivaluto le distanze dei vicini di E non ancora visitati. 
Noto che D, è raggiungibile a costo 7 da E, quindi è raggiungibile a costo complessivo 10 da A.
Noto che C, è raggiungibile a costo 4 da E, quindi è raggiungibile a costo complessivo 7 da A.
Dato che B è raggiungiibile da C rivaluto il costo di B. Il costo inizale era 6, ma siccome B dista 2 da E posso raggiungerlo a costo 5 passando per E.

Ecco la tabella aggiornata.

| Nodo | Distanza minima | Predecessore | Visitato |
| ---- | --------------- | ------------ | -------- |
| A    | 0               | no           | Si       |
| B    | 5               | E            |          |
| C    | 7               | E            |          |
| D    | 10              | E            |          |
| E    | 3               | A            | Si       |

Dopo aver rivalutato tutte le distanze vado a selezionare il nodo, **non ancora visitato**, che ha distanza minima da A. Scelgo B che è a distanza 5 da A.

![Dijkstra: aggiungo nodo C](/images/algoritmi/dijkstra/dijkstra04.png){:class="aside-image"}

Una volta aggiunto il nodo B rivaluto le distanze dei vicini di C non ancora visitati.

Noto che da B è raggiungibile D a costo 4. Dato che arrivare a C ha costo 5, il costo complessivo per andare da A a D, secondo questo nuovo percroso, è pari a 7. Al momento il costo per arrivare a D è 10 quindi il percorso appena trovato è più conveniente. Vado ad aggiornare allora la tabella.

Da B è raggiungibile C a costo 6. Dato che arrivare a C ha costo 5, il costo complessivo per arrivare da A a C secondo questo nuovo percorso sarebbe 11. Tuttavia è già possibile arrivare a B con un costo minore pari a 7 quindi scarto questa possibilità.


| Nodo | Distanza minima | Predecessore | Visitato |
| ---- | --------------- | ------------ | -------- |
| A    | 0               | no           | Si       |
| B    | 5               | E            | Si       |
| C    | 7               | E            |          |
| D    | 10              | C            |          |
| E    | 3               | A            | Si       |

Per andare al passo successivo guardo tutti i nodi rimasti e scelgo quello a costo minore. Sono rimasti C e D e il costo minore è rappresentato da C pari a 7.

![Dijkstra: aggiungo nodo B](/images/algoritmi/dijkstra/dijkstra05.png){:class="aside-image"}

Una volta in C rivaluto le distanze dei suoi vicini non visitati.
L'unico rimasto è il nodo D che è distante 3 da D. Dato che arrivare a C mi è costato 7 il costo complessivo per arrivare a D passando per B sarebbe 10. Ma questo non migliora il 10 già calcolato in precedenza, quindi non ricevo benefici e scarto questa ipotesi.

| Nodo | Distanza minima | Predecessore | Visitato |
| ---- | --------------- | ------------ | -------- |
| A    | 0               | no           | Si       |
| B    | 5               | E            | Si       |
| C    | 7               | E            | Si       |
| D    | 10              | C            |          |
| E    | 3               | A            | Si       |

![Dijkstra: aggiungo nodo D](/images/algoritmi/dijkstra/dijkstra06.png){:class="aside-image"}

A questo punto rimane solo il nodo D e lo aggiungo senza il bisogno di rivalutare nulla.

Ecco dunque la tabella indicante le distanze minime a partire dal nodo A.

| Nodo | Distanza minima | Predecessore | Visitato |
| ---- | --------------- | ------------ | -------- |
| A    | 0               | no           | Si       |
| B    | 5               | E            | Si       |
| C    | 7               | E            | Si       |
| D    | 10              | C            | Si       |
| E    | 3               | A            | Si       |

L'algoritmo di Dijkstra è stato completato.

![Dijkstra: aggiungo nodo D](/images/algoritmi/dijkstra/dijkstra07.png)


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
            if min_dist + grafo[indice][2] < percorsi_minimi[n2][0]:
                percorsi_minimi[n2][0] = min_dist + grafo[indice][2]
                percorsi_minimi[n2][3] = nodo_da_inserire
        
        if (n2 == nodo_da_inserire):
            if min_dist + grafo[indice][2] < percorsi_minimi[n1][0]:
                percorsi_minimi[n1][0] = min_dist + grafo[indice][2]
                percorsi_minimi[n2][3] = nodo_da_inserire
                
        indice = indice + 1
    

print(percorsi_minimi)
{% endhighlight %}

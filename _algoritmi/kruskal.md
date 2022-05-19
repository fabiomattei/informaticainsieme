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

![Kruskal, grafo iniziale](/images/algoritmi/kruskal/kruskal01.png){:class="aside-image"}

![Kruskal, primo ramo](/images/algoritmi/kruskal/kruskal02.png){:class="aside-image"}

![Kruskal, secondo ramo](/images/algoritmi/kruskal/kruskal03.png){:class="aside-image"}

![Kruskal, terzo ramo](/images/algoritmi/kruskal/kruskal04.png){:class="aside-image"}

![Kruskal, quarto ramo](/images/algoritmi/kruskal/kruskal05.png){:class="aside-image"}

## Implementazione in python

{% highlight python %}
grafo = [('E', 'B', 2), ('A', 'E', 3), ('D', 'C', 3),('B', 'D', 4), ('E', 'C', 4), ('B', 'C', 6), ('A', 'B', 6), ('E', 'D', 7)]
nnodi = 5

cont_archi = 0
kruskal = []
nodi = []

while cont_archi < nnodi - 1:
    aggiunto = False
    indice = 0
    while indice < len(grafo) and not aggiunto:
        #print("considero", grafo[indice])
        n1 = grafo[indice][0]
        n2 = grafo[indice][1]
        if (n1 in nodi and n2 not in nodi) or (n1 not in nodi and n2 in nodi) or len(kruskal) == 0:
            kruskal.append( grafo[indice] )
            if n1 not in nodi:
                nodi.append( n1 )
            if n2 not in nodi:
                nodi.append( n2 )
            cont_archi = cont_archi + 1
            aggiunto = True
            print("********* PASSO ***********")
            print("aggiungo", grafo[indice])
            grafo.remove( grafo[indice] )
            print("kruskal", kruskal)
            print("grafo", grafo)
            print("nodi", nodi)
            print("cont_archi", cont_archi)
        indice = indice + 1


print("MST:", kruskal)
{% endhighlight %}
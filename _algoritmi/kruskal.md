---
title: L'algoritmo di Kruskal
date: '2021-09-30T10:58:13+02:00'
author: Fabio Mattei
layout: page
---

L'algoritmo di Kruskal è un algoritmo che consente di calcolare l'albero di copertura minimo (minimum spanning tree o MST) di un un grafo non orientato.

Un grafo comprende per sua natura molti archi, il minimum spanning tree formato da un sottoinsieme di questi.
Il minimum spanning tree **non ha mai cicli al suo interno** e, se nel grafo sono presenti **n nodi**, per collegare n nodi utilizzando il minimo numero di archi **abbiamo la necessità di utilizzare n - 1 archi**.

Questi i passi dell'algoritmo:

1. Ordina tutti i rami in ordine di peso crescente. 
2. Prendi l'arco con peso minore. Controlla se forma un ciclo nel MST che hai formato fino a quel momento. Se il ciclo non si forma includi quel ramo nel MST altrimenti passa all'arco successivo.
3. Ripeti il passo 2 fino a quando non ci sono n - 1 archi nel MST

Si può dimostrare che utilizzando questo algoritmo si può trovare la soluzione ottima.

{::options parse_block_html="true" /}

![Kruskal, grafo iniziale](/images/algoritmi/kruskal/kruskal01.png){:class="aside-image"}

Inizialmente rappresentiamo il nostro grafo con tutti i nodi e tutti gli archi. 
Al contempo costruiremo una struttura dati che contiene tutti gli archi in ordine di lunghezza.

<br style="clear:both" />

![Kruskal, primo ramo](/images/algoritmi/kruskal/kruskal02.png){:class="aside-image"}

Il primo passo consiste nello scegliere il ramo più corto sul nostro grafo ed includerlo nel nostro MST.
D'ora in avanti rappresenteremo in rosso i rami che fanno parte del MST.

	MST: E-B

<br style="clear:both" />

![Kruskal, secondo ramo](/images/algoritmi/kruskal/kruskal03.png){:class="aside-image"}

Il secondo passo consiste nel prendere in esame tutti i rami collegati ai nodi connessi al MST, in questo caso E e B.
Tra questi consideriamo il ramo più corto, e se questo non forma un ciclo chiuso lo selezioniamo, altrimenti passiamo al nodo successivo.
In questo caso il ramo più corto è E-A. Dato che questo non forma un ciclo chiuso possiamo includerlo nel MST.

	MST: E-B, E-A

<br style="clear:both" />

![Kruskal, terzo ramo](/images/algoritmi/kruskal/kruskal04.png){:class="aside-image"}

Il passo successivo consiste di nuovo nel prendere in esame tutti i rami collegati ai nodi connessi al MST, in questo caso E, B e A.
Tra questi consideriamo il ramo più corto. Dato che ci sono due rami a lunghezza 4 consideriamo quello ad indice minimo, dunque quello collegato al nodo B. Questo non forma un ciclo chiuso quindi lo selezioniamo e lo includiamo nell'MST.

	MST: E-B, E-A, B-D

<br style="clear:both" />

![Kruskal, quarto ramo](/images/algoritmi/kruskal/kruskal05.png){:class="aside-image"}

L'ultmo passo consiste di nuovo nel prendere in esame tutti i rami collegati ai nodi connessi al MST, in ordine di lunghezza che non formano cicli. Noteremo che soltanto B-C e D-C non formano cicli. Il più corto tra questi è D-C quindi lo includiamo nel MST.

	MST: E-B, E-A, B-D, D-C

<br style="clear:both" />

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

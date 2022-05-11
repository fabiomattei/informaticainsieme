---
title: L'approccio greedy al problema di knapsak
date: '2021-09-30T10:58:13+02:00'
author: Fabio Mattei
layout: page
---

## L'approccio greedy al problema di Knapsack

E' possibile approcciare il problema di knapsack con approccio greedy. Questo vuol dire trovare una buona soluzione ammissibile, non necessariamente ottima, attraverso un numero di passi limitato.

Iniziamo dal creare una struttura dati che mi permetta di rappresentare una istanza del problema di knapsack. Utilizzeremo una variabile di nome **capienza** per memorizzare la capienza massima del nostro contenitore e una lista di dizionari per rappresentare i materiali.

{% highlight python %}
cap = 5    # capienza
oggetti = [ 
	{ 'n': 'm1', 'p': 1, 'v': 2 }, 
	{ 'n': 'm2', 'p': 3, 'v': 4 }, 
	{ 'n': 'm3', 'p': 2, 'v': 3 }, 
	{ 'n': 'm4', 'p': 3, 'v': 5 }, 
	{ 'n': 'm5', 'p': 2, 'v': 4 } 
]
{% endhighlight %}


Passo 1: Metto gli oggetti in ordine V/P e prendo gli oggetti che posso fino a soddisfare il vincolo ci capacità.


Aggiungo per ciascuno oggetto l'indice r come rapporto V/P

{% highlight python %}            
for oggetto in oggetti:
    oggetto['r'] = oggetto['v']/oggetto['p']
{% endhighlight %}   

Ordino la lista di oggetti 

{% highlight python %}     
ordinata = sorted(oggetti, key= lambda oggetto: oggetto['r'], reverse=True)
{% endhighlight %}

Prima fase di riempiment

{% highlight python %}    
pesotot = 0
valoretot = 0
indice = 0
selezionati = []
while pesotot + ordinata[indice]['p'] <= cap:
    pesotot = pesotot + ordinata[indice]['p']
    valoretot = valoretot + ordinata[indice]['v']
    selezionati.append(ordinata[indice]) # aggiungo l'elemento alla lista selezionati
    print(ordinata[indice], "peso parz", pesotot, "valore par", valoretot)
    indice = indice + 1

print("peso tot carciato:", pesotot, "valore tot caricato:", valoretot)
print("Ho ancora capacità residua:", cap - pesotot )
{% endhighlight %}

Riempimento degli sfridi

{% highlight python %}  
print("Riempimento degli sfridi")
while indice < len(ordinata):
    if pesotot + ordinata[indice]['p'] <= cap:
        pesotot = pesotot + ordinata[indice]['p']
        valoretot = valoretot + ordinata[indice]['v']
        selezionati.append(ordinata[indice]) # aggiungo l'elemento alla lista selezionati
        print(ordinata[indice], "peso parz", pesotot, "valore par", valoretot)
    indice = indice + 1

print("Lista materiali selezionati")
print(selezionati)
{% endhighlight %}



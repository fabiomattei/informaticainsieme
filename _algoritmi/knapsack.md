---
title: L'approccio greedy al problema di knapsak
date: '2021-09-30T10:58:13+02:00'
author: Fabio Mattei
layout: page
---

## L'approccio greedy al problema di Knapsack

E' possibile approcciare il problema di knapsack con approccio greedy. Questo vuol dire trovare una buona soluzione ammissibile, non necessariamente ottima, attraverso un numero di passi limitato.

Iniziamo dal creare una struttura dati che mi permetta di rappresentare una istanza del problema di knapsack. Utilizzeremo una variabile di nome **capienza** per memorizzare la capienza massima del nostro contenitore e una lista di dizionari per rappresentare i materiali.

Notiamo che per ciascun dizionario sono indicati gli indici n, p, v.

* n: nome o sigla dell'oggetto
* p: peso dell'oggetto
* v: valore dell'oggetto

## Organizziamo i dati

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

L'idea consiste nel mettere gli oggetti in ordine secondo il rapporto V/P decrescente, quindi si procede a selezionare gli oggetti iniziando da quello con rapporto V/P più grande, a scorrere verso quelli con rapporto V/P minore fino a soddisfare il vincolo ci capacità.

Procediamo aggiungendo per ciascun oggetto il rapporto e memorizzandolo all'indice r.

{% highlight python %}            
for oggetto in oggetti:
    oggetto['r'] = oggetto['v']/oggetto['p']
{% endhighlight %}   

Ordiniano la lista secondo il rapporto V/P decrescente

{% highlight python %}     
lista_ordinata = sorted(oggetti, key= lambda oggetto: oggetto['r'], reverse=True)
{% endhighlight %}

## Procediamo con il riempimento

Utilizziamo le variabili accumulatore **pesotot** e **valoretot** per memorizzare rispettivamente il peso totale degli oggetti selezionati e il valore totale degli oggetti selezionati.

Fintantoché c'è capienza, aggiungiamo gli elementi alla lista **selezionati**.

{% highlight python %}    
pesotot = 0      # variabile accumulatore che tiene memoria del peso totale degli oggetti selezionati
valoretot = 0    # variabile accumulatore che tiene memoria del valore totale degli oggetti selezionati    
indice = 0
selezionati = [] # lista che conterrà gli oggetti selezionati
while pesotot + lista_ordinata[indice]['p'] <= cap:
    pesotot = pesotot + lista_ordinata[indice]['p']
    valoretot = valoretot + lista_ordinata[indice]['v']
    selezionati.append(lista_ordinata[indice]) # aggiungo l'elemento alla lista selezionati
    print(lista_ordinata[indice], "peso parz", pesotot, "valore par", valoretot)
    indice = indice + 1

print("peso tot carciato:", pesotot, "valore tot caricato:", valoretot)
print("Ho ancora capacità residua:", cap - pesotot )
{% endhighlight %}

## Riempimento degli sfridi

Potrebbe essere che alla fine del processo c'è ancora dello spazio residuo per inserire qualche elemento in più. Allora aggiungo tutto quello che posso.

{% highlight python %}  
print("Riempimento degli sfridi")
while indice < len(lista_ordinata):
    if pesotot + lista_ordinata[indice]['p'] <= cap:
        pesotot = pesotot + lista_ordinata[indice]['p']
        valoretot = valoretot + lista_ordinata[indice]['v']
        selezionati.append(lista_ordinata[indice]) # aggiungo l'elemento alla lista selezionati
        print(lista_ordinata[indice], "peso parz", pesotot, "valore par", valoretot)
    indice = indice + 1

print("Lista materiali selezionati")
print(selezionati)
{% endhighlight %}



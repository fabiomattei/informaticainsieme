---
title: 'Doppio campione'
date: '2023-04-18'
author: Fabio Mattei
layout: page
---

In una lista di numeri interi S devo trovare due coppie di numeri più grandi.

## Algoritmo del campione

Iniziando dal semplice problema di trovare il numero più grande all'interno di una lista.

Inizializziamo una variabile **campA** dove andremo a memorizzare il campione. Attraverso un ciclo while visitiamo la lista e confrontiamo ad ogni iterazione il valore dell'elementi i-esimo della lista con il campione. Se questo è maggiore diventerà il nuovo campione.

{% highlight python %}
S = [4, 5, 7, 3, 4, 9, 3, 5, 7, 6, 4, 2, 9]

campA = 0

i = 0
while i < len(S):
    if S[i] > campA:  # ho un campione possibile
        campA = S[i]
    i=i+1

print(campA)
{% endhighlight %}

#### Output:

{% highlight python %}
9
{% endhighlight %}


## Algoritmo della più grande coppia campione

Ora troviamo all'interno della lista il numero più grande che abbia almeno due occorrenze nella lista. 

Se guardiamo il codice seguente ci accorgiamo che il numero più grande presente nella lista è il numero 11 ma siccome questo ha soltanto una occorrenza non rispetta i nostri requisiti. Il numero 9 d'altro canto ha due occorrenze, quindi il 9 e il numero che vogliamo il nostro algoritmo sia in grado di trovare.

{% highlight python %}
S = [4, 5, 7, 3, 4, 9, 3, 5, 7, 6, 4, 2, 9, 11]

campA = 0
camp_cont = 0

i = 0
while i < len(S):
    if S[i] > campA:  # ho un possibile campione 
        j=i+1
        while j < len(S): # controllo se c'è un altro numero uguale al possibile campione nel resto della lista
            if S[j]==S[i]: # il numero esiste
                campA = S[i]
                camp_cont= camp_cont+1
            j = j + 1
    i=i+1

print("numero campioni",camp_cont)             
print(campA)
{% endhighlight %}

Vediamo che quando troviamo un possibile campione si innesca un secondo ciclo che cerca di trovare una seconda occorrenza del numero nella lista.

#### Output:

{% highlight python %}
numero campioni 5
9
{% endhighlight %}

## Algoritmo delle due più grandi coppie campione

Il nostro problema ci chiede di trovare le due più grando coppie di numeri che siano più grandi quindi facciamo una piccola modifica all'algoritmo precedente, quando troviamo un nuovo campione, automaticamente il vecchio campione diventa il secondo campione.s

{% highlight python %}
S = [4, 5, 7, 3, 4, 9, 3, 5, 7, 6, 4, 2, 9]

campA = 0
campB = 0
camp_cont = 0

i = 0
while i < len(S):
     if S[i] > campA:  # ho un campione possibile
         j=i+1
         while j < len(S): # controllo se c'è un altro numero uguale al campione possibile nel resto della lista
             if S[j]==S[i]: # il numero esiste
                 campB = campA  # il vecchio campione diventa campB
                 campA = S[i]
                 camp_cont= camp_cont+1
             j = j + 1
     i=i+1

print("numero campioni", camp_cont)             
print(campA)
print(campB)
{% endhighlight %}

#### Output:

{% highlight python %}
numero campioni 5
9
7
{% endhighlight %}

## Problemino: cosa succede se il più grande campione anticipa il più piccolo campione?

Purtroppo se mi trovo in questa situazione l'algoritmo fallisce e non trova la seconda coppia campione.

{% highlight python %}
S = [9, 9, 5, 7, 3, 4, 3, 5, 7, 6, 4, 2]

campA = 0
campB = 0
camp_cont = 0

i = 0
while i < len(S):
     if S[i] > campA:  # ho un campione possibile
         j=i+1
         while j < len(S): # controllo se c'è un altro numero uguale al campione possibile nel resto della lista
             if S[j]==S[i]: # il numero esiste
                 campB = campA  #il vecchio campione diventa campB
                 campA = S[i]
                 camp_cont = camp_cont+1
             j = j + 1
     i=i+1

print("numero campioni", camp_cont)             
print(campA)
print(campB)
{% endhighlight %}

#### Output:

{% highlight python %}
numero campioni 1
9
0
{% endhighlight %}

## La soluzione

Per ovviare al problema ripropongo in seconda battuta l'algoritmo scritto in precedenza, il quale viene applicato se s[i] > campB e se al contempo S[i] != campA. 

{% highlight python %}
S = [9, 9, 5, 7, 3, 4, 3, 5, 7, 6, 4, 2]

campA = 0
campB = 0
camp_cont = 0

i = 0
while i < len(S):
     if S[i] > campA:  # ho un possibile campione 
         j=i+1
         while j < len(S): # controllo se c'è un altro numero uguale al possibile campione nel resto della lista
             if S[j]==S[i]: # il numero esiste
                 campB = campA  #il vecchio campione diventa campB
                 campA = S[i]
                 camp_cont = camp_cont+1
             j = j + 1
     if S[i] > campB and S[i] != campA:  # cerco di capire se S[i] è maggiore di campB ma al contempo è diverso da campA
         j=i+1
         while j < len(S): # controllo se c'è un altro numero uguale al possibile campione B nel resto della lista
             if S[j]==S[i]: # il numero esiste
                 campB = S[i]
                 camp_cont = camp_cont+1
             j = j + 1
     i=i+1

print("numero campioni", camp_cont)             
print(campA)
print(campB)
{% endhighlight %}

In questo modo ottengo il risultato voluto.

#### Output:

{% highlight python %}
numero campioni 3
9
7
{% endhighlight %}

---
title: 'Doppio campione'
date: '2023-04-18'
author: Fabio Mattei
layout: page
---

In una lista di numeri interi S devo trovare la coppia di numeri più grandi 

## Algoritmo del campione

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

Output:

{% highlight python %}
9
{% endhighlight %}


## Algoritmo della più grande coppia campione

{% highlight python %}
S = [4, 5, 7, 3, 4, 9, 3, 5, 7, 6, 4, 2, 9]

campA = 0
camp_cont = 0

i = 0
while i < len(S):
     if S[i] > campA:  # ho un campione possibile
         j=i+1
         while j < len(S): # controllo se c'è un altro numero uguale al campione possibile nel resto della lista
             if S[j]==S[i]: # il numero esiste
                 campA = S[i]
                 camp_cont= camp_cont+1
             j = j + 1
     i=i+1

print("numero campioni",camp_cont)             
print(campA)
{% endhighlight %}

Output:

{% highlight python %}
numero campioni 5
9
{% endhighlight %}

## Algoritmo delle due più grandi coppie campione

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
                 campB = campA  #il vecchio campione diventa campB
                 campA = S[i]
                 camp_cont= camp_cont+1
             j = j + 1
     i=i+1

print("numero campioni", camp_cont)             
print(campA)
print(campB)
{% endhighlight %}

Output:

{% highlight python %}
numero campioni 5
9
7
{% endhighlight %}

## Problemino: cosa succede se il piùgrande  campione anticipa il  più piccolo?

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

Output:

{% highlight python %}
numero campioni 1
9
0
{% endhighlight %}

## La soluzione

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
     if S[i] > campB and S[i] != campA:  # ho un campione possibile
         j=i+1
         while j < len(S): # controllo se c'è un altro numero uguale al campione possibile nel resto della lista
             if S[j]==S[i]: # il numero esiste
                 campB = S[i]
                 camp_cont = camp_cont+1
             j = j + 1
     i=i+1

print("numero campioni", camp_cont)             
print(campA)
print(campB)
{% endhighlight %}

Output:

{% highlight python %}
numero campioni 3
9
7
{% endhighlight %}

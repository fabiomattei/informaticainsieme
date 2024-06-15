---
layout: post
title:  "I commenti in Python"
date:   2024-06-15 00:00:00
categories: python sintassi
excerpt: Impariamo come commentare il codice in Python e perché è importante.
---

Essendo un linguaggio molto dinamico Python perde un pochino di precisione se comparato ai linguaggi staticamente tipati come C, C++ e Java. Mentre in questi linguaggio lo sviluppatore dichiara esplicitamente il tipo di ciascuna variabile in Python il tipo viene __indovinato__ dall'interprete e questo quando i sorgenti cominciano a crescere in dimensioni può portare a qualche mal di testa.

Vengono in aiuto i commenti che permettono al programmatore di esplicitare il tipo di una variabile e trascrivere una descrizione del suo contenuto a futura memoria. (leggere uno script rimane sempre una azione molto faticosa)

I commenti sono __informazioni__ che inseriamo all'interno del codice sorgente che non vengono interpretati dal computer, anzi vengono ingnorati da questo.

In Python è possibile iserire due tipi di commenti, quello a singola linea e quello multi-linea

{% highlight python %}
# questo è un commento
{% endhighlight %}

{% highlight python %}
"""
questo è un commento 
su più linee
"""
{% endhighlight %}

Se si desidera avere un codice pulito e leggibile queste sono le idee generali:

1. commenta il tuo codice il più possibile. I commenti all'interno del tuo codice sono **documentazione**. Il tuo futuro te o altri programmatori, o , saranno in grado di capire cosa sta succendendo all'interno di una logica.
2. Sii specifico, non dire cose come "questo fa x" ma scrivi "la funzione y fa x". Il codice è vivo, spesso si sposta, meglio essere precisi.
3. Non lasciare vecchio codice all'interno dei sorgenti disattivato attraverso un commento. E' una cattiva pratica. Sii sempre pulito ed ordinato.




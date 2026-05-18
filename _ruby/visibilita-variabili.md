---
title: 'Ruby: visibilità delle variabili'
date: '2020-11-15T08:47:01+01:00'
author: Fabio Mattei
layout: page
---

## Visibilità delle variabili

Il linguaggio Ruby rende la visibilità delle variabili immediatamente riconoscibile dal nome stesso.
Un prefisso diverso identifica il tipo di scope senza ambiguità.

{% highlight ruby %}
una_semplice_variabile   # variabile locale
@variabile_di_istanza    # appartiene all'oggetto corrente
@@variabile_di_classe    # condivisa da tutte le istanze della classe
$variabile_globale       # accessibile ovunque nel programma
COSTANTE                 # valore che non dovrebbe cambiare
{% endhighlight %}

Le variabili locali vivono solo nel blocco in cui sono definite. Le variabili di istanza
(`@`) persistono per tutta la vita dell'oggetto. Le variabili di classe (`@@`) sono
condivise tra tutte le istanze e la classe stessa. Le variabili globali (`$`) sono
accessibili da qualsiasi punto del programma ma andrebbero usate con parsimonia
perché rendono il codice difficile da comprendere e testare.

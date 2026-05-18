---
title: 'Ruby: operatori'
date: '2020-11-15T08:47:01+01:00'
author: Fabio Mattei
layout: page
---

## Operatori

### Assegnamento

Il primo operatore è il normale assegnamento con simbolo `=`.
A seguire vanno gli operatori di assegnamento che includono al loro interno un'operazione aritmetica.
Ad esempio `c += a` equivale a scrivere `c = c + a`.

{% highlight ruby %}
c = a + b
c += a
c -= a
c *= a
c /= a
c %= a
c **= a
{% endhighlight %}

{% highlight ruby %}
DEFINED? VAR_NAME # si usa per capire se una variabile è stata definita
{% endhighlight %}

### Assegnamento parallelo

{% highlight ruby %}
a, b = 1, 2

a, b = [1, 3]       # spacchetta l'array

a, b = [1, 2, 3, 4] # il resto viene ignorato

a, b, *c = [1, 3, 4, 5] # c = [4, 5] — operatore splat
{% endhighlight %}

### Scambio

L'assegnamento parallelo è comodo per scambiare tra loro i valori di due variabili.

{% highlight ruby %}
a, b = b, a
{% endhighlight %}

### Aritmetici

{% highlight ruby %}
3 + 2
3 - 2
3 * 2
3 / 2    # risulta 1
3 / 2.0  # risulta 1.5
3 ** 2   # risulta 9
3 % 2    # risulta 1
{% endhighlight %}

### Logici

{% highlight ruby %}
true and false
true && false   # equivale al precedente
true or false
true || false   # equivale al precedente
not true
! true          # equivale al precedente
{% endhighlight %}

### Confronto

{% highlight ruby %}
3 == 4
3 < 4
3 > 4
4 >= 4
4 <= 4
{% endhighlight %}

### Operatore spaceship (nave spaziale)

L'operatore `<=>` restituisce:

* `-1` se il valore sinistro è più piccolo del destro
* `0` se i valori sono uguali
* `1` se il valore destro è più piccolo del sinistro

{% highlight ruby %}
3 <=> 4   # -1
3 <=> 3   # 0
4 <=> 3   # 1
{% endhighlight %}

### Operatore ternario

Valuta un'espressione booleana e restituisce il primo valore se vera, il secondo se falsa.

{% highlight ruby %}
a = condizione ? valore_se_vero : valore_se_falso
{% endhighlight %}

### Operatore range

{% highlight ruby %}
1 .. 3   # 1, 2, 3  — limite destro incluso
1 ... 3  # 1, 2     — limite destro escluso
{% endhighlight %}

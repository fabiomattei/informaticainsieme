---
title: 'Ruby: funzioni'
date: '2020-11-15T08:47:01+01:00'
author: Fabio Mattei
layout: page
---

## Funzioni

In Ruby una funzione si definisce con la parola chiave `def` e si chiude con `end`.

{% highlight ruby %}
def somma(a, b)
  return a + b
end
{% endhighlight %}

### Parentesi opzionali

Le parentesi attorno ai parametri non sono obbligatorie.

{% highlight ruby %}
def somma a, b
  return a + b
end
{% endhighlight %}

### Return implicito

Se non viene specificato un `return`, Ruby restituisce automaticamente il risultato
dell'ultima espressione valutata.

{% highlight ruby %}
def somma a, b
  a + b
end
{% endhighlight %}

### Parametri facoltativi

Un parametro con valore di default diventa facoltativo: se il chiamante non lo passa,
viene usato il valore di default.

{% highlight ruby %}
def somma a, b, c = 2
  a + b + c
end
{% endhighlight %}

### Parametri variabili: splat e double splat

Ruby permette di raccogliere i parametri in eccesso in un array (`*args`) o in un
hash (`**kwargs`).

{% highlight ruby %}
def midiverto(a, b, c = 10, *args, **kwargs)
  # args raccoglie i parametri posizionali in eccesso come array
  # kwargs raccoglie i parametri con nome in eccesso come hash
  if c == 3
    return 0
  end
  a + b + c
end

x = midiverto 1, 2, 3, 4, 5, m: 100
# a = 1, b = 2, c = 3, args = [4, 5], kwargs = { m: 100 }
{% endhighlight %}

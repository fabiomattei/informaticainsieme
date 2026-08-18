---
title: 'Godot: operatori'
date: '2026-08-18T09:25:00+01:00'
author: Fabio Mattei
layout: page
---

## Operatori

### Assegnamento

Il primo operatore è il normale assegnamento con simbolo `=`. A seguire gli
operatori di assegnamento composta, che includono un'operazione aritmetica.

{% highlight gdscript %}
c = a + b
c += a
c -= a
c *= a
c /= a
c %= a
{% endhighlight %}

GDScript **non** ha un operatore di potenza (`**` non esiste): si usa la funzione
`pow(base, esponente)`.

### Aritmetici

{% highlight gdscript %}
3 + 2
3 - 2
3 * 2
3 / 2       # risulta 1
3 / 2.0     # risulta 1.5
pow(3, 2)   # risulta 9.0
3 % 2       # risulta 1
{% endhighlight %}

### Logici

GDScript accetta sia la forma simbolica sia quella verbale, come Ruby.

{% highlight gdscript %}
true and false
true && false   # equivale al precedente
true or false
true || false   # equivale al precedente
not true
!true            # equivale al precedente
{% endhighlight %}

### Confronto

{% highlight gdscript %}
3 == 4
3 != 4
3 < 4
3 > 4
4 >= 4
4 <= 4
{% endhighlight %}

GDScript **non** ha l'operatore "nave spaziale" `<=>` di Ruby: per ottenere lo stesso
risultato (-1, 0 o 1) si combinano i confronti normali, oppure si usa direttamente
`<` e `>` in un `if`.

### Operatore ternario

Come in Ruby, valuta un'espressione booleana e restituisce il primo valore se vera,
il secondo se falsa — ma con una sintassi diversa, ispirata a Python: il valore
`if`/`else` sta nel mezzo, non la condizione.

{% highlight gdscript %}
var a = valore_se_vero if condizione else valore_se_falso
{% endhighlight %}

{% highlight gdscript %}
var eta = 20
var stato = "maggiorenne" if eta >= 18 else "minorenne"
print(stato)   # maggiorenne
{% endhighlight %}

### L'operatore in

GDScript introduce l'operatore `in`, non presente in Ruby con questa sintassi, per
verificare se un valore è contenuto in una collezione (array, dizionario, stringa,
range).

{% highlight gdscript %}
print(3 in [1, 2, 3])          # true
print("a" in "banana")         # true
print("nome" in {"nome": 1})   # true (verifica le chiavi)
{% endhighlight %}

### Operatore range

La funzione `range()`, usata quasi sempre nei cicli `for`, sostituisce l'operatore
`..`/`...` di Ruby. Ne parliamo in dettaglio nella pagina su [range()]({{ site.baseurl }}{% link _godot/range.md %}.html).

{% highlight gdscript %}
range(1, 4)    # 1, 2, 3   — il limite superiore è sempre escluso
range(4)       # 0, 1, 2, 3
{% endhighlight %}

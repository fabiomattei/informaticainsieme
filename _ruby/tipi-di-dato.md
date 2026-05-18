---
title: 'Ruby: tipi di dato'
date: '2020-11-15T08:47:01+01:00'
author: Fabio Mattei
layout: page
---

## Tipi di dato

Ruby definisce numerosi tipi di dato, alcuni sono i soliti: stringa, intero, float, boolean.

Altri sono le costanti, riconoscibili perché sempre maiuscole e danno un warning se modificate,
i simboli, che sono stringhe di testo più leggere e con meno funzionalità ma molto più veloci.

Infine abbiamo gli array, corrispondenti alle liste in Python, e gli hash, corrispondenti ai
dizionari in Python.

{% highlight ruby %}
s = "string"
s = 'also string'
i = 100
f = 3.14
ofcourse = true
CON = 'constant' # warning if modified
:symbol
none = nil
array = [i, f, s, {:symbol => 'lol'}, CON]
hash = { :5 => 42, 'key': 'value', anything: ar }
{% endhighlight %}

Gli hash sono interessanti perché i dati vi possono essere inseriti in tre modi diversi:

* `:5 => 42`          — simbolo => valore
* `'key': 'value'`    — 'chiave': 'valore'
* `anything: ar`      — simbolo: valore

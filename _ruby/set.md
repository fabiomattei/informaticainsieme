---
title: 'Ruby: i set'
date: '2020-11-15T08:47:01+01:00'
author: Fabio Mattei
layout: page
---

## Un insieme senza duplicati

Un **Set** è una collezione di elementi **non ordinata** e **senza
duplicati**. A differenza dell'array, in un set ogni valore può comparire
al massimo una volta: se si prova ad aggiungere un elemento già presente,
l'operazione viene ignorata silenziosamente.

Il Set è il corrispettivo informatico dell'insieme matematico. Le
operazioni classiche della teoria degli insiemi — unione, intersezione,
differenza — sono disponibili come metodi Ruby.

Per usare i Set è necessario caricare la libreria standard con
`require 'set'`.

---

## Creare un set

#### Esercizio 1
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
require 'set'

vuoto   = Set.new
numeri  = Set.new([1, 2, 3, 4, 5])
lettere = Set.new(['a', 'b', 'c'])

puts numeri.inspect     # #<Set: {1, 2, 3, 4, 5}>
puts numeri.length      # 5

# i duplicati vengono eliminati automaticamente
con_doppioni = Set.new([1, 2, 2, 3, 3, 3])
puts con_doppioni.inspect   # #<Set: {1, 2, 3}>
{% endhighlight %}

---

## Aggiungere e rimuovere elementi

#### Esercizio 2
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
require 'set'

frutti = Set.new(['mela', 'pera', 'banana'])

frutti.add('kiwi')
puts frutti.include?('kiwi')    # true

frutti << 'mango'               # << è sinonimo di add
puts frutti.length              # 5

frutti.delete('pera')
puts frutti.include?('pera')    # false

# aggiungere un elemento già presente non cambia nulla
frutti.add('mela')
puts frutti.length              # 4 (invariato)
{% endhighlight %}

---

## Verificare l'appartenenza

Il metodo `.include?` è molto più rapido di quello dell'array per
collezioni grandi, perché il set usa una struttura interna ottimizzata
per la ricerca.

#### Esercizio 3
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
require 'set'

vocali = Set.new(['a', 'e', 'i', 'o', 'u'])

print "Inserisci una lettera: "
lettera = gets.chomp.downcase

if vocali.include?(lettera)
  puts "#{lettera} è una vocale"
else
  puts "#{lettera} è una consonante"
end
{% endhighlight %}

---

## Operazioni tra insiemi

Queste operazioni restituiscono sempre un nuovo Set e non modificano
gli originali.

| Operazione      | Operatore | Metodo              | Significato                         |
|-----------------|:---------:|---------------------|-------------------------------------|
| Unione          | `\|`      | `.union(altro)`      | tutti gli elementi di entrambi      |
| Intersezione    | `&`       | `.intersection(altro)` | solo gli elementi comuni         |
| Differenza      | `-`       | `.difference(altro)` | elementi del primo non nel secondo  |
| Differenza simm.| `^`       |                     | elementi non comuni a entrambi      |

#### Esercizio 4
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
require 'set'

classe_a = Set.new(['Alice', 'Bruno', 'Carla', 'Davide'])
classe_b = Set.new(['Carla', 'Davide', 'Elena', 'Franco'])

puts "Unione:"
puts (classe_a | classe_b).inspect

puts "Intersezione (in entrambe le classi):"
puts (classe_a & classe_b).inspect

puts "Solo in classe A:"
puts (classe_a - classe_b).inspect

puts "Solo in classe B:"
puts (classe_b - classe_a).inspect
{% endhighlight %}

---

## Relazioni tra insiemi

#### Esercizio 5
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
require 'set'

tutti  = Set.new([1, 2, 3, 4, 5, 6, 7, 8, 9, 10])
pari   = Set.new([2, 4, 6, 8, 10])
primi  = Set.new([2, 3, 5, 7])

puts pari.subset?(tutti)        # true  (pari ⊆ tutti)
puts tutti.superset?(pari)      # true  (tutti ⊇ pari)
puts pari.subset?(primi)        # false
puts pari.intersect?(primi)     # true  (hanno il 2 in comune)
puts pari.disjoint?(Set.new([1, 3, 5, 7, 9]))  # true (nessun elemento in comune)
{% endhighlight %}

---

## Metodi utili

| Metodo             | Significato                                         |
|--------------------|-----------------------------------------------------|
| `.add(x)` / `<< x` | aggiunge `x` (ignorato se già presente)             |
| `.delete(x)`       | rimuove `x`                                         |
| `.include?(x)`     | `true` se `x` è nel set                             |
| `.length`          | numero di elementi                                  |
| `.empty?`          | `true` se il set è vuoto                            |
| `.to_a`            | converte in array                                   |
| `.subset?(altro)`  | `true` se tutti gli elementi sono nell'altro set    |
| `.superset?(altro)`| `true` se contiene tutti gli elementi dell'altro    |
| `.intersect?(altro)`| `true` se i due set hanno almeno un elemento comune|
| `.disjoint?(altro)`| `true` se i due set non hanno elementi in comune   |

---

## Esercizi

#### Esercizio 6
Scrivi un programma che lette due liste di numeri separatamente (una
per riga) trovi e stampi gli elementi presenti in entrambe le liste.

#### Esercizio 7
Scrivi un programma che dato un array con possibili duplicati restituisca
un array con soli elementi unici usando un Set.

#### Esercizio 8
Scrivi un programma che letti i nomi degli studenti di due classi
stampi: quanti sono in totale (senza ripetizioni), quanti frequentano
entrambe le classi e i nomi di chi è solo nella prima classe.

#### Esercizio 9
Scrivi un programma che verifichi se due insiemi sono disgiunti (non
hanno nessun elemento in comune) leggendo i valori in input.

#### Esercizio 10
Dato il testo di una frase letta in input, scrivi un programma che
utilizzi un Set per trovare e stampare le lettere distinte usate
(senza spazi e senza ripetizioni), in ordine alfabetico.

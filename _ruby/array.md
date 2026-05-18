---
title: 'Ruby: gli array'
date: '2020-11-15T08:47:01+01:00'
author: Fabio Mattei
layout: page
---

## Una sequenza di elementi

Un **array** è una struttura dati che contiene una sequenza ordinata di
elementi. A differenza di una variabile, che conserva un solo valore, un
array può conservarne molti all'interno di un unico contenitore.

Ruby fornisce gli array come tipo built-in: fanno parte delle caratteristiche
di base del linguaggio. Gli elementi possono essere di qualsiasi tipo e
possono anche essere miscelati.

## Creare un array

Un array si crea con le parentesi quadre `[]`. Gli elementi sono separati
da virgole.

#### Esercizio 1
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
vuoto   = []
numeri  = [0, 1, 2, 3]
lettere = ['a', 'b', 'c', 'd', 'e']
parole  = ['mattina', 'pomeriggio', 'sera']
misto   = [42, "ciao", true, 3.14]

puts numeri.length    # 4
puts lettere.length   # 5
puts misto.inspect    # [42, "ciao", true, 3.14]
{% endhighlight %}

---

## Accedere agli elementi

Ogni elemento ha un **indice** che parte da 0. Gli indici negativi contano
dalla fine: `-1` è l'ultimo elemento, `-2` il penultimo.

#### Esercizio 2
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
lettere = ['a', 'b', 'c', 'd', 'e']
puts lettere[0]    # a
puts lettere[2]    # c
puts lettere[-1]   # e  (ultimo)
puts lettere[-2]   # d  (penultimo)
puts lettere.first # a
puts lettere.last  # e
{% endhighlight %}

| indice          |  0  |  1  |  2  |  3  |  4  |
|-----------------|:---:|:---:|:---:|:---:|:---:|
| contenuto       | 'a' | 'b' | 'c' | 'd' | 'e' |
| indice negativo | -5  | -4  | -3  | -2  | -1  |

---

## Modificare gli elementi

Si può cambiare il valore di un elemento assegnando direttamente tramite
il suo indice.

#### Esercizio 3
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
lettere = ['a', 'b', 'c', 'd', 'e']
lettere[0] = 'u'
lettere[-2] = 'k'
puts lettere.inspect    # ["u", "b", "c", "k", "e"]
{% endhighlight %}

---

## Aggiungere e rimuovere elementi

Ruby offre diversi metodi per aggiungere e togliere elementi da un array.

| Metodo / Operatore | Effetto                                         |
|--------------------|-------------------------------------------------|
| `arr << x`         | aggiunge `x` in fondo (shovel operator)         |
| `arr.push(x)`      | aggiunge `x` in fondo (equivalente a `<<`)      |
| `arr.pop`          | rimuove e restituisce l'ultimo elemento         |
| `arr.unshift(x)`   | aggiunge `x` all'inizio                         |
| `arr.shift`        | rimuove e restituisce il primo elemento         |

#### Esercizio 4
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
classe = ['Gino', 'Sandra']
puts classe.inspect             # ["Gino", "Sandra"]

classe << 'Mario'
puts classe.inspect             # ["Gino", "Sandra", "Mario"]

rimosso = classe.pop
puts rimosso                    # Mario
puts classe.inspect             # ["Gino", "Sandra"]

classe.unshift('Anna')
puts classe.inspect             # ["Anna", "Gino", "Sandra"]
{% endhighlight %}

---

## Operatori sugli array

Gli operatori `+` e `*` funzionano sugli array con lo stesso significato
che hanno sulle stringhe.

#### Esercizio 5
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
vocali = ['a', 'e'] + ['i', 'o', 'u']
puts vocali.inspect             # ["a", "e", "i", "o", "u"]

ripetute = ['ha'] * 3
puts ripetute.inspect           # ["ha", "ha", "ha"]
{% endhighlight %}

---

## Sottarray (slicing)

Con la notazione `array[inizio..fine]` si estrae una porzione dell'array.
Come per le stringhe, `..` include l'estremo finale, `...` lo esclude.

#### Esercizio 6
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
lettere = ['a', 'b', 'c', 'd', 'e']
puts lettere[1..3].inspect    # ["b", "c", "d"]
puts lettere[0...3].inspect   # ["a", "b", "c"]
puts lettere[-3..-1].inspect  # ["c", "d", "e"]
{% endhighlight %}

---

## Verificare se un elemento è presente

Il metodo `.include?` restituisce `true` se l'elemento cercato è
contenuto nell'array.

#### Esercizio 7
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
lettere = ['a', 'b', 'c', 'd', 'e']

if lettere.include?('c')
  puts "La lettera c è nella lista"
end

unless lettere.include?('k')
  puts "La lettera k NON è nella lista"
end
{% endhighlight %}

---

## Visitare un array

Visitare un array significa **esaminare uno alla volta ciascun elemento**
e applicare delle operazioni a ognuno. In Ruby il modo più naturale è
il metodo `each`.

#### Esercizio 8
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
numeri = [1, 2, 3, 4, 5, 6]
for n in numeri
  if n % 2 == 0
    puts "#{n} è pari"
  end
end
{% endhighlight %}

#### Esercizio 9
Copia il seguente codice nell'editor e fallo eseguire. `each` è lo stile
idiomatico Ruby per visitare un array.

{% highlight ruby %}
parole = ['mattina', 'pomeriggio', 'sera']
parole.each do |parola|
  puts "Buona #{parola}!"
end
{% endhighlight %}

#### Esercizio 10
Quando serve anche l'indice si usa `each_with_index`.

{% highlight ruby %}
nomi = ['Alice', 'Bruno', 'Carla', 'Davide']
nomi.each_with_index do |nome, i|
  puts "#{i}: #{nome}"
end
{% endhighlight %}

Si può visitare un array anche con un ciclo `while` e un contatore
esplicito, utile quando si vuole controllare manualmente l'indice.

{% highlight ruby %}
numeri = [10, 20, 30, 40, 50]
indice = 0
while indice < numeri.length
  puts numeri[indice]
  indice = indice + 1
end
{% endhighlight %}

---

## Metodi utili

Ruby mette a disposizione molti metodi predefiniti che operano sull'intero
array.

| Metodo           | Significato                                      |
|------------------|--------------------------------------------------|
| `.length`        | numero di elementi                               |
| `.min`           | elemento più piccolo                             |
| `.max`           | elemento più grande                              |
| `.sum`           | somma di tutti gli elementi                      |
| `.sort`          | restituisce una copia ordinata                   |
| `.sort!`         | ordina in-place (modifica l'array originale)     |
| `.reverse`       | restituisce una copia capovolta                  |
| `.uniq`          | rimuove i duplicati                              |
| `.flatten`       | appiattisce array annidati                       |
| `.compact`       | rimuove i valori `nil`                           |
| `.count(x)`      | conta le occorrenze di `x`                       |

#### Esercizio 11
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
numeri = [5, 3, 8, 1, 9, 2, 7, 4, 6]
puts numeri.min      # 1
puts numeri.max      # 9
puts numeri.sum      # 45
puts numeri.sort.inspect    # [1, 2, 3, 4, 5, 6, 7, 8, 9]
puts numeri.reverse.inspect # [6, 4, 7, 2, 9, 1, 8, 3, 5]

con_doppioni = [1, 2, 2, 3, 3, 3, 4]
puts con_doppioni.uniq.inspect  # [1, 2, 3, 4]
{% endhighlight %}

---

## Costruire un array leggendo l'input

#### Esercizio 12
Copia il seguente codice nell'editor e fallo eseguire. Il programma legge
10 numeri e li conserva in un array.

{% highlight ruby %}
numeri = []
10.times do
  print "Inserisci un numero: "
  numeri << gets.chomp.to_i
end
puts "Hai inserito: #{numeri.inspect}"
puts "Somma: #{numeri.sum}"
puts "Massimo: #{numeri.max}"
puts "Minimo: #{numeri.min}"
{% endhighlight %}

---

## Esercizi

#### Esercizio 13
Inizializza una lista di sei nomi a tua scelta. Scrivi il nome all'indice 3
e sostituisci il nome all'indice 5 con uno letto in input dall'utente.

#### Esercizio 14
Inizializza una lista di sei nomi e stampali tutti usando un ciclo `for`.

#### Esercizio 15
Inizializza una lista di numeri interi a tua scelta e scrivi solo i numeri
dispari.

#### Esercizio 16
Scrivi un programma che legga 10 numeri, li memorizzi in un array e li
stampi in ordine inverso.

#### Esercizio 17
Scrivi un programma che legga 10 numeri e li memorizzi in due array
separati: `grandi` (valori ≥ 100) e `piccoli` (valori < 100). I numeri
negativi non vanno memorizzati. Stampa entrambi gli array alla fine.

#### Esercizio 18
Scrivi un programma che dati due array di numeri interi trovi e stampi
gli elementi comuni a entrambi.

#### Esercizio 19
Scrivi un programma che inizializzi un array con i numeri da 1 a 20 e
calcoli la somma degli elementi in posizione pari (indici 0, 2, 4, …).

#### Esercizio 20
Scrivi un programma che legga N numeri, li memorizzi in un array e
trovi il secondo elemento più grande (senza usare `.sort`).

#### Esercizio 21
Scrivi un programma che inizializzi una lista di nomi e crei una nuova
lista contenente solo i nomi che iniziano per vocale.

#### Esercizio 22
Scrivi un programma che calcoli la frequenza degli elementi di un array
(quante volte compare ciascun elemento) e stampi ogni elemento con il
suo numero di occorrenze.

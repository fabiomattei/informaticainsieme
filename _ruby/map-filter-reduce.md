---
title: 'Ruby: map, select e reduce'
date: '2020-11-15T08:47:01+01:00'
author: Fabio Mattei
layout: page
---

## Trasformare, filtrare, ridurre

Quando si lavora con gli array, tre operazioni tornano continuamente:

- **trasformare** ogni elemento in qualcos'altro (`map`)
- **filtrare** tenendo solo gli elementi che soddisfano una condizione (`select`)
- **ridurre** tutti gli elementi a un unico valore (`reduce`)

In Ruby queste operazioni si esprimono con metodi che accettano un **blocco**:
un pezzo di codice racchiuso tra `{ |x| ... }` o `do |x| ... end` che viene
eseguito una volta per ogni elemento dell'array.

---

## Blocchi, proc e lambda

Prima di vedere i tre metodi, è utile capire come Ruby rappresenta
"una funzione da passare a un altro metodo".

In Ruby si usano tre forme equivalenti:

{% highlight ruby %}
# 1. blocco inline (la forma più comune)
[1, 2, 3].map { |x| x * 2 }

# 2. lambda (funzione anonima assegnata a una variabile)
doppio = ->(x) { x * 2 }
[1, 2, 3].map { |x| doppio.call(x) }

# 3. shorthand con & (passa un metodo come blocco)
["alice", "bob"].map(&:upcase)
{% endhighlight %}

La forma più usata è il **blocco inline**. La lambda è utile quando si vuole
riutilizzare la stessa logica in più punti.

---

## Il metodo map

`.map` (sinonimo: `.collect`) restituisce un **nuovo array** in cui ogni
elemento è il risultato del blocco applicato all'elemento originale.
L'array di partenza non viene modificato.

#### Esercizio 1
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
numeri = [1, 2, 3, 4, 5]

doppi = numeri.map { |n| n * 2 }
puts doppi.inspect    # [2, 4, 6, 8, 10]

quadrati = numeri.map { |n| n ** 2 }
puts quadrati.inspect # [1, 4, 9, 16, 25]

puts numeri.inspect   # [1, 2, 3, 4, 5] — invariato
{% endhighlight %}

Il blocco `{ |n| n * 2 }` viene eseguito per ciascun elemento: `n` assume
il valore dell'elemento corrente e il risultato dell'espressione diventa il
nuovo elemento dell'array restituito.

---

#### Esercizio 2 — map su stringhe

{% highlight ruby %}
parole = ["alice", "bob", "carla"]

maiuscole = parole.map { |p| p.upcase }
puts maiuscole.inspect   # ["ALICE", "BOB", "CARLA"]

lunghezze = parole.map { |p| p.length }
puts lunghezze.inspect   # [5, 3, 5]
{% endhighlight %}

---

#### Esercizio 3 — lambda riutilizzabile

{% highlight ruby %}
converti_celsius = ->(c) { (c * 9.0 / 5) + 32 }

temperature_c = [0, 20, 37, 100]
temperature_f = temperature_c.map { |c| converti_celsius.call(c) }
puts temperature_f.inspect   # [32.0, 68.0, 98.6, 212.0]
{% endhighlight %}

La lambda `converti_celsius` è definita una sola volta e può essere usata
ovunque nel programma.

---

#### Esercizio 4 — shorthand &:metodo

Quando il blocco chiama un solo metodo senza argomenti si può usare la
notazione `&:nome_metodo`, che è equivalente a `{ |x| x.nome_metodo }`.

{% highlight ruby %}
parole = ["  alice  ", "  bob  ", "  carla  "]

pulite = parole.map(&:strip)
puts pulite.inspect    # ["alice", "bob", "carla"]

numeri_str = ["1", "2", "3", "4"]
interi = numeri_str.map(&:to_i)
puts interi.inspect    # [1, 2, 3, 4]
{% endhighlight %}

---

## Il metodo select (filter)

`.select` (sinonimo: `.filter`) restituisce un **nuovo array** contenente
solo gli elementi per cui il blocco restituisce `true`.
Il metodo opposto è `.reject`, che tiene gli elementi per cui il blocco
restituisce `false`.

#### Esercizio 5

{% highlight ruby %}
numeri = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

pari    = numeri.select { |n| n.even? }
puts pari.inspect     # [2, 4, 6, 8, 10]

dispari = numeri.reject { |n| n.even? }
puts dispari.inspect  # [1, 3, 5, 7, 9]

grandi  = numeri.select { |n| n > 5 }
puts grandi.inspect   # [6, 7, 8, 9, 10]
{% endhighlight %}

---

#### Esercizio 6 — select su stringhe

{% highlight ruby %}
parole = ["gatto", "cane", "elefante", "ape", "coccodrillo"]

lunghe  = parole.select { |p| p.length > 4 }
puts lunghe.inspect    # ["gatto", "elefante", "coccodrillo"]

con_a   = parole.select { |p| p.include?("a") }
puts con_a.inspect     # ["gatto", "cane", "elefante", "ape"]
{% endhighlight %}

---

#### Esercizio 7 — select su hash

`.select` funziona anche sugli hash: il blocco riceve chiave e valore.

{% highlight ruby %}
voti = { fisica: 8, matematica: 6, storia: 4, inglese: 7 }

sufficienti = voti.select { |materia, voto| voto >= 6 }
puts sufficienti.inspect
# {:fisica=>8, :matematica=>6, :inglese=>7}

insufficienti = voti.reject { |materia, voto| voto >= 6 }
puts insufficienti.inspect
# {:storia=>4}
{% endhighlight %}

---

## Il metodo reduce

`.reduce` (sinonimo: `.inject`) **riduce** un array a un singolo valore
applicando il blocco in modo cumulativo. Il blocco riceve due parametri:
l'**accumulatore** (il valore calcolato finora) e l'**elemento corrente**.

{% highlight ruby %}
array.reduce(valore_iniziale) { |accumulatore, elemento| ... }
{% endhighlight %}

Se il valore iniziale è omesso, il primo elemento dell'array diventa
l'accumulatore iniziale.

#### Esercizio 8 — somma e prodotto

{% highlight ruby %}
numeri = [1, 2, 3, 4, 5]

somma   = numeri.reduce(0) { |acc, n| acc + n }
puts somma     # 15

prodotto = numeri.reduce(1) { |acc, n| acc * n }
puts prodotto  # 120
{% endhighlight %}

**Traccia di esecuzione per la somma:**

| Passo | acc (prima) | n | acc (dopo) |
|------:|:-----------:|:-:|:----------:|
| 1     | 0           | 1 | 1          |
| 2     | 1           | 2 | 3          |
| 3     | 3           | 3 | 6          |
| 4     | 6           | 4 | 10         |
| 5     | 10          | 5 | 15         |

---

#### Esercizio 9 — shorthand con simbolo

Per le operazioni aritmetiche standard si può usare il nome dell'operatore
come simbolo, eliminando il blocco.

{% highlight ruby %}
numeri = [1, 2, 3, 4, 5]

puts numeri.reduce(:+)   # 15
puts numeri.reduce(:*)   # 120

parole = ["uno", "due", "tre"]
puts parole.reduce { |acc, p| acc + ", " + p }
# "uno, due, tre"
{% endhighlight %}

---

#### Esercizio 10 — reduce per trovare il massimo

{% highlight ruby %}
numeri = [3, 1, 8, 2, 7, 4]

massimo = numeri.reduce { |acc, n| acc > n ? acc : n }
puts massimo   # 8

# equivalente con il metodo max:
puts numeri.max   # 8
{% endhighlight %}

---

## Concatenare map, select e reduce

La vera potenza di questi metodi emerge quando si **incatenano**:
il risultato di uno diventa l'input del successivo.

#### Esercizio 11

{% highlight ruby %}
numeri = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

# somma dei quadrati dei numeri pari
risultato = numeri
              .select { |n| n.even? }
              .map    { |n| n ** 2 }
              .reduce(0) { |acc, n| acc + n }

puts risultato   # 4 + 16 + 36 + 64 + 100 = 220
{% endhighlight %}

**Passo per passo:**
1. `.select` → `[2, 4, 6, 8, 10]`
2. `.map`    → `[4, 16, 36, 64, 100]`
3. `.reduce` → `220`

---

#### Esercizio 12 — catena su stringhe

{% highlight ruby %}
frasi = [
  "Ruby è un linguaggio elegante",
  "Python è molto usato",
  "Ruby on Rails è un framework web",
  "C++ è veloce"
]

# lunghezza totale delle frasi che parlano di Ruby
totale = frasi
           .select { |f| f.include?("Ruby") }
           .map    { |f| f.length }
           .reduce(0, :+)

puts totale   # somma delle lunghezze delle frasi con "Ruby"
{% endhighlight %}

---

## Riepilogo

| Metodo          | Cosa restituisce          | Uso tipico                              |
|-----------------|---------------------------|-----------------------------------------|
| `.map`          | array della stessa taglia | trasformare ogni elemento               |
| `.collect`      | (sinonimo di map)         |                                         |
| `.select`       | array più piccolo o uguale| tenere gli elementi che soddisfano      |
| `.filter`       | (sinonimo di select)      | una condizione                          |
| `.reject`       | array più piccolo o uguale| scartare gli elementi che soddisfano    |
| `.reduce`       | valore singolo            | sommare, moltiplicare, concatenare      |
| `.inject`       | (sinonimo di reduce)      | accumulare un risultato                 |

---

## Esercizi

#### Esercizio 13
Dato un array di numeri interi, usa `.map` per creare un array con il
valore assoluto di ciascun numero (suggerimento: `.abs`).

#### Esercizio 14
Dato un array di stringhe, usa `.select` per ottenere solo le parole
che iniziano con la lettera "a" (indipendentemente dal maiuscolo/minuscolo).

#### Esercizio 15
Dato un array di numeri, usa `.reduce` per trovare il minimo senza
usare il metodo `.min`.

#### Esercizio 16
Dato un array di parole, usa `.map` per creare un array di coppie
`[parola, lunghezza]`. Esempio: `["gatto", "cane"]` →
`[["gatto", 5], ["cane", 4]]`.

#### Esercizio 17
Dato un array di numeri interi da 1 a 20, usa una catena
`.select` → `.map` → `.reduce` per calcolare la somma dei cubi
dei numeri dispari.

#### Esercizio 18
Dato un array di hash `{ nome: ..., voto: ... }`, usa `.select` per
ottenere solo gli studenti promossi (voto ≥ 6), poi `.map` per
estrarre solo i nomi, e infine `.reduce` per costruire una stringa
con i nomi separati da virgola.

#### Esercizio 19
Scrivi una lambda `positivo` che restituisce `true` se un numero è
maggiore di zero. Usala con `.select` per filtrare un array di interi.

#### Esercizio 20
Dato un testo letto in input, usa `.split`, `.map` e `.select` per
stampare le parole uniche (senza duplicati) che hanno più di 4 lettere,
ordinate in ordine alfabetico.

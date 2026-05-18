---
title: 'Ruby: i range'
date: '2020-11-15T08:47:01+01:00'
author: Fabio Mattei
layout: page
---

## Una sequenza continua di valori

Un **range** rappresenta un intervallo continuo di valori compresi tra un
estremo iniziale e uno finale. In Ruby si scrive con l'operatore `..`
(estremi inclusi) o `...` (estremo finale escluso).

{% highlight ruby %}
1..10    # da 1 a 10 compresi
1...10   # da 1 a 9  (10 escluso)
'a'..'z' # tutte le lettere minuscole dell'alfabeto
{% endhighlight %}

I range sono molto usati nei cicli `for`, per controllare se un valore
rientra in un intervallo e per generare sequenze.

---

## Creare e usare un range

#### Esercizio 1
Copia il seguente codice nell'editor e fallo eseguire. Osserva la
differenza tra `..` e `...`.

{% highlight ruby %}
r1 = 1..5
r2 = 1...5

puts r1.to_a.inspect    # [1, 2, 3, 4, 5]
puts r2.to_a.inspect    # [1, 2, 3, 4]

puts r1.min    # 1
puts r1.max    # 5
puts r1.sum    # 15
puts r1.count  # 5
{% endhighlight %}

Il metodo `.to_a` converte il range in un array di tutti i suoi elementi.

---

## Range nei cicli

#### Esercizio 2
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
for i in 1..5
  puts i
end
{% endhighlight %}

{% highlight ruby %}
(1..10).each do |i|
  print "#{i} "
end
puts
{% endhighlight %}

---

## Verificare se un valore è nel range

Il metodo `.include?` (o `.cover?`) controlla se un valore rientra
nell'intervallo del range.

#### Esercizio 3
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
print "Inserisci la tua età: "
eta = gets.chomp.to_i

case eta
when 0..12
  puts "Bambino"
when 13..17
  puts "Adolescente"
when 18..64
  puts "Adulto"
when 65..Float::INFINITY
  puts "Anziano"
else
  puts "Età non valida"
end
{% endhighlight %}

I range si usano spesso nei `case/when` come alternativa più leggibile
a una catena di `elsif`.

---

## Range di caratteri

I range funzionano anche con le lettere, seguendo l'ordine alfabetico.

#### Esercizio 4
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
puts ('a'..'e').to_a.inspect    # ["a", "b", "c", "d", "e"]
puts ('A'..'Z').count           # 26

if ('a'..'z').include?('m')
  puts "m è una lettera minuscola"
end
{% endhighlight %}

---

## Range con step

Il metodo `.step` percorre il range avanzando di un passo personalizzato.

#### Esercizio 5
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
# numeri pari da 2 a 20
(2..20).step(2) do |n|
  print "#{n} "
end
puts

# numeri decimali
(0.0..1.0).step(0.25) do |n|
  print "#{n} "
end
puts
{% endhighlight %}

---

## Metodi utili

| Metodo          | Significato                                      |
|-----------------|--------------------------------------------------|
| `.min`          | valore minimo                                    |
| `.max`          | valore massimo                                   |
| `.sum`          | somma di tutti i valori                          |
| `.count`        | numero di elementi                               |
| `.include?(x)`  | `true` se `x` è nel range                        |
| `.cover?(x)`    | come `include?` ma più efficiente per range ampi |
| `.to_a`         | converte in array                                |
| `.each { }`     | itera su ogni elemento                           |
| `.step(n) { }`  | itera avanzando di `n` alla volta                |
| `.first(n)`     | primi `n` elementi                               |
| `.last(n)`      | ultimi `n` elementi                              |

---

## Esercizi

#### Esercizio 6
Scrivi un programma che letto un numero N stampi tutti i numeri pari
compresi tra 1 e N usando un range e `.step`.

#### Esercizio 7
Scrivi un programma che letto un numero verifichi a quale intervallo
appartiene tra: [0, 10), [10, 50), [50, 100], oppure fuori intervallo.

#### Esercizio 8
Scrivi un programma che usando un range di lettere stampi l'alfabeto
maiuscolo separato da trattini: `A-B-C-D-…-Z`.

#### Esercizio 9
Scrivi un programma che letto un numero intero N calcoli la somma di
tutti i numeri da 1 a N usando `.sum` su un range.

#### Esercizio 10
Scrivi un programma che generi la tabella della moltiplicazione di un
numero N letto in input, usando un range e `.each`.

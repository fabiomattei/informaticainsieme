---
title: 'Ruby: operatori logici'
date: '2020-11-15T08:47:01+01:00'
author: Fabio Mattei
layout: page
---

## Operatori logici

Gli operatori logici permettono di **combinare più condizioni** in un'unica
espressione. Il risultato è sempre un valore booleano: `true` o `false`.

Ruby dispone di tre operatori logici fondamentali: **AND**, **OR** e **NOT**.
Ciascuno esiste in due forme: una simbolica (`&&`, `||`, `!`) e una verbale
(`and`, `or`, `not`). Le forme simboliche hanno priorità più alta nella
valutazione delle espressioni e sono quelle preferite nella pratica.

---

## AND — entrambe le condizioni devono essere vere

L'operatore `&&` restituisce `true` solo se **entrambe** le condizioni sono
vere. Basta che una sola sia falsa perché il risultato sia falso.

| A     | B     | A && B |
|-------|-------|--------|
| true  | true  | true   |
| true  | false | false  |
| false | true  | false  |
| false | false | false  |

#### Esercizio 1
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
print "Inserisci la temperatura dell'acqua: "
temperatura = gets.chomp.to_i

if temperatura > 0 && temperatura <= 100
  puts "Stato liquido"
elsif temperatura <= 0
  puts "Stato solido"
else
  puts "Stato gassoso"
end
{% endhighlight %}

La stringa "Stato liquido" viene stampata solo se la temperatura è
**contemporaneamente** maggiore di 0 **e** minore o uguale a 100. Devono
essere vere entrambe le condizioni.

#### Esercizio 2
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
print "Inserisci un numero: "
n = gets.chomp.to_i

if n >= 10 && n <= 99
  puts "#{n} è un numero a due cifre"
else
  puts "#{n} non è un numero a due cifre"
end
{% endhighlight %}

---

## OR — almeno una condizione deve essere vera

L'operatore `||` restituisce `true` se **almeno una** delle due condizioni
è vera. Diventa falso solo se entrambe le condizioni sono false.

| A     | B     | A \|\| B |
|-------|-------|----------|
| true  | true  | true     |
| true  | false | true     |
| false | true  | true     |
| false | false | false    |

#### Esercizio 3
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
print "Inserisci un voto (1-10): "
voto = gets.chomp.to_i

if voto < 1 || voto > 10
  puts "Voto non valido"
elsif voto >= 6
  puts "Promosso"
else
  puts "Bocciato"
end
{% endhighlight %}

Il voto viene rifiutato se è minore di 1 **oppure** maggiore di 10: basta
che una sola delle due condizioni sia vera.

#### Esercizio 4
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
print "Inserisci il mese (1-12): "
mese = gets.chomp.to_i

if mese == 12 || mese == 1 || mese == 2
  puts "Inverno"
elsif mese >= 3 && mese <= 5
  puts "Primavera"
elsif mese >= 6 && mese <= 8
  puts "Estate"
elsif mese >= 9 && mese <= 11
  puts "Autunno"
else
  puts "Mese non valido"
end
{% endhighlight %}

---

## NOT — nega il valore della condizione

L'operatore `!` inverte il valore booleano: trasforma `true` in `false` e
viceversa.

| A     | !A    |
|-------|-------|
| true  | false |
| false | true  |

#### Esercizio 5
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
print "Sei maggiorenne? (s/n): "
risposta = gets.chomp

if !( risposta == "n" )
  puts "Accesso consentito"
else
  puts "Accesso negato"
end
{% endhighlight %}

Questo esempio equivale a usare `unless risposta == "n"`. La negazione
esplicita con `!` è utile quando la condizione è già espressa in forma
positiva e non si vuole riscriverla.

---

## Combinare più operatori

Gli operatori logici possono essere combinati tra loro. Quando si mescolano
`&&` e `||` nella stessa espressione, `&&` viene valutato **prima** di `||`.
Per rendere l'intenzione esplicita è buona norma usare le parentesi.

#### Esercizio 6
Copia il seguente codice nell'editor e fallo eseguire. Osserva come le
parentesi cambiano il risultato.

{% highlight ruby %}
a = true
b = false
c = true

puts a || b && c    # true  (b && c prima, poi || a)
puts (a || b) && c  # true  (a || b prima, poi && c)
puts a || (b && c)  # true

b = true
c = false

puts a || b && c    # true  (b && c = false, poi true || false = true)
puts (a || b) && c  # false (a || b = true, poi true && false = false)
{% endhighlight %}

---

## Valutazione a corto circuito

Ruby valuta le condizioni da sinistra a destra e si ferma non appena il
risultato è determinato: questo si chiama **valutazione a corto circuito**.

- Con `&&`: se la prima condizione è `false`, la seconda non viene valutata
  (il risultato è già `false` comunque).
- Con `||`: se la prima condizione è `true`, la seconda non viene valutata
  (il risultato è già `true` comunque).

{% highlight ruby %}
# La divisione per zero non avviene mai:
# se n == 0 è falso, Ruby non valuta il resto
n = 0
if n != 0 && 10 / n > 2
  puts "quoziente maggiore di 2"
else
  puts "n è zero oppure il quoziente è <= 2"
end
{% endhighlight %}

---

## L'operatore ||= (assegnazione condizionale)

`||=` è un idioma molto comune in Ruby: assegna un valore a una variabile
**solo se** questa è attualmente `nil` o `false`.

{% highlight ruby %}
nome = nil
nome ||= "Anonimo"
puts nome   # "Anonimo"

nome ||= "Mario"
puts nome   # "Anonimo"  (non modificato, già valorizzato)
{% endhighlight %}

---

## Esercizi

#### Esercizio 7
Scrivi un programma che letto un numero intero scriva se è positivo,
negativo o nullo.

#### Esercizio 8
Scrivi un programma che letti tre numeri interi visualizzi "tutti uguali"
se sono tutti uguali, "tutti diversi" se sono tutti diversi, altrimenti
"né tutti uguali né tutti diversi".

#### Esercizio 9
Scrivi un programma che letti tre numeri interi scriva "in ordine" se sono
ordinati in senso crescente o decrescente, "non in ordine" altrimenti.
Esempi: 1, 2, 5 → in ordine; 6, 4, 2 → in ordine; 7, 2, 9 → non in ordine.

#### Esercizio 10
Scrivi un programma che legga un anno e scriva se è bisestile oppure no.
Un anno è bisestile se è divisibile per 4, eccetto i multipli di 100 che
non lo sono, a meno che non siano anche multipli di 400.
Esempi: 1996 → bisestile; 1900 → non bisestile; 2000 → bisestile.

#### Esercizio 11
Scrivi un programma che legga una temperatura e l'unità di misura (C o F)
e indichi se l'acqua si trova allo stato solido, liquido o gassoso.
(Acqua: solida sotto 0°C / 32°F, gassosa sopra 100°C / 212°F.)

#### Esercizio 12
Scrivi un programma che letti due istanti di tempo espressi in ore e minuti
determini quale preceda l'altro e li scriva in ordine crescente.
Esempio: ore1=12 min1=30, ore2=7 min2=45 → output: 07:45 → 12:30.

#### Esercizio 13
Scrivi un programma che chieda all'utente il mese e il giorno di nascita
e restituisca il suo segno zodiacale.

#### Esercizio 14
Scrivi un programma che calcoli le imposte IRPEF su un reddito letto in
input, applicando gli scaglioni seguenti:

| Reddito                    | Aliquota |
|----------------------------|----------|
| da 0 a 15.000 €            | 23%      |
| da 15.001 a 28.000 €       | 25%      |
| da 28.001 a 50.000 €       | 35%      |
| oltre 50.000 €             | 43%      |

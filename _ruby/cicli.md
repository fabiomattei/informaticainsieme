---
title: 'Ruby: cicli'
date: '2020-11-15T08:47:01+01:00'
author: Fabio Mattei
layout: page
---

## Il ciclo while

Il ciclo `while` esegue il blocco di codice racchiuso tra `while` e `end` **fintanto che la condizione è vera**. È un ciclo con **controllo in testa**: la condizione viene verificata prima di ogni iterazione, quindi se è subito falsa il blocco non viene mai eseguito.

#### Esercizio 1
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
cont = 0
while cont < 10
  cont = cont + 1
  puts cont
end
{% endhighlight %}

La variabile `cont` si chiama **contatore** perché il suo scopo è contare le iterazioni. Viene incrementata a ogni giro: quando raggiunge 10, la condizione `cont < 10` diventa falsa e il ciclo termina.

#### Esercizio 2
Copia il seguente codice nell'editor e fallo eseguire. Osserva come la variabile `acc` accumula la somma dei numeri da 1 a 10.

{% highlight ruby %}
n = 10
cont = 1
acc = 0
while cont <= n
  acc = acc + cont
  cont = cont + 1
end
puts acc
{% endhighlight %}

La variabile `acc` si chiama **accumulatore** perché accumula il risultato parziale a ogni iterazione. La coppia contatore/accumulatore è uno schema ricorrente nei cicli.

#### Esercizio 3
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
print "Inserisci la password: "
tentativo = gets.chomp
while tentativo != "segreto"
  puts "Password errata, riprova."
  print "Inserisci la password: "
  tentativo = gets.chomp
end
puts "Accesso consentito."
{% endhighlight %}

Questo ciclo continua a richiedere la password finché l'utente non inserisce quella corretta.

## Il ciclo until

`until` è l'equivalente di `while not`: esegue il blocco **finché la condizione è falsa**, terminando non appena diventa vera. È più leggibile quando la condizione è naturalmente negativa.

#### Esercizio 4
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
cont = 0
until cont >= 5
  cont = cont + 1
  puts cont
end
{% endhighlight %}

## Il ciclo do-while (begin … end while)

A differenza di `while`, il costrutto `begin … end while` esegue il corpo **almeno una volta** prima di valutare la condizione. Questa caratteristica è utile quando la prima esecuzione deve avvenire senza condizioni.

#### Esercizio 5
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
begin
  print "Inserisci un numero positivo: "
  n = gets.chomp.to_i
end while n <= 0
puts "Hai inserito: #{n}"
{% endhighlight %}

Il programma chiede almeno una volta un numero, e continua a chiedere finché l'utente non inserisce un valore positivo.

## Il ciclo for

Il ciclo `for` itera su un range o una collezione, assegnando a ogni iterazione il valore corrente a una variabile di ciclo.

### for su un range

#### Esercizio 6
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
for i in 1..5
  puts i
end
{% endhighlight %}

Il range `1..5` include i valori da 1 a 5 compresi. Il range `1...5` invece esclude il 5 (estremo superiore escluso).

#### Esercizio 7
Copia il seguente codice nell'editor e fallo eseguire. Osserva come si calcola la somma dei numeri da 1 a 100.

{% highlight ruby %}
acc = 0
for i in 1..100
  acc = acc + i
end
puts acc
{% endhighlight %}

### for su un array

#### Esercizio 8
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
for i in [1, 3, 5, 7, 9]
  puts i
end
{% endhighlight %}

### for su un hash

#### Esercizio 9
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
for k, v in {nome: "Mario", eta: 25, citta: "Roma"}
  puts "#{k}: #{v}"
end
{% endhighlight %}

## Controllo del flusso: next, break e redo

All'interno di un ciclo si possono usare tre istruzioni speciali per modificare il comportamento dell'iterazione:

| Istruzione | Effetto |
|------------|---------|
| `next`     | Salta all'iterazione successiva |
| `break`    | Esce immediatamente dal ciclo |
| `redo`     | Ripete l'iterazione corrente senza rivalutare la condizione |

#### Esercizio 10
Copia il seguente codice nell'editor e fallo eseguire. Osserva quali numeri vengono stampati.

{% highlight ruby %}
for i in 1..10
  next if i == 3
  break if i == 7
  puts i
end
{% endhighlight %}

Il programma salta il numero 3 e interrompe il ciclo quando raggiunge 7.

## Metodi iteratori di Ruby

Ruby offre metodi specifici sugli oggetti numerici che rendono i cicli più espressivi della sola parola chiave `for`.

#### Esercizio 11
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
5.times do |i|
  puts "iterazione #{i}"
end
{% endhighlight %}

`times` esegue il blocco per un numero fisso di volte. La variabile `i` assume i valori da 0 a 4.

#### Esercizio 12
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
1.upto(5) do |i|
  puts i
end

5.downto(1) do |i|
  puts i
end

1.step(20, 3) do |i|
  puts i
end
{% endhighlight %}

- `upto` conta in su da un valore a un altro
- `downto` conta in giù
- `step` avanza con un passo specificato

---

## Esercizi

#### Esercizio 13
Scrivi un programma che letto un numero intero N calcoli la somma di tutti i numeri da 1 a N.

#### Esercizio 14
Scrivi un programma che letti due numeri interi N e M (con N < M) calcoli la somma di tutti i numeri da N a M (estremi inclusi).

#### Esercizio 15
Scrivi un programma che letto un numero intero N calcoli il fattoriale di N (1 × 2 × 3 × … × N).

#### Esercizio 16
Scrivi un programma che calcoli i primi 20 numeri della sequenza di Fibonacci.

#### Esercizio 17
Scrivi un programma che letti N numeri calcoli il massimo e il minimo tra i valori inseriti.

#### Esercizio 18
Scrivi un programma che letto un numero intero N scriva la tavola pitagorica delle moltiplicazioni di N (da N×1 fino a N×10).

#### Esercizio 19
Scrivi un programma che utilizzi due cicli annidati per scrivere la tabella della moltiplicazione completa (da 1×1 a 10×10).

#### Esercizio 20
Scrivi un programma che letto un numero intero N trovi tutti i suoi divisori.

#### Esercizio 21
Scrivi un programma che letto un numero intero N conti i suoi divisori e scriva quanti sono.

#### Esercizio 22
Scrivi un programma che trovi tutti i numeri primi compresi tra 1 e 100.

#### Esercizio 23
Scrivi un programma che letto un numero positivo N determini il massimo intero K tale che la somma dei primi K interi sia minore o uguale a N.
Ad esempio, se N=20 allora K=5, infatti 1+2+3+4+5=15 mentre 1+2+3+4+5+6=21.

#### Esercizio 24
Scrivi un programma che letti due numeri interi calcoli il massimo comune divisore (MCD) usando l'algoritmo di Euclide.

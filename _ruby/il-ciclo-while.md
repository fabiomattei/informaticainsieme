---
title: 'Ruby: il ciclo while'
date: '2020-11-15T08:47:01+01:00'
author: Fabio Mattei
layout: page
---

## Il ciclo while

Il costrutto `while` permette di eseguire un blocco di istruzioni più volte.
Si chiama **ciclo** perché le istruzioni contenute al suo interno vengono
ripetute ciclicamente.

La sintassi è la seguente:

{% highlight ruby %}
while <condizione>
  <istruzione 1>
  <istruzione 2>
end
{% endhighlight %}

`while` è un ciclo con **controllo in testa**: la condizione viene verificata
**prima** di entrare nel ciclo. Se la condizione è già falsa all'inizio, il
blocco non viene mai eseguito. Le istruzioni interne vengono ripetute finché
la condizione rimane vera; non appena diventa falsa, l'esecuzione prosegue
con la prima istruzione dopo `end`.

#### Esercizio 1
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
cont = 0
while cont < 10
  cont = cont + 1
  puts cont
end
{% endhighlight %}

La variabile `cont` si chiama **contatore** perché il suo scopo è contare
le iterazioni. Possiamo tracciare il suo valore a ogni giro del ciclo:

| Iterazione | cont (prima) | cont (dopo) | Condizione cont < 10 |
|:----------:|:------------:|:-----------:|:--------------------:|
| 1          | 0            | 1           | vera                 |
| 2          | 1            | 2           | vera                 |
| 3          | 2            | 3           | vera                 |
| …          | …            | …           | …                    |
| 10         | 9            | 10          | vera                 |
| —          | 10           | —           | **falsa** → uscita   |

#### Esercizio 2
Copia il seguente codice nell'editor e fallo eseguire. Osserva come la
variabile `acc` accumula la somma dei numeri da 1 a 10.

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

La variabile `acc` si chiama **accumulatore** perché accumula il risultato
parziale a ogni iterazione. La coppia contatore/accumulatore è uno degli
schemi più ricorrenti nella programmazione.

| cont | acc (prima del ciclo) | acc (dopo l'iterazione) |
|:----:|:---------------------:|:-----------------------:|
| 1    | 0                     | 1                       |
| 2    | 1                     | 3                       |
| 3    | 3                     | 6                       |
| 4    | 6                     | 10                      |
| …    | …                     | …                       |
| 10   | 45                    | 55                      |

#### Esercizio 3
Copia il seguente codice nell'editor e fallo eseguire. Questo programma
calcola la sequenza di Fibonacci: ogni numero è la somma dei due precedenti.

{% highlight ruby %}
acc_a = 0
acc_b = 1
cont = 0
while cont < 20
  print "#{acc_a} "
  nuovo = acc_a + acc_b
  acc_a = acc_b
  acc_b = nuovo
  cont = cont + 1
end
puts
{% endhighlight %}

L'algoritmo usa due accumulatori: `acc_a` e `acc_b` si passano i valori
a ogni iterazione per produrre il numero successivo della sequenza.

#### Esercizio 4
Copia il seguente codice nell'editor e fallo eseguire. Il ciclo continua
finché l'utente non indovina il numero segreto.

{% highlight ruby %}
segreto = 42
print "Indovina il numero (1-100): "
tentativo = gets.chomp.to_i
while tentativo != segreto
  if tentativo < segreto
    puts "Troppo basso!"
  else
    puts "Troppo alto!"
  end
  print "Riprova: "
  tentativo = gets.chomp.to_i
end
puts "Hai indovinato!"
{% endhighlight %}

Questo ciclo non ha un contatore esplicito: termina solo quando si
verifica una condizione legata all'input dell'utente.

#### Esercizio 5
Copia il seguente codice nell'editor e fallo eseguire. Osserva come la
variabile `massimo` conserva il valore più grande incontrato finora.

{% highlight ruby %}
print "Quanti numeri vuoi inserire? "
n = gets.chomp.to_i
massimo = nil
cont = 0
while cont < n
  print "Numero: "
  num = gets.chomp.to_i
  if massimo.nil? || num > massimo
    massimo = num
  end
  cont = cont + 1
end
puts "Il massimo è: #{massimo}"
{% endhighlight %}

La variabile `massimo` si chiama **campione**: a ogni iterazione confronta
il nuovo valore con quello migliore trovato finora e lo aggiorna se necessario.

## Il ciclo do-while (begin … end while)

A differenza di `while`, il costrutto `begin … end while` esegue il corpo
**almeno una volta** prima di valutare la condizione. Questa caratteristica
è utile quando la prima esecuzione deve avvenire senza condizioni, tipicamente
per leggere e validare l'input dell'utente.

#### Esercizio 6
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
begin
  print "Inserisci un numero positivo: "
  n = gets.chomp.to_i
end while n <= 0
puts "Hai inserito: #{n}"
{% endhighlight %}

Il programma chiede almeno una volta un numero e continua a chiedere finché
il valore non è positivo. Con un `while` ordinario sarebbe necessario
leggere il valore una volta prima del ciclo e ripetere la lettura dentro,
duplicando il codice.

#### Esercizio 7
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
totale = 0
begin
  print "Inserisci un numero (0 per terminare): "
  n = gets.chomp.to_i
  totale = totale + n
end while n != 0
puts "Totale: #{totale}"
{% endhighlight %}

Il ciclo termina quando l'utente inserisce 0. Poiché il valore sentinella
viene letto all'interno del ciclo, `begin … end while` è la forma più
compatta per questo schema.

---

## Esercizi

#### Esercizio 8
Scrivi un programma che letto un numero intero N calcoli la somma di tutti
i numeri da 1 a N.

#### Esercizio 9
Scrivi un programma che letti due numeri interi N e M (con N < M) calcoli
la somma di tutti i numeri da N a M (estremi inclusi).

#### Esercizio 10
Scrivi un programma che letto un numero intero N calcoli il fattoriale di N
(1 × 2 × 3 × … × N).

#### Esercizio 11
Scrivi un programma che letti N numeri calcoli il massimo e il minimo tra i
valori inseriti.

#### Esercizio 12
Scrivi un programma che letti N numeri calcoli la media aritmetica.

#### Esercizio 13
Scrivi un programma che letto un numero intero positivo N calcoli la somma
di tutte le sue cifre (suggerimento: usa l'operatore `%` per estrarre
l'ultima cifra e `/` per eliminare quella cifra dal numero).

#### Esercizio 14
Scrivi un programma che letti N numeri in ingresso calcoli la somma di tutti
i numeri mostrando le somme parziali ogni 3 numeri inseriti.

#### Esercizio 15
Scrivi un programma che legga valori interi uno alla volta e li sommi, fino
all'immissione del quinto numero pari. Scrivi la somma ottenuta.

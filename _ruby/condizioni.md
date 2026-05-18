---
title: 'Ruby: condizioni'
date: '2020-11-15T08:47:01+01:00'
author: Fabio Mattei
layout: page
---

## Il costrutto if

Il costrutto **if** consente di cambiare la sequenza logica di istruzioni da eseguire in un programma.
Il blocco di codice che segue viene eseguito solo se la condizione risulta vera.
In Ruby il blocco si chiude sempre con la parola chiave **end**.

#### Esercizio 1
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
a = 33
b = 200
if b > a
  puts "b e' maggiore di a"
end
{% endhighlight %}

A differenza di Python, Ruby non usa l'indentazione per delimitare i blocchi: usa le parole chiave
`if` … `end`. L'indentazione rimane però importante per la leggibilità del codice.

### Operatori di confronto

Sono gli operatori che possiamo utilizzare all'interno di una condizione.

| Operatore | Significato          | Esempio   |
|-----------|----------------------|-----------|
| `==`      | uguale               | `a == b`  |
| `!=`      | diverso              | `a != b`  |
| `<`       | minore               | `a < b`   |
| `<=`      | minore o uguale      | `a <= b`  |
| `>`       | maggiore             | `a > b`   |
| `>=`      | maggiore o uguale    | `a >= b`  |

## Il costrutto elsif

`elsif` permette di aggiungere una condizione alternativa quando la prima non è verificata.

#### Esercizio 2
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
a = 33
b = 33
if b > a
  puts "b e' piu' grande di a"
elsif a == b
  puts "a e b sono uguali"
end
{% endhighlight %}

## Il costrutto else

Se nessuna delle condizioni precedenti è verificata, viene eseguito il blocco `else`.

#### Esercizio 3
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
a = 33
b = 33
if b > a
  puts "b e' piu' grande di a"
elsif a == b
  puts "a e b sono uguali"
else
  puts "a e' piu' grande di b"
end
{% endhighlight %}

#### Esercizio 4
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
print "Inserisci un numero: "
n = gets.chomp.to_i
if n > 0
  puts "il numero e' positivo"
elsif n < 0
  puts "il numero e' negativo"
else
  puts "il numero e' zero"
end
{% endhighlight %}

## Il costrutto unless

Ruby è uno dei pochi linguaggi a disporre della parola chiave **unless**, traducibile come *se non*.
`unless condizione` equivale a scrivere `if not condizione` ed è più leggibile quando la condizione
principale è negativa.

#### Esercizio 5
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
print "Sei maggiorenne? (s/n): "
risposta = gets.chomp
unless risposta == 'n'
  puts "Benvenuto!"
else
  puts "Accesso negato."
end
{% endhighlight %}

#### Esercizio 6
Copia il seguente codice nell'editor e fallo eseguire. Osserva come `unless` renda il
codice più naturale da leggere rispetto a un `if` con condizione negata.

{% highlight ruby %}
password = "segreta"
print "Inserisci la password: "
input = gets.chomp
unless input == password
  puts "Password errata!"
else
  puts "Accesso consentito."
end
{% endhighlight %}

## Il costrutto case / when

L'istruzione **case** permette di confrontare un valore con più alternative in modo più compatto
rispetto a una catena di `elsif`. Supporta valori singoli, liste di valori e range.

#### Esercizio 7
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
print "Inserisci un voto da 0 a 100: "
voto = gets.chomp.to_i
case voto
  when 0..5
    puts 'insufficiente'
  when 6
    puts 'sufficiente'
  when 7
    puts 'discreto'
  when 8
    puts 'buono'
  when 9, 10
    puts 'ottimo'
  else
    puts 'voto non valido'
end
{% endhighlight %}

#### Esercizio 8
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
print "In che mese siamo? (1-12): "
mese = gets.chomp.to_i
case mese
  when 3, 4, 5
    puts 'primavera'
  when 6, 7, 8
    puts 'estate'
  when 9, 10, 11
    puts 'autunno'
  when 12, 1, 2
    puts 'inverno'
  else
    puts 'mese non valido'
end
{% endhighlight %}

---

## Esercizi

#### Esercizio 9
Scrivi un programma che legga due stringhe di testo e scriva la prima in ordine alfabetico.

#### Esercizio 10
Scrivi un programma che legga due numeri e scriva il più grande tra i due.

#### Esercizio 11
Scrivi un programma che letto un numero intero determini se è pari o dispari
(suggerimento: usa l'operatore resto `%`).

#### Esercizio 12
Dati 4 numeri in input determinare se la somma dei primi due è minore o uguale
alla somma del terzo e del quarto.

#### Esercizio 13
Un'automobile percorre 20 km con un litro di benzina. Calcolare la spesa necessaria
a percorrere 100 km. Se la spesa è maggiore di €100, applicare uno sconto del 5%.

#### Esercizio 14
Letti i lati di un triangolo determinare se è scaleno, isoscele o equilatero.

#### Esercizio 15
Letti due numeri naturali A e B, sottrarre il più piccolo dal più grande.

#### Esercizio 16
Letti due numeri determinare se sono entrambi compresi tra 100 e 200.

#### Esercizio 17
Letto un numero intero, trovare il suo valore assoluto senza usare metodi built-in.

#### Esercizio 18
Scrivi un programma che legga il valore di una spesa e calcoli lo sconto secondo
la seguente tabella:

| Spesa                | Sconto         |
|----------------------|----------------|
| Al di sotto di 100 € | nessuno sconto |
| Tra 100 e 300 €      | sconto del 10% |
| Tra 300 e 500 €      | sconto del 15% |
| Tra 500 e 800 €      | sconto del 20% |

#### Esercizio 19
Simona deve comprare le paste per il suo bar. Le tariffe applicate dal pasticciere sono:

| Quantità          | Prezzo per pasta |
|-------------------|-----------------|
| Fino a 20         | €1,00           |
| Da 21 a 39        | €0,90           |
| Da 40 a 59        | €0,80           |
| Da 60 a 99        | €0,70           |
| 100 o più         | €0,60           |

Scrivi il programma che calcola l'ammontare che Simona dovrà pagare.

#### Esercizio 20
Tenendo conto degli scaglioni fiscali:

| Reddito           | Aliquota |
|-------------------|----------|
| 0 – 15.000 €      | 23%      |
| 15.001 – 28.000 € | 25%      |
| 28.001 – 50.000 € | 35%      |
| Oltre 50.000 €    | 43%      |

Scrivi un programma che letto un reddito calcoli l'ammontare delle tasse da pagare.

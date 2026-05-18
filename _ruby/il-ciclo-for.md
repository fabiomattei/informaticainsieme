---
title: 'Ruby: il ciclo for'
date: '2020-11-15T08:47:01+01:00'
author: Fabio Mattei
layout: page
---

## Il ciclo for

Il ciclo `for` itera su un range o una collezione, assegnando a ogni
iterazione il valore corrente a una variabile di ciclo. A differenza di
`while`, non richiede di gestire manualmente un contatore: Ruby si occupa
di avanzare automaticamente al valore successivo.

La sintassi è la seguente:

{% highlight ruby %}
for <variabile> in <collezione>
  <istruzione 1>
  <istruzione 2>
end
{% endhighlight %}

## for su un range

Un **range** rappresenta una sequenza continua di valori. In Ruby si scrive
con due punti `..` (estremi inclusi) o tre punti `...` (estremo superiore
escluso).

#### Esercizio 1
Copia il seguente codice nell'editor e fallo eseguire. Osserva la differenza
tra `..` e `...`.

{% highlight ruby %}
for i in 1..5
  puts i
end
{% endhighlight %}

Output: 1, 2, 3, 4, 5 — il 5 è incluso.

{% highlight ruby %}
for i in 1...5
  puts i
end
{% endhighlight %}

Output: 1, 2, 3, 4 — il 5 è escluso.

La variabile `i` assume i valori del range uno alla volta, nell'ordine:

| Iterazione | i |
|:----------:|:-:|
| 1          | 1 |
| 2          | 2 |
| 3          | 3 |
| 4          | 4 |
| 5          | 5 |

#### Esercizio 2
Copia il seguente codice nell'editor e fallo eseguire. Osserva come si
calcola la somma dei numeri da 1 a 100 con una variabile accumulatore.

{% highlight ruby %}
acc = 0
for i in 1..100
  acc = acc + i
end
puts acc
{% endhighlight %}

La variabile `acc` inizia a 0 e a ogni iterazione accumula il valore di `i`.
Al termine del ciclo contiene la somma 1+2+3+…+100 = 5050.

#### Esercizio 3
Copia il seguente codice nell'editor e fallo eseguire. Questo programma
stampa i numeri pari da 2 a 20 usando la condizione all'interno del ciclo.

{% highlight ruby %}
for i in 1..20
  if i % 2 == 0
    puts i
  end
end
{% endhighlight %}

#### Esercizio 4
Copia il seguente codice nell'editor e fallo eseguire. Osserva come si
costruisce e si stampa la tavola pitagorica di un numero.

{% highlight ruby %}
print "Di quale numero vuoi la tavola? "
n = gets.chomp.to_i
for i in 1..10
  puts "#{n} x #{i} = #{n * i}"
end
{% endhighlight %}

## for su un array

Il ciclo `for` può iterare su qualsiasi collezione, non solo su range.
Con un array la variabile di ciclo assume uno alla volta i valori contenuti
nella lista.

#### Esercizio 5
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
for i in [1, 3, 5, 7, 9]
  puts i
end
{% endhighlight %}

| Iterazione | i |
|:----------:|:-:|
| 1          | 1 |
| 2          | 3 |
| 3          | 5 |
| 4          | 7 |
| 5          | 9 |

#### Esercizio 6
Copia il seguente codice nell'editor e fallo eseguire. Questo programma
trova il valore più grande in un array usando la variabile **campione**.

{% highlight ruby %}
numeri = [34, 7, 89, 12, 55, 3, 78]
massimo = numeri[0]
for n in numeri
  if n > massimo
    massimo = n
  end
end
puts "Il massimo è: #{massimo}"
{% endhighlight %}

La variabile `massimo` viene inizializzata al primo elemento e poi
confrontata con ogni elemento successivo. Al termine contiene il valore
più grande dell'intera lista.

## for su un hash

Quando si itera su un hash si possono dichiarare due variabili di ciclo:
una per la chiave e una per il valore.

#### Esercizio 7
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
for k, v in {nome: "Mario", eta: 25, citta: "Roma"}
  puts "#{k}: #{v}"
end
{% endhighlight %}

#### Esercizio 8
Copia il seguente codice nell'editor e fallo eseguire. Questo programma
calcola il totale dei prezzi di un piccolo magazzino.

{% highlight ruby %}
magazzino = {mele: 1.20, pane: 0.80, latte: 1.50, pasta: 0.90}
totale = 0.0
for prodotto, prezzo in magazzino
  puts "#{prodotto}: #{prezzo} €"
  totale = totale + prezzo
end
puts "Totale: #{totale} €"
{% endhighlight %}

## Controllo del flusso: next, break e redo

All'interno di un ciclo si possono usare tre istruzioni speciali per
modificare il comportamento dell'iterazione corrente.

| Istruzione | Effetto                                                  |
|------------|----------------------------------------------------------|
| `next`     | Salta immediatamente all'iterazione successiva           |
| `break`    | Esce immediatamente dal ciclo                            |
| `redo`     | Ripete l'iterazione corrente senza rivalutare la condizione |

#### Esercizio 9
Copia il seguente codice nell'editor e fallo eseguire. Osserva quali numeri
vengono stampati e perché.

{% highlight ruby %}
for i in 1..10
  next if i == 3
  break if i == 7
  puts i
end
{% endhighlight %}

Il programma salta il numero 3 (passa all'iterazione successiva) e
interrompe il ciclo quando `i` vale 7. L'output è: 1, 2, 4, 5, 6.

#### Esercizio 10
Copia il seguente codice nell'editor e fallo eseguire. `next` viene usato
per saltare i numeri dispari e sommare solo i pari.

{% highlight ruby %}
acc = 0
for i in 1..20
  next if i % 2 != 0
  acc = acc + i
end
puts "Somma dei pari da 1 a 20: #{acc}"
{% endhighlight %}

## Cicli annidati

È possibile inserire un ciclo `for` all'interno di un altro. Il ciclo
interno viene eseguito completamente a ogni iterazione del ciclo esterno.

#### Esercizio 11
Copia il seguente codice nell'editor e fallo eseguire. Osserva come i due
cicli producono tutte le combinazioni di riga e colonna.

{% highlight ruby %}
for riga in 1..3
  for col in 1..3
    print "#{riga},#{col}  "
  end
  puts
end
{% endhighlight %}

#### Esercizio 12
Copia il seguente codice nell'editor e fallo eseguire. Questo programma
stampa la tabella della moltiplicazione completa.

{% highlight ruby %}
for i in 1..10
  for j in 1..10
    print "#{(i * j).to_s.rjust(4)}"
  end
  puts
end
{% endhighlight %}

`rjust(4)` allinea ogni numero a destra in un campo di 4 caratteri, così
le colonne risultano allineate.

## Metodi iteratori di Ruby

Ruby offre metodi specifici sugli oggetti numerici che rendono i cicli
ancora più espressivi. Sono lo stile preferito in Ruby idiomatico perché
rendono evidente l'intenzione del codice.

#### Esercizio 13
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
5.times do |i|
  puts "iterazione #{i}"
end
{% endhighlight %}

`times` esegue il blocco per un numero fisso di volte. La variabile `i`
assume i valori da 0 a 4. Quando il contatore non serve si può omettere:

{% highlight ruby %}
3.times do
  puts "ciao"
end
{% endhighlight %}

#### Esercizio 14
Copia il seguente codice nell'editor e fallo eseguire. Osserva le differenze
tra `upto`, `downto` e `step`.

{% highlight ruby %}
puts "upto:"
1.upto(5) do |i|
  puts i
end

puts "downto:"
5.downto(1) do |i|
  puts i
end

puts "step di 3:"
1.step(20, 3) do |i|
  puts i
end
{% endhighlight %}

- `upto` conta in su fino al valore indicato (incluso)
- `downto` conta in giù fino al valore indicato (incluso)
- `step` avanza di un passo specificato: `1.step(20, 3)` produce 1, 4, 7, 10, 13, 16, 19

| Metodo                | Valori prodotti           |
|-----------------------|---------------------------|
| `1.upto(5)`           | 1, 2, 3, 4, 5             |
| `5.downto(1)`         | 5, 4, 3, 2, 1             |
| `1.step(10, 2)`       | 1, 3, 5, 7, 9             |
| `10.step(1, -3)`      | 10, 7, 4, 1               |
| `5.times`             | 0, 1, 2, 3, 4             |

---

## Esercizi

#### Esercizio 15
Scrivi un programma che letto un numero intero N trovi tutti i suoi divisori.

#### Esercizio 16
Scrivi un programma che letto un numero intero N conti i suoi divisori e
scriva quanti sono.

#### Esercizio 17
Scrivi un programma che trovi tutti i numeri primi compresi tra 1 e 100.

#### Esercizio 18
Scrivi un programma che letti N numeri calcoli la somma dei soli numeri pari
inseriti.

#### Esercizio 19
Trova il minor numero di banconote da 100€, 50€, 10€ e 5€ necessarie per
pagare una cifra C multipla di 5, letta in input.

#### Esercizio 20
Scrivi un programma che utilizzi due cicli annidati per scrivere la tabella
dell'addizione completa (da 1+1 a 10+10).

#### Esercizio 21
Scrivi un programma che letto un numero intero N stampi un triangolo rettangolo
di asterischi alto N righe. Con N=4 l'output deve essere:
```
*
**
***
****
```

#### Esercizio 22
Scrivi un programma che letto un numero intero N verifichi se è primo
(un numero è primo se ha esattamente due divisori: 1 e se stesso).

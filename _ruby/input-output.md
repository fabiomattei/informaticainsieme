---
title: 'Ruby: input e output'
date: '2020-11-15T08:47:01+01:00'
author: Fabio Mattei
layout: page
---

## Scrivere sulla console: puts e print

In Ruby esistono tre modi per scrivere un valore sulla console.

| Comando | Comportamento |
|---------|---------------|
| `puts`  | scrive il valore e va a capo automaticamente |
| `print` | scrive il valore senza andare a capo |
| `p`     | scrive il valore nel formato "grezzo" (utile per il debug) |

#### Esercizio 1
Copia il seguente codice nell'editor e fallo eseguire. Osserva la
differenza tra `puts` e `print`.

{% highlight ruby %}
puts "Ciao"
puts "Mondo"

print "Ciao"
print " "
print "Mondo"
puts
{% endhighlight %}

`puts` inserisce un carattere di fine riga dopo ogni valore. `print` no:
le parole restano sulla stessa riga. Una `puts` senza argomenti scrive
solo una riga vuota.

#### Esercizio 2
Copia il seguente codice nell'editor e fallo eseguire. Osserva come `p`
mostri la differenza tra un numero e una stringa.

{% highlight ruby %}
p 42          # 42
p "42"        # "42"  (con le virgolette)
p nil         # nil
p [1, 2, 3]   # [1, 2, 3]
{% endhighlight %}

`p` è utile durante lo sviluppo per capire esattamente che tipo di valore
contiene una variabile.

---

## Scrivere più valori sulla stessa riga

Con `print` si possono concatenare i valori usando `+` (solo tra stringhe)
o l'**interpolazione** `#{}` dentro una stringa con doppi apici.

#### Esercizio 3
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
nome = "Alice"
eta  = 17

puts "Mi chiamo " + nome + " e ho " + eta.to_s + " anni."
puts "Mi chiamo #{nome} e ho #{eta} anni."
{% endhighlight %}

L'interpolazione `#{}` è la forma preferita in Ruby: converte
automaticamente il valore in stringa e il codice risulta più leggibile.
Dentro `#{}` si può mettere qualsiasi espressione Ruby:

{% highlight ruby %}
lato = 5
puts "Area del quadrato: #{lato ** 2}"     # Area del quadrato: 25
puts "Pi greco approssimato: #{22.0 / 7}"  # Pi greco approssimato: 3.142857...
{% endhighlight %}

---

## Leggere da tastiera: gets e chomp

La funzione `gets` legge una riga di testo digitata dall'utente e la
restituisce come **stringa**, incluso il carattere di fine riga `\n`.
Il metodo `.chomp` rimuove quel carattere finale.

**I dati letti con `gets` sono sempre stringhe**: se servono numeri
occorre convertirli con `.to_i` o `.to_f`.

#### Esercizio 4
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
print "Come ti chiami? "
nome = gets.chomp
puts "Ciao, #{nome}!"
{% endhighlight %}

`print` (senza `puts`) viene usato per il messaggio di invito perché
lascia il cursore sulla stessa riga, così l'utente scrive subito dopo
il testo visualizzato.

#### Esercizio 5
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
print "Scrivi il tuo nome: "
nome = gets.chomp
print "Scrivi il tuo cognome: "
cognome = gets.chomp
puts "Nome completo: #{nome} #{cognome}"
puts "Il tuo nome è lungo #{nome.length} caratteri"
{% endhighlight %}

---

## Leggere numeri interi

`gets.chomp` restituisce sempre una stringa. Per ottenere un numero
intero si aggiunge `.to_i`.

#### Esercizio 6
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
print "Scrivi la lunghezza del lato: "
lato = gets.chomp.to_i
area = lato * lato
puts "L'area del quadrato di lato #{lato} vale #{area}"
{% endhighlight %}

`.to_i` converte la stringa in intero. Le due chiamate si possono
scrivere in sequenza sulla stessa riga: `gets.chomp.to_i`.

#### Esercizio 7
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
print "Capitale iniziale: "
capitale = gets.chomp.to_i
print "Tasso annuo (es. 5 per 5%): "
tasso = gets.chomp.to_f / 100
print "Numero di anni: "
anni = gets.chomp.to_i

capitale_finale = capitale * (1 + tasso) ** anni
puts "Dopo #{anni} anni il capitale è #{capitale_finale.round(2)} €"
{% endhighlight %}

Il metodo `.round(2)` arrotonda il risultato a due cifre decimali.

---

## Leggere numeri decimali

Quando il valore atteso è un numero a virgola mobile si usa `.to_f`
al posto di `.to_i`.

#### Esercizio 8
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
print "Raggio del cerchio: "
raggio = gets.chomp.to_f
area = 3.14159 * raggio ** 2
puts "Area: #{area.round(4)}"
{% endhighlight %}

---

## Riepilogo: leggere e convertire l'input

| Istruzione         | Risultato                              |
|--------------------|----------------------------------------|
| `gets`             | stringa con `\n` alla fine             |
| `gets.chomp`       | stringa senza `\n`                     |
| `gets.chomp.to_i`  | numero intero                          |
| `gets.chomp.to_f`  | numero decimale                        |
| `gets.chomp.to_sym`| simbolo                                |

---

## Esercizi

#### Esercizio 9
Scrivi un programma che legga un numero intero e calcoli l'area del
cerchio che ha quel numero come raggio (usa `Math::PI` oppure `3.14159`).

#### Esercizio 10
Scrivi un programma che legga un numero intero `n` e calcoli il valore
di `n + nn + nnn`.
Esempio: se n=3, calcola 3 + 33 + 333 = 369.

#### Esercizio 11
Scrivi un programma che legga un tempo espresso in ore e calcoli di
quanti minuti e di quanti secondi è composto.

#### Esercizio 12
Scrivi un programma che legga due numeri a virgola mobile e scriva il
risultato di tutte le operazioni aritmetiche: somma, differenza, prodotto,
quoziente, quoziente intero, resto, potenza.

#### Esercizio 13
Scrivi un programma che legga la distanza da percorrere (in km), i km
percorsi per litro e il prezzo del carburante al litro, e calcoli la
quantità di litri necessari e il costo totale.

#### Esercizio 14
Scrivi un programma che legga un valore imponibile e calcoli l'IVA al
22%, stampando imponibile, IVA e totale.

#### Esercizio 15
Scrivi un programma che legga un valore in euro e lo converta in
dollari (usa il tasso di cambio 1 € = 1.08 $).

#### Esercizio 16
Scrivi un programma che legga una stringa di testo e un numero intero N,
e scriva la stringa N volte sulla stessa riga.

#### Esercizio 17
Scrivi un programma che legga il raggio di una sfera e ne calcoli il
volume. (Formula: V = 4/3 × π × r³)

#### Esercizio 18
Scrivi un programma che legga tre numeri e verifichi se costituiscono
una terna pitagorica (a² + b² = c², con c il numero più grande).

#### Esercizio 19
Scrivi un programma che legga tre numeri rappresentanti i lati di un
triangolo e scriva se è scaleno, isoscele o equilatero.

#### Esercizio 20
Scrivi un programma che legga due numeri interi n e m e scriva il più
grande tra i due.

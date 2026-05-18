---
title: 'Ruby: funzioni'
date: '2020-11-15T08:47:01+01:00'
author: Fabio Mattei
layout: page
---

## Funzioni

In Ruby una funzione si definisce con la parola chiave `def` e si chiude con `end`.
Le funzioni raggruppano una sequenza di istruzioni che può essere richiamata più volte
dal resto del programma. Questo evita di copiare e incollare lo stesso codice
in più punti e rende il programma più leggibile e facile da modificare.

#### Esercizio 1
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
def saluta
  puts 'Ciao!'
  puts 'Ciao!!!'
  puts 'Ciao a te!'
end

puts 'Prima della chiamata'
saluta
saluta
puts 'Dopo le chiamate'
{% endhighlight %}

La parola chiave `def` dice a Ruby che stiamo definendo una nuova funzione.
Il nome `saluta` è quello con cui la funzione verrà invocata. Il blocco di
istruzioni tra `def` e `end` viene eseguito ogni volta che scriviamo `saluta`.

## Parametri e argomenti

Una funzione può ricevere valori dall'esterno attraverso i **parametri**.
Quando la funzione viene chiamata, i valori passati si chiamano **argomenti**
e vengono assegnati ai parametri corrispondenti.

#### Esercizio 2
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
def saluta(nome)
  puts 'Ciao ' + nome
end

saluta('Alice')
saluta('Bob')
{% endhighlight %}

La variabile `nome` è un parametro locale alla funzione: nasce quando la
funzione viene chiamata e viene distrutta quando termina.

### Parentesi opzionali

Le parentesi attorno ai parametri non sono obbligatorie in Ruby. Entrambe le
forme seguenti sono valide.

{% highlight ruby %}
def somma(a, b)
  a + b
end

def moltiplica a, b
  a * b
end
{% endhighlight %}

## Valori di ritorno

Una funzione può restituire un valore al chiamante con l'istruzione `return`.
Il valore restituito può essere memorizzato in una variabile o usato direttamente
in un'espressione.

#### Esercizio 3
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
def raddoppia(numero)
  return numero * 2
end

risultato = raddoppia(5)
puts risultato
puts raddoppia(10)
{% endhighlight %}

### Return implicito

In Ruby non è necessario scrivere `return` esplicitamente. Se non viene specificato,
Ruby restituisce automaticamente il risultato dell'ultima espressione valutata.

#### Esercizio 4
Copia il seguente codice nell'editor e fallo eseguire. Il risultato è identico
all'esercizio precedente.

{% highlight ruby %}
def raddoppia(numero)
  numero * 2
end

puts raddoppia(5)
{% endhighlight %}

#### Esercizio 5
Copia il seguente codice nell'editor e fallo eseguire. Osserva come `return`
esca dalla funzione immediatamente: le istruzioni successive non vengono eseguite.

{% highlight ruby %}
def pari_o_dispari(numero)
  if numero % 2 == 0
    return 'pari'
  end
  'dispari'
end

puts pari_o_dispari(4)
puts pari_o_dispari(7)
{% endhighlight %}

## Parametri facoltativi

Un parametro con valore di default diventa **facoltativo**: se il chiamante
non lo passa, viene usato il valore di default. I parametri facoltativi devono
sempre seguire quelli obbligatori.

#### Esercizio 6
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
def moltiplica(a, b, c = 2)
  a * b * c
end

puts moltiplica(3, 4)    # c vale 2 per default
puts moltiplica(3, 4, 5) # c vale 5
{% endhighlight %}

## Parametri variabili: splat e double splat

Ruby permette di raccogliere un numero variabile di argomenti in un array
con `*args`, o in un hash con `**kwargs`.

#### Esercizio 7
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
def somma_tutto(*numeri)
  acc = 0
  for n in numeri
    acc = acc + n
  end
  acc
end

puts somma_tutto(1, 2, 3)
puts somma_tutto(10, 20, 30, 40)
{% endhighlight %}

`*numeri` raccoglie tutti gli argomenti in eccesso come array. La funzione
può quindi essere chiamata con qualsiasi numero di argomenti.

#### Esercizio 8
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
def descrivi(nome, **info)
  puts "Nome: #{nome}"
  for k, v in info
    puts "  #{k}: #{v}"
  end
end

descrivi('Mario', eta: 25, citta: 'Roma')
{% endhighlight %}

`**info` raccoglie gli argomenti con nome (in forma `chiave: valore`) in un hash.

## Ambito delle variabili (scope)

Le variabili definite all'interno di una funzione sono **locali**: nascono quando
la funzione viene chiamata e vengono distrutte quando termina. Il programma
principale non può accedervi dopo la chiamata.

#### Esercizio 9
Copia il seguente codice nell'editor e prova a eseguirlo. Osserva il messaggio
di errore.

{% highlight ruby %}
def pollaio
  uova = 32765
end

pollaio
puts uova  # NameError: undefined local variable 'uova'
{% endhighlight %}

La variabile `uova` esiste solo all'interno della funzione. Una volta terminata
la funzione, non è più accessibile.

Le variabili globali (prefisso `$`) sono invece accessibili ovunque, ma vanno
usate con parsimonia perché rendono il codice difficile da seguire.

{% highlight ruby %}
$contatore = 0

def incrementa
  $contatore = $contatore + 1
end

incrementa
incrementa
puts $contatore  # 2
{% endhighlight %}

---

## Esercizi

#### Esercizio 10
Scrivi una funzione `calcola_maggiore` che prenda due numeri come parametri e
restituisca il più grande tra i due.

#### Esercizio 11
Scrivi una funzione `calcola_maggiore_tre` che prenda tre numeri come parametri
e restituisca il più grande tra i tre.

#### Esercizio 12
Scrivi una funzione `fattoriale` che accetti un numero intero e restituisca il
suo fattoriale (1 × 2 × 3 × … × N).

#### Esercizio 13
Scrivi una funzione `fibonacci` che accetti un numero intero N e restituisca
l'N-esimo numero della sequenza di Fibonacci.

#### Esercizio 14
Scrivi una funzione `primo?` che accetti un numero intero e restituisca `true`
se è primo e `false` altrimenti.

#### Esercizio 15
Scrivi una funzione `mcd` che calcoli il massimo comune divisore tra due numeri
usando l'algoritmo di Euclide.

#### Esercizio 16
Scrivi una funzione `mcm` che calcoli il minimo comune multiplo tra due numeri.

#### Esercizio 17
Scrivi una funzione `celsius_to_fahrenheit` che converta una temperatura da
gradi Celsius a Fahrenheit (formula: `Tf = Tc * 9.0 / 5 + 32`).
Scrivi poi la funzione inversa `fahrenheit_to_celsius`.
Nel programma principale stampa la tabella di conversione per i gradi Celsius
da -20 a 100 a passo di 5.

#### Esercizio 18
Scrivi una funzione `somma_numeri` che prenda due numeri interi `a` e `b` e
restituisca la somma di tutti i numeri compresi tra `a` e `b` (estremi inclusi).

#### Esercizio 19
Scrivi una funzione `somma_pari` che prenda due numeri interi `a` e `b` e
restituisca la somma di tutti i numeri pari compresi tra `a` e `b`.

#### Esercizio 20
Scrivi una funzione `esprimi_giudizio` che riceva il voto di uno studente
(intero da 1 a 10) e restituisca un giudizio secondo la seguente tabella:

| Voto     | Giudizio                |
|----------|-------------------------|
| 1, 2, 3, 4 | Gravemente insufficiente |
| 5        | Insufficiente           |
| 6        | Sufficiente             |
| 7        | Discreto                |
| 8        | Buono                   |
| 9        | Distinto                |
| 10       | Ottimo                  |

#### Esercizio 21
Scrivi una funzione `calcola_stipendio` che prenda come parametri il numero di
ore lavorate e la paga oraria. Fino a 40 ore lo stipendio è `ore * paga`; per
le ore di straordinario si applica la paga maggiorata del 50%.

Esempi:
- `calcola_stipendio(20, 20)` → 400
- `calcola_stipendio(40, 20)` → 800
- `calcola_stipendio(50, 20)` → 1100

#### Esercizio 22
Scrivi una funzione `perfetto?` che riceva un numero intero e restituisca `true`
se è un numero perfetto. Un numero perfetto è uguale alla somma dei propri
divisori positivi escluso se stesso (es: 6 = 1+2+3, 28 = 1+2+4+7+14).

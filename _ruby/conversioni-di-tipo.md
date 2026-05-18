---
title: 'Ruby: conversioni di tipo'
date: '2020-11-15T08:47:01+01:00'
author: Fabio Mattei
layout: page
---

## Perché convertire i tipi

In Ruby ogni valore ha un tipo. A volte è necessario trasformare un valore
da un tipo a un altro. Il caso più comune è la lettura da tastiera: `gets`
restituisce **sempre una stringa**, anche se l'utente ha digitato un numero.
Per poter fare calcoli è necessario convertirla esplicitamente.

{% highlight ruby %}
testo = gets.chomp
puts testo.class   # String
numero = testo.to_i
puts numero.class  # Integer
{% endhighlight %}

In Ruby le conversioni non sono funzioni autonome ma **metodi** chiamati
direttamente sul valore o sulla variabile. Per conoscere il tipo di una
variabile si usa il metodo `.class`.

---

## .to_i — da stringa o float a intero

`.to_i` converte il valore in un numero intero. Se il valore è un `Float`,
la parte decimale viene **troncata** (non arrotondata). Se il valore è una
stringa, Ruby legge i cifre iniziali e ignora il resto; se non ci sono cifre
restituisce 0 senza generare errori.

#### Esercizio 1
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
puts 3.9.to_i      # 3  (troncato, non arrotondato)
puts 3.1.to_i      # 3
puts "42".to_i     # 42
puts "-7".to_i     # -7
puts "3.14".to_i   # 3  (si ferma al punto)
puts "abc".to_i    # 0  (nessuna cifra iniziale)
puts "5abc".to_i   # 5  (legge le cifre iniziali)
{% endhighlight %}

`.to_i` accetta anche una base come argomento per convertire stringhe che
rappresentano numeri in basi diverse da 10.

{% highlight ruby %}
puts "1010".to_i(2)   # 10  (binario)
puts "FF".to_i(16)    # 255 (esadecimale)
puts "17".to_i(8)     # 15  (ottale)
{% endhighlight %}

### Conversione stretta con Integer()

Se si vuole che la conversione **fallisca con un errore** invece di
restituire 0 per stringhe non valide, si usa la funzione `Integer()`.

{% highlight ruby %}
puts Integer("42")    # 42
puts Integer("abc")   # ArgumentError: invalid value for Integer()
{% endhighlight %}

---

## .to_f — da stringa o intero a numero decimale

`.to_f` converte il valore in un numero a virgola mobile.

#### Esercizio 2
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
puts 5.to_f        # 5.0
puts "3.14".to_f   # 3.14
puts "2e3".to_f    # 2000.0  (notazione scientifica)
puts "abc".to_f    # 0.0
{% endhighlight %}

La conversione da intero a float è utile per ottenere la divisione decimale,
dato che in Ruby la divisione tra due interi produce un intero.

{% highlight ruby %}
puts 7 / 2          # 3   (divisione intera)
puts 7.to_f / 2     # 3.5 (divisione decimale)
puts 7 / 2.to_f     # 3.5
{% endhighlight %}

---

## .to_s — da numero a stringa

`.to_s` converte il valore nella sua rappresentazione testuale.
È necessario quando si vuole concatenare un numero a una stringa con `+`.

#### Esercizio 3
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
eta = 25
puts "Ho " + eta.to_s + " anni"   # Ho 25 anni
puts "Pi greco: " + 3.14.to_s     # Pi greco: 3.14
{% endhighlight %}

In alternativa si può usare l'**interpolazione** con `#{}`, che chiama
automaticamente `.to_s` sul valore:

{% highlight ruby %}
eta = 25
puts "Ho #{eta} anni"     # Ho 25 anni  (più idiomatico)
{% endhighlight %}

`.to_s` accetta anche una base come argomento per convertire un intero nella
sua rappresentazione in un'altra base.

{% highlight ruby %}
puts 255.to_s(16)   # "ff"  (esadecimale)
puts 10.to_s(2)     # "1010" (binario)
puts 15.to_s(8)     # "17"  (ottale)
{% endhighlight %}

---

## .to_sym — da stringa a simbolo

`.to_sym` converte una stringa in un simbolo. Il metodo inverso è `.to_s`.

{% highlight ruby %}
puts "nome".to_sym.class    # Symbol
puts "nome".to_sym          # nome
puts :nome.to_s             # "nome"
{% endhighlight %}

---

## .chr e .ord — caratteri e codici ASCII

Ogni carattere è associato a un numero intero nella tabella **ASCII**
(e più in generale Unicode). `.chr` chiamato su un intero restituisce
il carattere corrispondente; `.ord` chiamato su una stringa restituisce
il codice del primo carattere.

#### Esercizio 4
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight ruby %}
puts 65.chr    # A
puts 97.chr    # a
puts 48.chr    # 0

puts 'A'.ord   # 65
puts 'a'.ord   # 97
puts '0'.ord   # 48
{% endhighlight %}

Le lettere maiuscole vanno da 65 (`A`) a 90 (`Z`), le minuscole da 97 (`a`)
a 122 (`z`). La differenza tra maiuscola e minuscola è sempre 32.

{% highlight ruby %}
puts 'a'.ord - 'A'.ord        # 32
puts ('A'.ord + 32).chr       # a  (da maiuscola a minuscola)
puts ('a'.ord - 32).chr       # A  (da minuscola a maiuscola)
{% endhighlight %}

---

## Valori truthy e falsy

In Ruby le regole di truthiness sono **più semplici** che in Python:
sono **falsy** solo `nil` e `false`. Tutto il resto — inclusi `0`,
la stringa vuota `""` e l'array vuoto `[]` — è **truthy**.

#### Esercizio 5
Copia il seguente codice nell'editor e fallo eseguire. Osserva la differenza
rispetto a Python.

{% highlight ruby %}
[nil, false, 0, "", [], 0.0].each do |val|
  if val
    puts "#{val.inspect} è truthy"
  else
    puts "#{val.inspect} è falsy"
  end
end
{% endhighlight %}

Output atteso:
```
nil è falsy
false è falsy
0 è truthy
"" è truthy
[] è truthy
0.0 è truthy
```

---

## Tabella riepilogativa

| Metodo / Funzione | Converte a | Note                                          |
|-------------------|------------|-----------------------------------------------|
| `.to_i`           | `Integer`  | Tronca i decimali; legge le cifre iniziali    |
| `.to_i(base)`     | `Integer`  | Converte stringa in base 2, 8, 16…            |
| `Integer(x)`      | `Integer`  | Conversione stretta: errore se non valido     |
| `.to_f`           | `Float`    | Accetta notazione scientifica                 |
| `Float(x)`        | `Float`    | Conversione stretta: errore se non valido     |
| `.to_s`           | `String`   | Interpolazione `#{}` chiama `.to_s` in automatico |
| `.to_s(base)`     | `String`   | Intero → stringa in base 2, 8, 16…            |
| `.to_sym`         | `Symbol`   | Inverso di `:sym.to_s`                        |
| `.chr`            | `String`   | Codice ASCII/Unicode → carattere              |
| `.ord`            | `Integer`  | Carattere → codice ASCII/Unicode              |

---

## Esercizi

#### Esercizio 6
Scrivi un programma che legga due numeri interi dall'utente e stampi la loro
somma, differenza, prodotto e quoziente (con decimali).

#### Esercizio 7
Scrivi un programma che legga un numero decimale dall'utente e stampi
separatamente la parte intera e la parte decimale.

#### Esercizio 8
Scrivi un programma che legga dall'utente un numero in binario (come stringa)
e stampi il suo valore in base 10.

#### Esercizio 9
Scrivi un programma che legga un carattere dall'utente e stampi il suo codice
ASCII. Se il carattere è una lettera minuscola, stampa anche la corrispondente
lettera maiuscola senza usare `.upcase` (usa `.ord` e `.chr`).

#### Esercizio 10
Scrivi un programma che stampi la tabella ASCII dei caratteri stampabili,
ovvero i caratteri con codice da 32 a 126, nel formato:
```
32  (spazio)
33  !
34  "
...
```

#### Esercizio 11
Scrivi un programma che legga il nome dell'utente e la sua età come stringa,
poi costruisca e stampi la frase:
`"Tra 10 anni, Nome avrà X anni."`
dove X è l'età aumentata di 10. Usa l'interpolazione.

#### Esercizio 12
Scrivi un programma che legga tre voti (numeri decimali) e stampi la media
con due cifre decimali e il valore intero della media (troncato).

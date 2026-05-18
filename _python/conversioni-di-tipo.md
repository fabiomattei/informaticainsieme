---
id: 238
title: 'Conversioni di tipo'
date: '2020-02-08T06:03:58+01:00'
author: Fabio Mattei
layout: page
---

## Perché convertire i tipi

In Python ogni valore ha un tipo: `int`, `float`, `str`, `bool` e così via.
A volte è necessario trasformare un valore da un tipo a un altro. Il caso
più comune è la lettura da tastiera: la funzione `input()` restituisce
**sempre una stringa**, anche se l'utente ha digitato un numero. Per poter
fare calcoli è necessario convertirla esplicitamente.

{% highlight python %}
testo = input("Inserisci un numero: ")
print(type(testo))  # <class 'str'>
numero = int(testo)
print(type(numero)) # <class 'int'>
{% endhighlight %}

La funzione `type()` restituisce il tipo di una variabile ed è utile per
fare debug.

---

## int() — da stringa o float a intero

`int(x)` converte `x` in un numero intero. Se `x` è un `float`, la parte
decimale viene **troncata** (non arrotondata). Se `x` è una stringa deve
contenere un numero intero valido.

#### Esercizio 1
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight python %}
print(int(3.9))     # 3  (troncato, non arrotondato)
print(int(3.1))     # 3
print(int("42"))    # 42
print(int("-7"))    # -7
{% endhighlight %}

`int()` accetta anche una base come secondo argomento per convertire stringhe
che rappresentano numeri in basi diverse da 10.

{% highlight python %}
print(int("1010", 2))   # 10  (binario)
print(int("FF", 16))    # 255 (esadecimale)
print(int("17", 8))     # 15  (ottale)
{% endhighlight %}

Attenzione: `int("3.14")` genera un errore. Per convertire una stringa con
decimali occorre passare prima per `float()`.

{% highlight python %}
print(int(float("3.14")))  # 3
{% endhighlight %}

---

## float() — da stringa o intero a numero decimale

`float(x)` converte `x` in un numero a virgola mobile.

#### Esercizio 2
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight python %}
print(float(5))       # 5.0
print(float("3.14"))  # 3.14
print(float("2e3"))   # 2000.0  (notazione scientifica)
{% endhighlight %}

La conversione da `int` a `float` è utile per evitare la divisione intera:

{% highlight python %}
a = 7
b = 2
print(a / b)          # 3.5  (Python 3 divide sempre con float)
print(a // b)         # 3    (quoziente intero)
print(float(a) / b)   # 3.5
{% endhighlight %}

---

## str() — da numero a stringa

`str(x)` converte `x` nella sua rappresentazione testuale.
È necessaria quando si vuole concatenare un numero a una stringa con `+`.

#### Esercizio 3
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight python %}
eta = 25
print("Ho " + str(eta) + " anni")  # Ho 25 anni
print("Pi greco: " + str(3.14))    # Pi greco: 3.14
{% endhighlight %}

Senza `str()` la concatenazione con `+` genera un `TypeError`:

{% highlight python %}
# print("Ho " + 25 + " anni")  # TypeError!
{% endhighlight %}

---

## bool() — da qualsiasi valore a booleano

`bool(x)` converte `x` in `True` o `False` secondo le regole di **truthiness**
di Python: quasi tutto è `True`, tranne i valori considerati "vuoti" o "nulli".

#### Esercizio 4
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight python %}
print(bool(0))       # False
print(bool(0.0))     # False
print(bool(""))      # False  (stringa vuota)
print(bool([]))      # False  (lista vuota)
print(bool(None))    # False

print(bool(1))       # True
print(bool(-5))      # True
print(bool("ciao"))  # True
print(bool([0]))     # True  (lista con un elemento)
{% endhighlight %}

---

## chr() e ord() — caratteri e codici ASCII

Ogni carattere è associato a un numero intero nella tabella **ASCII** (e più
in generale Unicode). `chr(n)` restituisce il carattere corrispondente al
codice `n`; `ord(c)` fa il contrario.

#### Esercizio 5
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight python %}
print(chr(65))   # A
print(chr(97))   # a
print(chr(48))   # 0

print(ord('A'))  # 65
print(ord('a'))  # 97
print(ord('0'))  # 48
{% endhighlight %}

Le lettere maiuscole vanno da 65 (`A`) a 90 (`Z`), le minuscole da 97 (`a`)
a 122 (`z`). La differenza tra maiuscola e minuscola è sempre 32.

{% highlight python %}
print(ord('a') - ord('A'))   # 32
print(chr(ord('A') + 32))    # a  (da maiuscola a minuscola)
{% endhighlight %}

---

## Tabella riepilogativa

| Funzione         | Converte a | Note                                      |
|------------------|------------|-------------------------------------------|
| `int(x)`         | `int`      | Tronca i decimali; accetta una base       |
| `int(x, base)`   | `int`      | Converte stringa in base 2, 8, 16…        |
| `float(x)`       | `float`    | Accetta notazione scientifica             |
| `str(x)`         | `str`      | Necessaria per concatenare numeri         |
| `bool(x)`        | `bool`     | False per 0, "", [], None; True altrimenti|
| `chr(n)`         | `str`      | Codice ASCII/Unicode → carattere          |
| `ord(c)`         | `int`      | Carattere → codice ASCII/Unicode          |

---

## Esercizi

#### Esercizio 6
Scrivi un programma che legga due numeri interi dall'utente e stampi la loro
somma, differenza, prodotto e quoziente (con decimali).

#### Esercizio 7
Scrivi un programma che legga un numero decimale dall'utente e stampi
separatamente la parte intera e la parte decimale.
(Suggerimento: la parte decimale si ottiene sottraendo la parte intera.)

#### Esercizio 8
Scrivi un programma che legga dall'utente un numero in binario (come stringa)
e stampi il suo valore in base 10.

#### Esercizio 9
Scrivi un programma che legga un carattere dall'utente e stampi il suo codice
ASCII. Se il carattere è una lettera minuscola, stampa anche la corrispondente
lettera maiuscola senza usare `.upper()` (usa `chr` e `ord`).

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
dove X è l'età aumentata di 10.

#### Esercizio 12
Scrivi un programma che legga tre voti (numeri decimali) e stampi la media
arrotondata a due cifre decimali e il valore intero della media (troncato).

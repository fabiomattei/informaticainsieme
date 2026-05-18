---
title: 'C++: conversioni di tipo'
date: '2020-11-15T08:47:01+01:00'
author: Fabio Mattei
layout: page
---

## Conversioni implicite ed esplicite

In C++ è possibile passare da un tipo a un altro in due modi:

- **Implicita** — il compilatore la esegue automaticamente quando non c'è
  perdita di informazione (es. da `int` a `double`).
- **Esplicita** — il programmatore la richiede con `static_cast` quando
  potrebbe esserci perdita di informazione (es. da `double` a `int`).

---

## Conversione implicita (widening)

#### Esercizio 1
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
using namespace std;

int main() {
    int    interi = 7;
    double decimale = interi;  // conversione implicita int -> double

    cout << decimale << endl;  // 7

    double pi = 3.14;
    int    tronco = pi;        // conversione implicita double -> int
    cout << tronco << endl;    // 3  (la parte decimale viene scartata)

    return 0;
}
{% endhighlight %}

La conversione da `double` a `int` tronca la parte decimale **senza
arrotondare**. Il compilatore può mostrare un avviso (*warning*).

---

## static_cast — conversione esplicita

#### Esercizio 2
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
using namespace std;

int main() {
    int a = 7, b = 2;

    // divisione intera: il risultato è 3, non 3.5
    cout << a / b << endl;

    // per ottenere 3.5 si converte uno dei due operandi
    double risultato = static_cast<double>(a) / b;
    cout << risultato << endl;  // 3.5

    return 0;
}
{% endhighlight %}

`static_cast<tipo>(valore)` è la forma moderna e preferita in C++.
Il vecchio stile C `(double)a` funziona ancora ma è sconsigliato.

---

## Da stringa a numero: stoi, stof, stod

Per convertire una stringa che rappresenta un numero si usano le funzioni
della libreria `<string>`:

| Funzione   | Converte in | Esempio                    |
|------------|-------------|----------------------------|
| `stoi(s)`  | `int`       | `stoi("42")` → `42`        |
| `stol(s)`  | `long`      | `stol("100000")` → `100000`|
| `stof(s)`  | `float`     | `stof("3.14")` → `3.14f`   |
| `stod(s)`  | `double`    | `stod("3.14")` → `3.14`    |

#### Esercizio 3
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
#include <string>
using namespace std;

int main() {
    string s1 = "42";
    string s2 = "3.14";

    int    n = stoi(s1);
    double d = stod(s2);

    cout << n + 1  << endl;  // 43
    cout << d * 2  << endl;  // 6.28

    return 0;
}
{% endhighlight %}

Se la stringa non rappresenta un numero valido, queste funzioni lanciano
un'eccezione `std::invalid_argument`.

---

## Da numero a stringa: to_string

#### Esercizio 4
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
#include <string>
using namespace std;

int main() {
    int    punteggio = 100;
    double media     = 7.5;

    string messaggio = "Punteggio: " + to_string(punteggio);
    cout << messaggio << endl;

    string risultato = "Media: " + to_string(media);
    cout << risultato << endl;

    return 0;
}
{% endhighlight %}

`to_string` converte qualsiasi tipo numerico in una `string`.

---

## Convertire char ↔ int

#### Esercizio 5
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
using namespace std;

int main() {
    char c = 'A';
    int  codice = static_cast<int>(c);
    cout << codice << endl;   // 65

    int  num = 97;
    char lettera = static_cast<char>(num);
    cout << lettera << endl;  // a

    return 0;
}
{% endhighlight %}

---

## Esercizi

#### Esercizio 6
Leggi due interi con `cin` e stampa la loro divisione come numero decimale
(non intera). Usa `static_cast` per forzare la divisione reale.

#### Esercizio 7
Leggi una stringa dall'utente che rappresenta un prezzo (es. "12.50"),
convertila in `double` con `stod` e stampa il prezzo con IVA al 22%.

#### Esercizio 8
Leggi un numero intero con `cin`. Concatenalo a una stringa di testo
usando `to_string` e stampa il risultato.

#### Esercizio 9
Scrivi un programma che converta gradi Celsius (interi, letti con `cin`)
in Fahrenheit (double). Formula: `F = C * 9.0 / 5 + 32`.
Spiega perché è necessario scrivere `9.0` e non `9`.

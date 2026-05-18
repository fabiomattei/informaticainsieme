---
title: 'C++: gli operatori'
date: '2020-11-15T08:47:01+01:00'
author: Fabio Mattei
layout: page
---

## Operatori aritmetici

Gli operatori aritmetici di C++ funzionano come in matematica, con una
differenza importante: la **divisione tra interi** produce un intero
(la parte decimale viene scartata).

| Operatore | Significato         | Esempio        | Risultato |
|:---------:|---------------------|----------------|----------:|
| `+`       | addizione           | `7 + 3`        | `10`      |
| `-`       | sottrazione         | `7 - 3`        | `4`       |
| `*`       | moltiplicazione     | `7 * 3`        | `21`      |
| `/`       | divisione           | `7 / 3`        | `2`       |
| `%`       | modulo (resto)      | `7 % 3`        | `1`       |

#### Esercizio 1
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
using namespace std;

int main() {
    int a = 17, b = 5;

    cout << a + b  << endl;   // 22
    cout << a - b  << endl;   // 12
    cout << a * b  << endl;   // 85
    cout << a / b  << endl;   // 3  (divisione intera!)
    cout << a % b  << endl;   // 2  (resto della divisione)

    double c = 17.0;
    cout << c / b  << endl;   // 3.4 (divisione reale)

    return 0;
}
{% endhighlight %}

---

## Operatori di confronto

Restituiscono `true` (1) o `false` (0).

| Operatore | Significato        | Esempio    | Risultato |
|:---------:|--------------------|------------|-----------|
| `==`      | uguale a           | `5 == 5`   | `true`    |
| `!=`      | diverso da         | `5 != 3`   | `true`    |
| `<`       | minore di          | `3 < 5`    | `true`    |
| `>`       | maggiore di        | `3 > 5`    | `false`   |
| `<=`      | minore o uguale    | `5 <= 5`   | `true`    |
| `>=`      | maggiore o uguale  | `3 >= 5`   | `false`   |

#### Esercizio 2
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
using namespace std;

int main() {
    int x = 8;

    cout << (x > 5)  << endl;  // 1 (true)
    cout << (x == 8) << endl;  // 1 (true)
    cout << (x != 8) << endl;  // 0 (false)
    cout << (x < 8)  << endl;  // 0 (false)

    return 0;
}
{% endhighlight %}

---

## Operatori di assegnazione composta

| Operatore | Equivalente a   | Esempio       |
|:---------:|-----------------|---------------|
| `+=`      | `a = a + b`     | `a += 3`      |
| `-=`      | `a = a - b`     | `a -= 3`      |
| `*=`      | `a = a * b`     | `a *= 3`      |
| `/=`      | `a = a / b`     | `a /= 3`      |
| `%=`      | `a = a % b`     | `a %= 3`      |

#### Esercizio 3
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
using namespace std;

int main() {
    int n = 20;
    n += 5;   cout << n << endl;  // 25
    n -= 3;   cout << n << endl;  // 22
    n *= 2;   cout << n << endl;  // 44
    n /= 4;   cout << n << endl;  // 11
    n %= 3;   cout << n << endl;  // 2

    return 0;
}
{% endhighlight %}

---

## Incremento e decremento

`++` e `--` incrementano o decrementano di 1. La forma **prefissa** (`++i`)
modifica la variabile *prima* di usarla nell'espressione; la forma
**postfissa** (`i++`) la modifica *dopo*.

#### Esercizio 4
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight cpp %}
#include <iostream>
using namespace std;

int main() {
    int i = 10;

    cout << i++ << endl;  // stampa 10, poi i diventa 11
    cout << i   << endl;  // 11

    cout << ++i << endl;  // i diventa 12, poi stampa 12
    cout << i   << endl;  // 12

    return 0;
}
{% endhighlight %}

---

## Precedenza degli operatori

Gli operatori hanno una priorità (precedenza). Quelli con precedenza più
alta vengono valutati prima. Usare le parentesi quando il dubbio c'è.

| Priorità | Operatori             |
|---------:|-----------------------|
| alta     | `++` `--` (prefisso)  |
|          | `*` `/` `%`           |
|          | `+` `-`               |
|          | `<` `>` `<=` `>=`     |
|          | `==` `!=`             |
| bassa    | `=` `+=` `-=` ...     |

---

## Esercizi

#### Esercizio 5
Dati `a = 15` e `b = 4`, calcola e stampa: somma, differenza, prodotto,
quoziente intero e resto.

#### Esercizio 6
Leggi un numero intero e determina se è pari o dispari usando l'operatore `%`.

#### Esercizio 7
Leggi tre lati di un triangolo e stampa `true` se il triangolo è equilatero
(tutti i lati uguali) o isoscele (almeno due lati uguali).

#### Esercizio 8
Calcola `2 + 3 * 4` e poi `(2 + 3) * 4`. Stampa entrambi i risultati e
spiega la differenza.

#### Esercizio 9
Leggi un numero di secondi totale e convertilo in ore, minuti e secondi
usando `/` e `%`.

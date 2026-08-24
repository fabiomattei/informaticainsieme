---
title: 'C: gli operatori'
date: '2026-08-23T00:00:00+01:00'
author: Fabio Mattei
layout: page
---

## Operatori aritmetici

Gli operatori aritmetici di C funzionano come in matematica, con una
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

{% highlight c %}
#include <stdio.h>

int main() {
    int a = 17, b = 5;

    printf("%d\n", a + b);   // 22
    printf("%d\n", a - b);   // 12
    printf("%d\n", a * b);   // 85
    printf("%d\n", a / b);   // 3  (divisione intera!)
    printf("%d\n", a % b);   // 2  (resto della divisione)

    double c = 17.0;
    printf("%.1f\n", c / b); // 3.4 (divisione reale)

    return 0;
}
{% endhighlight %}

---

## Operatori di confronto

Restituiscono `1` (vero) o `0` (falso): in C non esiste un vero tipo
booleano, si usa `int`.

| Operatore | Significato        | Esempio    | Risultato |
|:---------:|--------------------|------------|-----------|
| `==`      | uguale a           | `5 == 5`   | `1`       |
| `!=`      | diverso da         | `5 != 3`   | `1`       |
| `<`       | minore di          | `3 < 5`    | `1`       |
| `>`       | maggiore di        | `3 > 5`    | `0`       |
| `<=`      | minore o uguale    | `5 <= 5`   | `1`       |
| `>=`      | maggiore o uguale  | `3 >= 5`   | `0`       |

#### Esercizio 2
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight c %}
#include <stdio.h>

int main() {
    int x = 8;

    printf("%d\n", x > 5);   // 1
    printf("%d\n", x == 8);  // 1
    printf("%d\n", x != 8);  // 0
    printf("%d\n", x < 8);   // 0

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

{% highlight c %}
#include <stdio.h>

int main() {
    int n = 20;
    n += 5;   printf("%d\n", n);  // 25
    n -= 3;   printf("%d\n", n);  // 22
    n *= 2;   printf("%d\n", n);  // 44
    n /= 4;   printf("%d\n", n);  // 11
    n %= 3;   printf("%d\n", n);  // 2

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

{% highlight c %}
#include <stdio.h>

int main() {
    int i = 10;

    printf("%d\n", i++);  // stampa 10, poi i diventa 11
    printf("%d\n", i);    // 11

    printf("%d\n", ++i);  // i diventa 12, poi stampa 12
    printf("%d\n", i);    // 12

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
Leggi tre lati di un triangolo e stampa `1` se il triangolo è equilatero
(tutti i lati uguali) o isoscele (almeno due lati uguali), `0` altrimenti.

#### Esercizio 8
Calcola `2 + 3 * 4` e poi `(2 + 3) * 4`. Stampa entrambi i risultati e
spiega la differenza.

#### Esercizio 9
Leggi un numero di secondi totale e convertilo in ore, minuti e secondi
usando `/` e `%`.

---
title: 'Rust: gli operatori'
date: '2026-08-23T00:00:00+01:00'
author: Fabio Mattei
layout: page
---

## Operatori aritmetici

Gli operatori aritmetici di Rust funzionano come in matematica, con una
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

{% highlight rust %}
fn main() {
    let a = 17;
    let b = 5;

    println!("{}", a + b);   // 22
    println!("{}", a - b);   // 12
    println!("{}", a * b);   // 85
    println!("{}", a / b);   // 3  (divisione intera!)
    println!("{}", a % b);   // 2  (resto della divisione)

    let c: f64 = 17.0;
    println!("{}", c / b as f64);  // 3.4 (divisione reale)
}
{% endhighlight %}

Attenzione: Rust **non converte automaticamente** tra tipi numerici
diversi. `c / b` non compila perché `c` è `f64` e `b` è `i32`: serve un
cast esplicito con `as` (vedi la pagina sulle
[conversioni di tipo]({{ site.baseurl }}{% link _rust/conversioni-di-tipo.md %}.html)).

---

## Operatori di confronto

Restituiscono un valore `bool`: `true` o `false`.

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

{% highlight rust %}
fn main() {
    let x = 8;

    println!("{}", x > 5);   // true
    println!("{}", x == 8);  // true
    println!("{}", x != 8);  // false
    println!("{}", x < 8);   // false
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

{% highlight rust %}
fn main() {
    let mut n = 20;
    n += 5;   println!("{}", n);  // 25
    n -= 3;   println!("{}", n);  // 22
    n *= 2;   println!("{}", n);  // 44
    n /= 4;   println!("{}", n);  // 11
    n %= 3;   println!("{}", n);  // 2
}
{% endhighlight %}

Gli operatori composti richiedono sempre una variabile `mut`, dato che
modificano il valore esistente.

---

## Nessun ++ o --

A differenza di C e C++, Rust **non ha** gli operatori `++` e `--`. Per
incrementare o decrementare si usa sempre `+=` o `-=`.

#### Esercizio 4
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
fn main() {
    let mut i = 10;

    i += 1;
    println!("{}", i);  // 11

    i -= 1;
    println!("{}", i);  // 10
}
{% endhighlight %}

---

## Precedenza degli operatori

Gli operatori hanno una priorità (precedenza). Quelli con precedenza più
alta vengono valutati prima. Usare le parentesi quando il dubbio c'è.

| Priorità | Operatori             |
|---------:|-----------------------|
| alta     | `*` `/` `%`           |
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
Leggi un numero intero e determina se è pari o dispari usando
l'operatore `%`.

#### Esercizio 7
Leggi tre lati di un triangolo e stampa `true` se il triangolo è
equilatero (tutti i lati uguali) o isoscele (almeno due lati uguali).

#### Esercizio 8
Calcola `2 + 3 * 4` e poi `(2 + 3) * 4`. Stampa entrambi i risultati e
spiega la differenza.

#### Esercizio 9
Leggi un numero di secondi totale e convertilo in ore, minuti e secondi
usando `/` e `%`.

---
title: 'Rust: il ciclo for'
date: '2026-08-23T00:00:00+01:00'
author: Fabio Mattei
layout: page
---

## Iterare su un intervallo o una collezione

Il ciclo `for` in Rust scorre sempre gli elementi di un **iteratore**:
un intervallo numerico (`0..10`) oppure una collezione come un array o
un vettore. A differenza del `for` di C, non esiste una forma con
inizializzazione/condizione/aggiornamento manuali.

```
for elemento in iteratore {
    // istruzioni
}
```

---

## for su un intervallo (range)

#### Esercizio 1
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
fn main() {
    for i in 1..=5 {
        println!("{}", i);
    }
}
{% endhighlight %}

**Traccia di esecuzione:**

| i    | output |
|:----:|:------:|
| 1    | 1      |
| 2    | 2      |
| 3    | 3      |
| 4    | 4      |
| 5    | 5      |

`1..=5` è un intervallo **inclusivo** (include 5). `1..5`, senza `=`,
sarebbe **esclusivo** (si fermerebbe a 4).

---

## Contare all'indietro e usare step diversi

#### Esercizio 2
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
fn main() {
    // conto alla rovescia
    for i in (1..=5).rev() {
        print!("{} ", i);
    }
    println!();

    // solo numeri pari da 2 a 20
    for i in (2..=20).step_by(2) {
        print!("{} ", i);
    }
    println!();
}
{% endhighlight %}

`.rev()` inverte l'ordine dell'intervallo; `.step_by(n)` salta `n`
elementi alla volta.

---

## Accumulatore con for

#### Esercizio 3
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
use std::io;

fn main() {
    let mut input = String::new();
    println!("Inserisci N: ");
    io::stdin().read_line(&mut input).expect("Errore");
    let n: i32 = input.trim().parse().expect("Non è un numero");

    let mut somma = 0;
    for i in 1..=n {
        somma += i;
    }
    println!("Somma da 1 a {}: {}", n, somma);
}
{% endhighlight %}

---

## for su array e vettori

Scorrere un array o un `Vec` con `for` è il modo idiomatico in Rust:
non serve gestire un indice manuale come in C.

#### Esercizio 4
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
fn main() {
    let numeri = [10, 20, 30, 40, 50];

    for n in numeri {
        print!("{} ", n);
    }
    println!();

    // iterare su una stringa carattere per carattere
    let parola = "ciao";
    for c in parola.chars() {
        print!("{} ", c);
    }
    println!();
}
{% endhighlight %}

Se serve anche l'indice, si usa `.iter().enumerate()`:

{% highlight rust %}
let numeri = [10, 20, 30];
for (indice, valore) in numeri.iter().enumerate() {
    println!("posizione {}: {}", indice, valore);
}
{% endhighlight %}

---

## break e continue

`break` interrompe il ciclo immediatamente; `continue` salta alla
prossima iterazione.

#### Esercizio 5
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
fn main() {
    // break: si ferma al primo multiplo di 7
    for i in 1..=100 {
        if i % 7 == 0 {
            println!("Primo multiplo di 7: {}", i);
            break;
        }
    }

    // continue: stampa solo i dispari
    for i in 1..=10 {
        if i % 2 == 0 {
            continue;
        }
        print!("{} ", i);
    }
    println!();
}
{% endhighlight %}

---

## Cicli annidati

#### Esercizio 6
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
fn main() {
    // tavola pitagorica 5x5
    for i in 1..=5 {
        for j in 1..=5 {
            print!("{}\t", i * j);
        }
        println!();
    }
}
{% endhighlight %}

---

## Esercizi

#### Esercizio 7
Scrivi un programma che legga N e stampi i primi N numeri della
successione di Fibonacci usando un ciclo `for`.

#### Esercizio 8
Leggi 10 numeri interi in un vettore con un ciclo `for` e stampa il
massimo e il minimo.

#### Esercizio 9
Stampa un triangolo rettangolo di asterischi di altezza N letta in
input: la prima riga ha 1 asterisco, la seconda 2, ecc.

#### Esercizio 10
Scrivi un programma che calcoli N! (fattoriale) con un ciclo `for`.

#### Esercizio 11
Usando cicli annidati, stampa le tabelline dalla 1 alla 10 nel formato
`1 x 1 = 1`, `1 x 2 = 2`, …

---
title: 'Rust: le condizioni'
date: '2026-08-23T00:00:00+01:00'
author: Fabio Mattei
layout: page
---

## Eseguire codice solo in certi casi

Le istruzioni condizionali permettono di scegliere quale blocco di
codice eseguire in base al valore di un'espressione booleana. In Rust
le parole chiave sono `if`, `else if`, `else` e `match`.

Il blocco di codice da eseguire è racchiuso tra **parentesi graffe**
`{ }`, sempre obbligatorie. Le parentesi tonde attorno alla condizione,
invece, **non si usano**.

## if semplice

#### Esercizio 1
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
use std::io;

fn main() {
    let mut input = String::new();
    println!("Inserisci un numero: ");
    io::stdin().read_line(&mut input).expect("Errore");
    let numero: i32 = input.trim().parse().expect("Non è un numero");

    if numero > 0 {
        println!("Il numero è positivo.");
    }
}
{% endhighlight %}

L'espressione `numero > 0` ha come risultato un valore di tipo `bool`.
Il blocco tra `{ }` viene eseguito soltanto se la condizione vale
`true`. A differenza del C, in Rust una condizione **deve** essere
`bool`: non è possibile scrivere `if numero { ... }` con un intero.

## if / else

#### Esercizio 2
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
use std::io;

fn main() {
    let mut input = String::new();
    println!("Inserisci un numero: ");
    io::stdin().read_line(&mut input).expect("Errore");
    let numero: i32 = input.trim().parse().expect("Non è un numero");

    if numero % 2 == 0 {
        println!("Il numero è pari.");
    } else {
        println!("Il numero è dispari.");
    }
}
{% endhighlight %}

## if come espressione

In Rust `if` è un'**espressione**: può restituire un valore, che si può
assegnare direttamente a una variabile. È l'equivalente dell'operatore
ternario di altri linguaggi.

#### Esercizio 3
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
fn main() {
    let a = 7;
    let b = 12;

    let massimo = if a > b { a } else { b };
    println!("Il massimo è: {}", massimo);
}
{% endhighlight %}

I due rami (`{ a }` e `{ b }`) devono restituire lo **stesso tipo**: il
compilatore lo verifica.

## if / else if / else

Quando le alternative sono più di due si concatenano `else if`.

#### Esercizio 4
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
use std::io;

fn main() {
    let mut input = String::new();
    println!("Inserisci il voto (0-10): ");
    io::stdin().read_line(&mut input).expect("Errore");
    let voto: i32 = input.trim().parse().expect("Non è un numero");

    if voto >= 9 {
        println!("Ottimo");
    } else if voto >= 7 {
        println!("Buono");
    } else if voto >= 6 {
        println!("Sufficiente");
    } else {
        println!("Insufficiente");
    }
}
{% endhighlight %}

Le condizioni vengono valutate dall'alto verso il basso: appena una è
vera, il relativo blocco viene eseguito e le successive vengono
saltate.

## match

`match` è l'equivalente Rust dello `switch` di C, ma più potente: il
compilatore obbliga a coprire **tutti** i casi possibili (esaustività).
`_` rappresenta il caso "in tutti gli altri casi".

#### Esercizio 5
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
use std::io;

fn main() {
    let mut input = String::new();
    println!("Inserisci il numero del giorno (1-7): ");
    io::stdin().read_line(&mut input).expect("Errore");
    let giorno: i32 = input.trim().parse().expect("Non è un numero");

    let nome = match giorno {
        1 => "Lunedì",
        2 => "Martedì",
        3 => "Mercoledì",
        4 => "Giovedì",
        5 => "Venerdì",
        6 => "Sabato",
        7 => "Domenica",
        _ => "Giorno non valido",
    };

    println!("{}", nome);
}
{% endhighlight %}

A differenza dello `switch` in C, non serve `break`: solo un ramo viene
eseguito, senza *fall-through*. `match` è inoltre un'espressione: il suo
risultato può essere assegnato direttamente, come fatto qui con `nome`.

## match su intervalli

#### Esercizio 6
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
use std::io;

fn main() {
    let mut input = String::new();
    println!("Inserisci la temperatura: ");
    io::stdin().read_line(&mut input).expect("Errore");
    let temp: i32 = input.trim().parse().expect("Non è un numero");

    let descrizione = match temp {
        t if t < 0 => "gelido",
        0..=10 => "freddo",
        11..=20 => "fresco",
        21..=30 => "caldo",
        _ => "torrido",
    };

    println!("{}", descrizione);
}
{% endhighlight %}

`0..=10` è un intervallo **inclusivo** su entrambi gli estremi.
`t if t < 0` è una **guardia**: aggiunge una condizione extra al ramo.

## Esercizi

#### Esercizio 7
Leggi un numero intero e stampa "positivo", "negativo" o "zero" a
seconda del caso, usando `match`.

#### Esercizio 8
Leggi un anno e determina se è bisestile.
(Condizione: divisibile per 4, eccetto i centenari, che devono essere
divisibili per 400.)

#### Esercizio 9
Leggi tre interi e stampa il maggiore dei tre usando `if/else if/else`.

#### Esercizio 10
Scrivi un calcolatore semplice: leggi due numeri e un carattere
operatore (`+`, `-`, `*`, `/`). Usa `match` sul carattere per eseguire
l'operazione scelta. Gestisci la divisione per zero.

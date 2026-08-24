---
title: 'Rust: il ciclo while'
date: '2026-08-23T00:00:00+01:00'
author: Fabio Mattei
layout: page
---

## Ripetere finché una condizione è vera

Il ciclo `while` esegue un blocco di codice ripetutamente finché la sua
condizione rimane `true`. La condizione viene valutata **prima** di
ogni iterazione: se è già falsa alla prima valutazione, il blocco non
viene mai eseguito.

```
while condizione {
    // istruzioni
}
```

Come per `if`, non si usano parentesi tonde attorno alla condizione.

---

## Contatore

#### Esercizio 1
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
fn main() {
    let mut i = 1;
    while i <= 5 {
        println!("{}", i);
        i += 1;
    }
}
{% endhighlight %}

**Traccia di esecuzione:**

| Iterazione | i (inizio) | condizione | output | i (fine) |
|:----------:|:----------:|:----------:|:------:|:--------:|
| 1          | 1          | 1 ≤ 5 ✓   | 1      | 2        |
| 2          | 2          | 2 ≤ 5 ✓   | 2      | 3        |
| 3          | 3          | 3 ≤ 5 ✓   | 3      | 4        |
| 4          | 4          | 4 ≤ 5 ✓   | 4      | 5        |
| 5          | 5          | 5 ≤ 5 ✓   | 5      | 6        |
| —          | 6          | 6 ≤ 5 ✗   | —      | —        |

---

## Accumulatore

#### Esercizio 2
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
use std::io;

fn main() {
    let mut input = String::new();
    println!("Inserisci N: ");
    io::stdin().read_line(&mut input).expect("Errore");
    let n: i32 = input.trim().parse().expect("Non è un numero");

    let mut somma = 0;
    let mut i = 1;
    while i <= n {
        somma += i;
        i += 1;
    }
    println!("Somma da 1 a {}: {}", n, somma);
}
{% endhighlight %}

---

## Il ciclo loop

Rust ha un costrutto in più rispetto a C: `loop`, un ciclo
**esplicitamente infinito**, che si interrompe solo con `break`. È il
modo idiomatico di scrivere `while true` in Rust.

```
loop {
    // istruzioni
    break;  // interrompe il ciclo
}
```

#### Esercizio 3
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
use std::io;

fn main() {
    let segreto = 42;

    loop {
        let mut input = String::new();
        println!("Indovina il numero (1-100): ");
        io::stdin().read_line(&mut input).expect("Errore");
        let tentativo: i32 = input.trim().parse().expect("Non è un numero");

        if tentativo == segreto {
            println!("Corretto!");
            break;
        } else if tentativo < segreto {
            println!("Troppo basso.");
        } else {
            println!("Troppo alto.");
        }
    }
}
{% endhighlight %}

---

## loop come espressione: restituire un valore con break

A differenza di `while`, `loop` è un'**espressione**: `break valore`
interrompe il ciclo restituendo `valore`, che può essere assegnato a
una variabile.

#### Esercizio 4
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
fn main() {
    let mut contatore = 0;

    let risultato = loop {
        contatore += 1;
        if contatore == 10 {
            break contatore * 2;
        }
    };

    println!("Risultato: {}", risultato);  // 20
}
{% endhighlight %}

Questa è una caratteristica esclusiva di Rust: né `while` né `for` in C
possono restituire un valore in questo modo.

---

## Input ripetuto fino a un valore sentinella

#### Esercizio 5
Copia il seguente codice nell'editor e fallo eseguire.
Inserisci voti terminando con -1.

{% highlight rust %}
use std::io;

fn main() {
    let mut somma = 0;
    let mut contatore = 0;

    println!("Inserisci i voti (-1 per terminare):");

    loop {
        let mut input = String::new();
        io::stdin().read_line(&mut input).expect("Errore");
        let voto: i32 = input.trim().parse().expect("Non è un numero");

        if voto == -1 {
            break;
        }

        somma += voto;
        contatore += 1;
    }

    if contatore > 0 {
        let media = somma as f64 / contatore as f64;
        println!("Media: {:.2}", media);
    } else {
        println!("Nessun voto inserito.");
    }
}
{% endhighlight %}

---

## Esercizi

#### Esercizio 6
Scrivi un programma che stampi la tavola pitagorica di un numero N letto
in input, usando un ciclo `while`.

#### Esercizio 7
Leggi numeri positivi finché l'utente inserisce 0. Stampa il massimo tra
i numeri letti. Usa `loop` e `break`.

#### Esercizio 8
Calcola il fattoriale di N usando un ciclo `while`.

#### Esercizio 9
Scrivi un programma che legga una password (stringa) con `loop` finché
l'utente non inserisce quella corretta ("segreto").

#### Esercizio 10
Genera la successione di Fibonacci fino a un valore massimo M letto in
input: stampa tutti i termini ≤ M.

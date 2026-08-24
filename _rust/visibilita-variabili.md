---
title: 'Rust: visibilità delle variabili'
date: '2026-08-23T00:00:00+01:00'
author: Fabio Mattei
layout: page
---

## Dove "vive" una variabile

La **visibilità** (o *scope*) di una variabile è la porzione di codice
in cui quella variabile esiste ed è accessibile. In Rust lo scope è
definito dalle **parentesi graffe** `{ }`: una variabile dichiarata
all'interno di un blocco è visibile solo in quel blocco e cessa di
esistere quando il blocco termina (dove, per i tipi come `String`, la
sua memoria viene anche liberata — vedi
[ownership e borrowing]({{ site.baseurl }}{% link _rust/ownership.md %}.html)).

---

## Variabili locali

Una variabile dichiarata dentro una funzione (o un blocco) è
**locale**: non esiste fuori da quel contesto.

#### Esercizio 1
Copia il seguente codice nell'editor e osserva l'errore del compilatore.

{% highlight rust %}
fn funzione() {
    let x = 10;  // x esiste solo dentro funzione()
    println!("{}", x);
}

fn main() {
    funzione();
    // println!("{}", x);  // ERRORE: x non esiste qui
}
{% endhighlight %}

---

## Scope di blocco e shadowing

Le parentesi graffe di un blocco qualsiasi creano uno scope. Rust
permette anche di dichiarare di nuovo una variabile con lo stesso nome
(*shadowing*): la nuova dichiarazione nasconde la precedente per il
resto del blocco.

#### Esercizio 2
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
fn main() {
    let x = 5;
    println!("fuori: {}", x);   // 5

    {
        let x = 100;  // shadowing: nuova x locale al blocco
        println!("dentro: {}", x);   // 100
    }

    println!("fuori: {}", x);   // 5 (la x esterna è invariata)
}
{% endhighlight %}

A differenza di `mut`, ogni `let x = ...` crea una **nuova variabile**:
non è una modifica della precedente, ma una sostituzione visibile solo
da quel punto in poi nello scope corrente.

---

## Costanti globali

Una `const` dichiarata fuori da qualsiasi funzione è visibile in tutto
il file (o, se preceduta da `pub`, anche fuori dal modulo). A
differenza delle variabili globali mutabili di C, Rust **scoraggia
fortemente** lo stato globale mutabile: renderlo sicuro richiede
costrutti espliciti pensati apposta (fuori dallo scopo di questa
introduzione).

#### Esercizio 3
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
const LIMITE_MASSIMO: i32 = 100;

fn controlla(valore: i32) -> bool {
    valore <= LIMITE_MASSIMO
}

fn main() {
    println!("{}", controlla(50));   // true
    println!("{}", controlla(150));  // false
}
{% endhighlight %}

---

## Blocchi come espressioni

Un blocco `{ }` in Rust può restituire un valore: l'**ultima riga senza
punto e virgola** diventa il risultato del blocco, assegnabile
direttamente a una variabile.

#### Esercizio 4
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
fn main() {
    let y = {
        let a = 3;
        let b = 4;
        a * a + b * b  // risultato del blocco: nessun ; alla fine
    };

    println!("{}", y);  // 25
    // a e b non esistono qui: erano locali al blocco
}
{% endhighlight %}

---

## Variabili della stessa funzione vs blocchi interni

#### Esercizio 5
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
fn main() {
    for i in 0..3 {
        let quadrato = i * i;  // dichiarata a ogni iterazione
        println!("{}^2 = {}", i, quadrato);
    }
    // i e quadrato non esistono qui

    let mut somma = 0;
    for i in 1..=5 {
        somma += i;  // somma è dichiarata fuori: rimane
    }
    println!("Somma: {}", somma);  // 15
}
{% endhighlight %}

`i` del ciclo `for` esiste solo dentro il ciclo. `somma` è dichiarata
fuori e persiste dopo il ciclo.

---

## Esercizi

#### Esercizio 6
Scrivi un programma con una funzione `conta()` che riceve e restituisce
un contatore (`fn conta(n: i32) -> i32`, poiché Rust non ha variabili
`static` mutabili semplici come il C). Chiamala più volte accumulando
il risultato in `main` e spiega perché questo approccio è più esplicito
di una variabile globale mutabile.

#### Esercizio 7
Scrivi un programma con tre blocchi annidati `{ }`, ognuno con una
variabile `n` con valore diverso (usa lo shadowing). Stampa `n` in
ciascun blocco e spiega quale `n` viene usata.

#### Esercizio 8
Scrivi una funzione `fn massimo_di_vettore(v: &Vec<i32>) -> i32` che
trovi il massimo. La variabile `massimo` deve essere locale alla
funzione. Chiama la funzione da `main` su tre vettori diversi.

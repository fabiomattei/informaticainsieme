---
title: 'Linguaggio Rust'
date: '2026-08-23T00:00:00+01:00'
author: Fabio Mattei
layout: page
---

Il linguaggio **Rust** è stato sviluppato da **Graydon Hoare** e successivamente sostenuto da Mozilla, con la prima release stabile nel 2015. Rust nasce per risolvere un problema storico dei linguaggi come C e C++: ottenere le stesse prestazioni e lo stesso controllo sulla memoria, ma **senza** le classi di errori più pericolose (accessi a memoria non valida, race condition, puntatori penzolanti) — verificate dal compilatore **prima** che il programma venga eseguito.

Rust è un linguaggio **compilato**, a **tipizzazione statica**, senza garbage collector: la gestione della memoria è affidata a un sistema di regole chiamato **ownership** (proprietà), controllato interamente a tempo di compilazione. È oggi usato per sistemi operativi, browser (parti di Firefox), strumenti da riga di comando, sistemi embedded e servizi ad alte prestazioni.

Per esercitarci con Rust useremo il [Rust Playground](https://play.rust-lang.org/), un compilatore online che non richiede installazione. Per un uso più continuativo si può installare la toolchain ufficiale con [rustup](https://rustup.rs/).

Ecco un primo esempio di codice:

{% highlight rust %}
fn main() {
    // definizione delle variabili
    let base = 10;
    let altezza = 5;
    // esecuzione delle elaborazioni
    let area = base * altezza;
    // comunicazione dei risultati
    println!("Il rettangolo con:");
    println!("base: {}", base);
    println!("altezza: {}", altezza);
    println!("ha come area: {}", area);
}
{% endhighlight %}

A differenza di Python, in Rust le variabili sono **immutabili di default**: `let base = 10;` non può essere riassegnata. Il tipo viene quasi sempre **dedotto** automaticamente dal compilatore, ma può anche essere dichiarato esplicitamente (`let base: i32 = 10;`).

L'assegnazione ha una sintassi simile a Python così come gli operatori matematici sono analoghi.

Il comando per mandare le informazioni in output è la macro **println!**, che usa `{}` come segnaposto per i valori da inserire nella stringa.

**Introduciamo l'input**

{% highlight rust %}
use std::io;

fn main() {
    let mut input = String::new();
    println!("Inserisci un numero: ");
    io::stdin()
        .read_line(&mut input)
        .expect("Errore nella lettura");

    let numero: i32 = input.trim().parse().expect("Non è un numero valido");
    println!("Hai inserito: {}", numero);
}
{% endhighlight %}

Utilizziamo `io::stdin().read_line(...)` per leggere una riga di testo dall'utente. Il risultato è sempre una `String`: per ottenere un numero occorre convertirla con `.parse()`. Notiamo `mut`: `input` deve essere dichiarata **mutabile** perché `read_line` la modifica.

**Condizione**

La sintassi della condizione in Rust è simile a quella del C. La parola chiave è **if**, ma le parentesi attorno alla condizione **non sono richieste** (le parentesi graffe invece sono sempre obbligatorie).

{% highlight rust %}
use std::io;

fn main() {
    let mut input = String::new();
    io::stdin().read_line(&mut input).expect("Errore");
    let numero: i32 = input.trim().parse().expect("Non è un numero");

    if numero > 0 {
        println!("numero positivo");
    } else {
        println!("numero negativo");
    }
}
{% endhighlight %}

**L'iterazione**

L'iterazione in Rust può essere definita con la sintassi `for`, che scorre un **intervallo** (range) o una collezione, senza bisogno di gestire manualmente un indice.

{% highlight rust %}
fn main() {
    println!("inizio ciclo for");
    for i in 0..10 {
        println!("i vale: {}", i);
    }
}
{% endhighlight %}

**Il ciclo while**

L'iterazione in Rust può essere definita anche con la sintassi `while`. In questo caso inizializziamo un contatore **cont** a zero. La condizione, tra parentesi facoltative, viene valutata a ogni inizio iterazione; se il risultato è vero vengono eseguite le istruzioni all'interno delle parentesi graffe.

{% highlight rust %}
fn main() {
    println!("inizio ciclo while");
    let mut cont = 0;
    while cont < 10 {
        println!("cont vale: {}", cont);
        cont += 1;
    }
}
{% endhighlight %}

---
title: 'Rust: array e vettori'
date: '2026-08-23T00:00:00+01:00'
author: Fabio Mattei
layout: page
---

## Collezioni di elementi dello stesso tipo

Rust ha due forme principali di collezione sequenziale:

- **Array** (`[T; N]`) — dimensione **fissa**, nota a tempo di
  compilazione, allocato sullo stack.
- **`Vec<T>`** (vettore) — dimensione **variabile**, allocato
  sull'heap; la scelta preferita quando la dimensione non è nota in
  anticipo.

---

## Array a dimensione fissa

#### Esercizio 1
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
fn main() {
    let mut numeri: [i32; 5] = [10, 20, 30, 40, 50];

    println!("{}", numeri[0]);  // 10
    println!("{}", numeri[4]);  // 50

    numeri[2] = 99;
    println!("{}", numeri[2]);  // 99

    // visita con for
    for n in numeri {
        print!("{} ", n);
    }
    println!();
}
{% endhighlight %}

Gli indici partono da 0. A differenza del C, in Rust un accesso fuori
dai limiti (`numeri[10]`) causa un **panic** controllato a runtime
(il programma termina con un messaggio chiaro), non un comportamento
indefinito.

---

## Vec<T> — array dinamico

`Vec<T>` si ridimensiona automaticamente. Si crea con `Vec::new()` (poi
riempito con `.push()`) o con la macro `vec![...]`.

#### Esercizio 2
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
fn main() {
    let mut v = vec![1, 2, 3];

    v.push(4);   // aggiunge in fondo
    v.push(5);

    println!("Dimensione: {}", v.len());          // 5
    println!("Primo: {}", v[0]);                   // 1
    println!("Ultimo: {}", v[v.len() - 1]);         // 5

    v.pop();   // rimuove l'ultimo, restituisce Option<T>
    println!("Dopo pop: {}", v.len());              // 4
}
{% endhighlight %}

---

## Accesso sicuro con get()

L'operatore `[]` causa un panic se l'indice non esiste. Il metodo
`.get(i)` restituisce invece un `Option<&T>`: `Some(valore)` se
l'indice esiste, `None` altrimenti — nessun crash.

#### Esercizio 3
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
fn main() {
    let frutti = vec!["mela", "pera", "banana"];

    match frutti.get(1) {
        Some(f) => println!("Trovato: {}", f),
        None => println!("Indice non valido"),
    }

    match frutti.get(10) {
        Some(f) => println!("Trovato: {}", f),
        None => println!("Indice non valido"),  // questo viene stampato
    }

    for f in &frutti {
        print!("{} ", f);
    }
    println!();
}
{% endhighlight %}

---

## Metodi utili di Vec

| Metodo        | Significato                               |
|---------------|--------------------------------------------|
| `.push(x)`    | aggiunge `x` in fondo                     |
| `.pop()`      | rimuove e restituisce l'ultimo elemento (`Option<T>`) |
| `.len()`      | numero di elementi                        |
| `.is_empty()` | `true` se il vettore è vuoto              |
| `.clear()`    | svuota il vettore                         |
| `.get(i)`     | accesso sicuro (`Option<&T>`)             |
| `.contains(&x)` | `true` se `x` è presente                |

---

## Ordinamento con sort

#### Esercizio 4
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
fn main() {
    let mut v = vec![5, 2, 8, 1, 9, 3];

    v.sort();   // ordine crescente
    println!("{:?}", v);

    v.sort_by(|a, b| b.cmp(a));  // ordine decrescente
    println!("{:?}", v);
}
{% endhighlight %}

---

## Leggere un vettore da input

#### Esercizio 5
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
use std::io;

fn main() {
    let mut input = String::new();
    println!("Quanti numeri? ");
    io::stdin().read_line(&mut input).expect("Errore");
    let n: usize = input.trim().parse().expect("Non è un numero");

    let mut v: Vec<i32> = Vec::new();
    for _ in 0..n {
        let mut riga = String::new();
        io::stdin().read_line(&mut riga).expect("Errore");
        let x: i32 = riga.trim().parse().expect("Non è un numero");
        v.push(x);
    }

    let massimo = v.iter().max().expect("Vettore vuoto");
    println!("Massimo: {}", massimo);
}
{% endhighlight %}

`.iter().max()` scorre il vettore e restituisce un `Option<&i32>`: il
massimo, se il vettore non è vuoto.

---

## Esercizi

#### Esercizio 6
Leggi N numeri in un `Vec<f64>` e calcola la media aritmetica.

#### Esercizio 7
Leggi N interi in un vettore e stampa separatamente i pari e i dispari.

#### Esercizio 8
Leggi N stringhe in un `Vec<String>`, ordinale con `.sort()` e stampale
in ordine alfabetico.

#### Esercizio 9
Implementa la ricerca lineare: leggi N interi, poi un valore da
cercare; stampa l'indice della prima occorrenza o -1 se non trovato.
(Suggerimento: puoi anche usare `.iter().position(|&x| x == valore)`.)

#### Esercizio 10
Scrivi una funzione `fn inverti(v: &mut Vec<i32>)` che inverte l'ordine
degli elementi in-place (senza usare `.reverse()`).

---
title: 'Rust: input e output'
date: '2026-08-23T00:00:00+01:00'
author: Fabio Mattei
layout: page
---

## println! e stdin

In Rust l'output avviene tramite la macro `println!`, disponibile senza
bisogno di alcun `use`. L'input avviene invece tramite
`std::io::stdin()`, che richiede `use std::io;`.

`println!` usa `{}` come segnaposto: i valori da inserire vengono
passati come argomenti successivi alla stringa.

---

## Output con println!

#### Esercizio 1
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
fn main() {
    println!("Ciao, mondo!");
    println!("La risposta è: {}", 42);

    let eta = 25;
    println!("Hai {} anni.", eta);
}
{% endhighlight %}

`println!` manda sempre a capo alla fine. Per stampare senza andare a
capo si usa `print!`. Con `{:?}` si stampa la rappresentazione di
**debug** di un valore, utile per tipi composti come vettori e struct.

{% highlight rust %}
print!("Prima parte ");
print!("seconda parte\n");

let v = vec![1, 2, 3];
println!("{:?}", v);  // [1, 2, 3]
{% endhighlight %}

---

## Input con stdin

#### Esercizio 2
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
use std::io;

fn main() {
    let mut eta = String::new();

    println!("Inserisci la tua età: ");
    io::stdin()
        .read_line(&mut eta)
        .expect("Errore nella lettura");

    let eta: i32 = eta.trim().parse().expect("Non è un numero valido");
    println!("Hai {} anni.", eta);
}
{% endhighlight %}

`read_line` legge una riga di testo e la **aggiunge** alla `String`
passata come riferimento mutabile (`&mut`). Il risultato include sempre
il carattere di fine riga: `.trim()` lo rimuove prima di convertire il
testo in numero con `.parse()`.

---

## Leggere più valori

Per leggere più numeri sulla stessa riga si legge l'intera riga e la si
divide con `.split_whitespace()`.

#### Esercizio 3
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
use std::io;

fn main() {
    let mut input = String::new();
    println!("Inserisci base e altezza: ");
    io::stdin().read_line(&mut input).expect("Errore");

    let numeri: Vec<i32> = input
        .trim()
        .split_whitespace()
        .map(|s| s.parse().expect("Non è un numero"))
        .collect();

    let area = numeri[0] * numeri[1];
    println!("Area del rettangolo: {}", area);
}
{% endhighlight %}

---

## Gestire input non valido con match

`.expect(...)` interrompe il programma se la conversione fallisce.
Un'alternativa più robusta è usare `match` sul risultato di `.parse()`.

#### Esercizio 4
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
use std::io;

fn main() {
    let mut input = String::new();
    println!("Inserisci un numero: ");
    io::stdin().read_line(&mut input).expect("Errore");

    let numero: i32 = match input.trim().parse() {
        Ok(n) => n,
        Err(_) => {
            println!("Input non valido, uso 0.");
            0
        }
    };

    println!("Numero: {}", numero);
}
{% endhighlight %}

`.parse()` restituisce un `Result`: `Ok(valore)` se la conversione
riesce, `Err(errore)` altrimenti. `match` permette di gestire entrambi i
casi senza far terminare il programma.

---

## Riepilogo: expect vs match

| Approccio       | Comportamento in caso di errore              |
|------------------|-----------------------------------------------|
| `.expect("msg")` | il programma termina (*panic*) col messaggio  |
| `match` su `Result` | il codice decide cosa fare, senza terminare |

---

## Esercizi

#### Esercizio 5
Scrivi un programma che legga nome e cognome separatamente (due letture
con `read_line`) e li stampi nella forma "Cognome, Nome".

#### Esercizio 6
Leggi due numeri decimali (`f64`) sulla stessa riga e stampa la loro
somma, differenza e prodotto.

#### Esercizio 7
Leggi il raggio di un cerchio e stampa area e circonferenza.
Usa `const PI: f64 = 3.14159;`.

#### Esercizio 8
Leggi una frase intera e stampala al contrario carattere per carattere
(suggerimento: `frase.chars().rev()`).

#### Esercizio 9
Scrivi un programma che legga tre interi sulla stessa riga, calcoli la
loro media come `f64` e la stampi con due cifre decimali
(suggerimento: `println!("{:.2}", media)`).

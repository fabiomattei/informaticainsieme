---
title: 'Rust: conversioni di tipo'
date: '2026-08-23T00:00:00+01:00'
author: Fabio Mattei
layout: page
---

## Nessuna conversione implicita

A differenza di C e C++, in Rust **non esiste alcuna conversione
implicita** tra tipi numerici diversi, nemmeno tra `i32` e `f64`. Ogni
conversione va richiesta esplicitamente, il più delle volte con
l'operatore `as`.

---

## Il cast con as

#### Esercizio 1
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
fn main() {
    let interi: i32 = 7;
    let decimale: f64 = interi as f64;  // conversione esplicita

    println!("{}", decimale);  // 7

    let pi: f64 = 3.14;
    let tronco: i32 = pi as i32;        // f64 -> i32
    println!("{}", tronco);    // 3 (la parte decimale viene scartata)
}
{% endhighlight %}

La conversione da `f64` a `i32` con `as` tronca la parte decimale
**senza arrotondare**, come in C.

---

## Divisione intera vs divisione reale

#### Esercizio 2
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
fn main() {
    let a = 7;
    let b = 2;

    // divisione intera: il risultato è 3, non 3.5
    println!("{}", a / b);

    // per ottenere 3.5 si convertono gli operandi
    let risultato = a as f64 / b as f64;
    println!("{}", risultato);  // 3.5
}
{% endhighlight %}

---

## Da stringa a numero: parse

Per convertire una stringa (`&str` o `String`) in un numero si usa il
metodo generico `.parse::<Tipo>()`, che restituisce un `Result`.

#### Esercizio 3
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
fn main() {
    let s1 = "42";
    let s2 = "3.14";

    let n: i32 = s1.parse().expect("Non è un intero valido");
    let d: f64 = s2.parse().expect("Non è un decimale valido");

    println!("{}", n + 1);       // 43
    println!("{}", d * 2.0);     // 6.28
}
{% endhighlight %}

Il tipo di destinazione può essere dedotto dal contesto (come in questo
esempio, grazie all'annotazione su `n` e `d`) oppure specificato
esplicitamente: `s1.parse::<i32>()`.

---

## Da numero a stringa: to_string / format!

#### Esercizio 4
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
fn main() {
    let punteggio = 100;
    let media = 7.5;

    let messaggio = format!("Punteggio: {}", punteggio);
    println!("{}", messaggio);

    let risultato = punteggio.to_string() + " punti";
    println!("{}", risultato);
    println!("Media: {}", media.to_string());
}
{% endhighlight %}

`format!` funziona come `println!` ma restituisce una `String` invece
di stamparla. `.to_string()` converte qualsiasi tipo che implementa il
trait `Display` (numeri inclusi) in `String`.

---

## Convertire char ↔ numero

#### Esercizio 5
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
fn main() {
    let c: char = 'A';
    let codice: u32 = c as u32;
    println!("{}", codice);   // 65

    let num: u32 = 97;
    let lettera: char = num as u8 as char;
    println!("{}", lettera);  // a
}
{% endhighlight %}

La conversione da numero a `char` non può usare `as char` direttamente
su tutti i tipi interi: passa spesso per `u8` (per i codici ASCII) o si
usa `char::from_u32(num)`, che restituisce un `Option<char>` più sicuro.

---

## Esercizi

#### Esercizio 6
Leggi due interi con `read_line` e stampa la loro divisione come numero
decimale (non intera). Usa `as f64` per forzare la divisione reale.

#### Esercizio 7
Leggi una stringa dall'utente che rappresenta un prezzo (es. "12.50"),
convertila in `f64` con `.parse()` e stampa il prezzo con IVA al 22%.

#### Esercizio 8
Leggi un numero intero con `read_line`. Concatenalo a una stringa di
testo usando `format!` e stampa il risultato.

#### Esercizio 9
Scrivi un programma che converta gradi Celsius (interi, letti da input)
in Fahrenheit (`f64`). Formula: `F = C * 9.0 / 5.0 + 32.0`.
Spiega perché è necessario convertire `C` con `as f64`.

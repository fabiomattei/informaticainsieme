---
title: 'Rust: i tipi di dato'
date: '2026-08-23T00:00:00+01:00'
author: Fabio Mattei
layout: page
---

## Tipizzazione statica e inferenza

In Rust ogni variabile ha un tipo **fisso**, verificato a tempo di
compilazione. Nella maggior parte dei casi non serve scriverlo: il
compilatore lo **deduce** dal valore assegnato (*type inference*). Il
tipo può comunque essere sempre annotato esplicitamente.

---

## Tipi interi

Rust ha tipi interi **con dimensione esplicita nel nome**: `i32` è un
intero con segno a 32 bit, `u32` un intero senza segno a 32 bit, e così
via. Se non specificato, il tipo di default è `i32`.

#### Esercizio 1
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
fn main() {
    let a: i32 = 42;
    let b: i64 = 1_000_000;
    let c: u32 = 4_000_000_000 / 4;

    println!("i32: {}", a);
    println!("i64: {}", b);
    println!("u32: {}", c);
}
{% endhighlight %}

| Tipo   | Dimensione | Intervallo                    |
|--------|:----------:|--------------------------------|
| `i32`  | 4 byte     | −2 miliardi … +2 miliardi      |
| `u32`  | 4 byte     | 0 … +4 miliardi                |
| `i64`  | 8 byte     | ±9,2 × 10¹⁸                    |

Il carattere `_` nei numerici letterali (`1_000_000`) serve solo a
migliorare la leggibilità: viene ignorato dal compilatore.

---

## Tipi a virgola mobile

#### Esercizio 2
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
fn main() {
    let f: f32 = 3.14;
    let d: f64 = 3.14159265358979;

    println!("f32: {}", f);
    println!("f64: {}", d);
}
{% endhighlight %}

`f64` è il tipo di default per i numeri a virgola mobile: ha più cifre
significative di `f32` ed è preferito per la maggior parte dei calcoli.

---

## Il tipo bool

#### Esercizio 3
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
fn main() {
    let vero: bool = true;
    let falso: bool = false;

    println!("{}", vero);   // true
    println!("{}", falso);  // false
}
{% endhighlight %}

A differenza del C, in Rust `bool` è un tipo primitivo vero e proprio:
non può essere usato al posto di un intero e viceversa.

---

## Il tipo char

#### Esercizio 4
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
fn main() {
    let lettera: char = 'A';
    println!("{}", lettera);          // A
    println!("{}", lettera as u32);   // 65 (codice Unicode)

    let emoji: char = '🦀';
    println!("{}", emoji);
}
{% endhighlight %}

Un `char` in Rust rappresenta un **carattere Unicode** (4 byte), non
solo ASCII come in C: può contenere lettere accentate, ideogrammi o
emoji.

---

## I tipi stringa: String e &str

Rust ha due tipi principali per il testo: `String` (di proprietà,
modificabile, allocata sull'heap) e `&str` (un riferimento a una
sequenza di testo). L'argomento è approfondito nella pagina dedicata
alle [stringhe]({{ site.baseurl }}{% link _rust/stringhe.md %}.html).

{% highlight rust %}
let letterale: &str = "ciao";       // &str
let proprietaria: String = String::from("ciao"); // String
{% endhighlight %}

---

## std::mem::size_of

`std::mem::size_of::<Tipo>()` restituisce la dimensione in byte di un
tipo.

{% highlight rust %}
println!("{}", std::mem::size_of::<i32>());    // 4
println!("{}", std::mem::size_of::<f64>());    // 8
println!("{}", std::mem::size_of::<char>());   // 4
{% endhighlight %}

---

## Esercizi

#### Esercizio 5
Dichiara una variabile di ciascun tipo fondamentale (`i32`, `f64`,
`bool`, `char`), assegna valori a scelta e stampali tutti.

#### Esercizio 6
Dichiara due variabili `i32` e una `f64`. Esegui una divisione tra i due
interi assegnando il risultato alla `f64`. Cosa noti se non converti il
tipo? (suggerimento: prova `5 / 2` vs `5.0 / 2.0`)

#### Esercizio 7
Scrivi un programma che stampi i codici Unicode delle lettere `'A'` fino
a `'Z'` usando un ciclo `for` su un range di `char`
(`for c in 'A'..='Z'`).

#### Esercizio 8
Dichiara un `i64` con il valore 10.000.000.000 (dieci miliardi) e
stampalo. Prova a usare `i32` con lo stesso valore e osserva l'errore
del compilatore.

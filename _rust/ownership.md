---
title: 'Rust: ownership e borrowing'
date: '2026-08-23T00:00:00+01:00'
author: Fabio Mattei
layout: page
---

## Il concetto cardine di Rust

L'**ownership** (proprietà) è il sistema con cui Rust gestisce la
memoria **senza** garbage collector e **senza** i rischi tipici della
gestione manuale del C (memory leak, doppia liberazione, puntatori
penzolanti). Le regole sono verificate interamente dal **compilatore**,
prima ancora che il programma venga eseguito.

Tre regole fondamentali:

1. Ogni valore ha **un solo proprietario** (owner) alla volta.
2. Quando il proprietario esce dallo scope, il valore viene
   **automaticamente liberato**.
3. La proprietà può essere **trasferita** (move) ma non duplicata
   implicitamente.

---

## Move: il trasferimento di proprietà

Per i tipi allocati sull'heap (come `String`), assegnare una variabile
a un'altra **sposta** la proprietà: la variabile originale non è più
utilizzabile.

#### Esercizio 1
Copia il seguente codice nell'editor e osserva l'errore del compilatore.

{% highlight rust %}
fn main() {
    let s1 = String::from("ciao");
    let s2 = s1;  // la proprietà si sposta da s1 a s2

    println!("{}", s2);  // funziona
    // println!("{}", s1);  // ERRORE: value borrowed after move
}
{% endhighlight %}

A differenza di C++, Rust **non copia** automaticamente `s1` in `s2`:
sposta semplicemente il possesso. Usare `s1` dopo il move è un errore
di **compilazione**, non un bug scoperto a runtime.

---

## clone(): copiare esplicitamente

Se serve davvero una copia indipendente, si chiama `.clone()`
esplicitamente.

#### Esercizio 2
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
fn main() {
    let s1 = String::from("ciao");
    let s2 = s1.clone();  // copia indipendente dei dati

    println!("{}", s1);  // ancora valido
    println!("{}", s2);
}
{% endhighlight %}

I tipi semplici come `i32`, `f64`, `bool`, `char` implementano il trait
`Copy`: per loro l'assegnazione **copia** il valore invece di spostarlo,
quindi non c'è bisogno di `.clone()`.

---

## Ownership e funzioni

Passare una variabile a una funzione **sposta** la proprietà, esattamente
come un'assegnazione.

#### Esercizio 3
Copia il seguente codice nell'editor e osserva l'errore.

{% highlight rust %}
fn stampa(s: String) {
    println!("{}", s);
}  // s esce dallo scope: la memoria viene liberata qui

fn main() {
    let messaggio = String::from("ciao");
    stampa(messaggio);  // la proprietà si sposta nella funzione

    // println!("{}", messaggio);  // ERRORE: messaggio non è più valido
}
{% endhighlight %}

---

## Borrowing: prendere in prestito senza possedere

Per usare un valore senza trasferirne la proprietà, si passa un
**riferimento** con `&`: si dice che la funzione "prende in prestito"
(*borrow*) il valore.

#### Esercizio 4
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
fn calcola_lunghezza(s: &String) -> usize {
    s.len()
}  // s esce dallo scope, ma non possiede i dati: non li libera

fn main() {
    let messaggio = String::from("ciao");
    let lunghezza = calcola_lunghezza(&messaggio);

    println!("'{}' ha lunghezza {}", messaggio, lunghezza);  // messaggio è ancora valido
}
{% endhighlight %}

`&messaggio` crea un riferimento **immutabile**: la funzione può
leggere il valore ma non modificarlo, e il chiamante mantiene la
proprietà.

---

## Riferimenti mutabili

Per modificare un valore preso in prestito serve un riferimento
**mutabile** (`&mut`). Regola fondamentale: **un solo riferimento
mutabile alla volta**, e nessun riferimento immutabile mentre esiste
uno mutabile. Questa regola, verificata a compilazione, previene le
*race condition* alla radice.

#### Esercizio 5
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
fn aggiungi_saluto(s: &mut String) {
    s.push_str(", mondo!");
}

fn main() {
    let mut messaggio = String::from("Ciao");
    aggiungi_saluto(&mut messaggio);

    println!("{}", messaggio);  // Ciao, mondo!
}
{% endhighlight %}

#### Esercizio 6
Copia il seguente codice nell'editor e osserva l'errore.

{% highlight rust %}
fn main() {
    let mut x = String::from("valore");

    let r1 = &mut x;
    let r2 = &mut x;  // ERRORE: secondo prestito mutabile mentre r1 è ancora attivo

    println!("{}, {}", r1, r2);
}
{% endhighlight %}

Questo codice **non compila**: è esattamente il tipo di errore che in
C potrebbe causare un bug difficile da riprodurre (due parti di codice
che modificano lo stesso dato "in contemporanea"). In Rust diventa un
errore di compilazione, non un bug in produzione.

---

## Riepilogo: move vs borrow

| Operazione            | Chi possiede il dato dopo?           |
|-------------------------|----------------------------------------|
| `let s2 = s1;`          | `s2` (per tipi come `String`); `s1` non è più valido |
| `let s2 = s1.clone();`  | entrambi, con dati indipendenti        |
| `funzione(s1)`          | la funzione, se il parametro è `String` |
| `funzione(&s1)`         | `s1`; la funzione legge soltanto        |
| `funzione(&mut s1)`     | `s1`; la funzione può modificare        |

---

## Esercizi

#### Esercizio 7
Scrivi una funzione `fn lunghezza(s: &String) -> usize` che restituisca
la lunghezza di una stringa senza prenderne la proprietà. Chiamala e
stampa sia la stringa che la lunghezza dopo la chiamata.

#### Esercizio 8
Scrivi una funzione `fn maiuscolo(s: &mut String)` che trasformi in
maiuscolo il contenuto della stringa passata per riferimento mutabile
(suggerimento: `*s = s.to_uppercase();`).

#### Esercizio 9
Scrivi una funzione `fn raddoppia(v: &mut Vec<i32>)` che raddoppi ogni
elemento di un vettore passato per riferimento mutabile.

#### Esercizio 10
Prova a scrivere una funzione che accetta due riferimenti mutabili allo
stesso vettore contemporaneamente e osserva l'errore del compilatore.
Spiega, con parole tue, quale problema questa regola previene.

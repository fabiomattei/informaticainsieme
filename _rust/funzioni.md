---
title: 'Rust: le funzioni'
date: '2026-08-23T00:00:00+01:00'
author: Fabio Mattei
layout: page
---

## Dividere il programma in blocchi riutilizzabili

Una **funzione** è un blocco di codice con un nome che può essere
chiamato più volte. In Rust ogni funzione deve dichiarare il **tipo di
ogni parametro**; il tipo restituito si dichiara dopo `->`.

```
fn nome(param1: tipo1, param2: tipo2) -> tipo_ritorno {
    // corpo
    valore_di_ritorno  // senza punto e virgola: è l'ultima espressione
}
```

Se la funzione non restituisce nulla si omette del tutto `-> tipo`.

---

## Definire e chiamare una funzione

#### Esercizio 1
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
fn somma(a: i32, b: i32) -> i32 {
    a + b  // ultima espressione senza ; = valore restituito
}

fn main() {
    let risultato = somma(3, 7);
    println!("3 + 7 = {}", risultato);
    println!("10 + 5 = {}", somma(10, 5));
}
{% endhighlight %}

In Rust, a differenza del C, l'ordine di definizione **non conta**: una
funzione può essere chiamata anche se definita più in basso nel file,
senza bisogno di un prototipo separato.

---

## return esplicito

`return` è disponibile e serve soprattutto per uscire **anticipatamente**
da una funzione.

#### Esercizio 2
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
fn valore_assoluto(n: i32) -> i32 {
    if n < 0 {
        return -n;  // uscita anticipata
    }
    n  // valore restituito nel caso normale
}

fn main() {
    println!("{}", valore_assoluto(-7));  // 7
    println!("{}", valore_assoluto(3));   // 3
}
{% endhighlight %}

---

## Funzioni senza valore di ritorno

Una funzione senza `-> tipo` restituisce implicitamente `()` (la
"tupla vuota", equivalente concettualmente a `void` in C).

#### Esercizio 3
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
fn saluta(nome: &str) {
    println!("Ciao, {}!", nome);
}

fn main() {
    saluta("Alice");
    saluta("Bob");
}
{% endhighlight %}

Il parametro è `&str` (un riferimento a una porzione di testo), non
`String`: la funzione legge il nome ma non ne prende la proprietà.

---

## Passaggio per valore e per riferimento

Passare un parametro **per valore** trasferisce la proprietà (per i
tipi come `String` o `Vec`) o la copia (per i tipi semplici come
`i32`). Con `&` si passa **per riferimento**, senza cedere la
proprietà. L'argomento è approfondito nella pagina su
[ownership e borrowing]({{ site.baseurl }}{% link _rust/ownership.md %}.html).

#### Esercizio 4
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
fn raddoppia_valore(mut x: i32) {
    x *= 2;  // modifica solo la copia locale
}

fn raddoppia_riferimento(x: &mut i32) {
    *x *= 2;  // modifica la variabile originale
}

fn main() {
    let mut a = 5;
    raddoppia_valore(a);
    println!("{}", a);  // 5 (invariato: i32 implementa Copy)

    raddoppia_riferimento(&mut a);
    println!("{}", a);  // 10 (modificato)
}
{% endhighlight %}

---

## Nessun overloading

A differenza del C++, Rust **non supporta l'overloading**: non possono
esistere due funzioni con lo stesso nome, anche con parametri diversi.
Per comportamenti simili su tipi diversi si usano i **generics** (un
argomento più avanzato) o semplicemente nomi diversi.

---

## Funzioni ricorsive

Una funzione può chiamare **se stessa**: si parla di ricorsione. Serve
sempre un **caso base** che interrompe le chiamate.

#### Esercizio 5
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
fn fattoriale(n: u64) -> u64 {
    if n <= 1 {
        return 1;  // caso base
    }
    n * fattoriale(n - 1)  // chiamata ricorsiva
}

fn main() {
    println!("{}", fattoriale(5));   // 120
    println!("{}", fattoriale(0));   // 1
}
{% endhighlight %}

---

## Esercizi

#### Esercizio 6
Scrivi una funzione `fn fattoriale_iterativo(n: u64) -> u64` con un
ciclo (non ricorsiva) che restituisca n!. Chiamala per n = 0, 5, 10.

#### Esercizio 7
Scrivi una funzione `fn is_primo(n: u32) -> bool` che restituisca
`true` se n è primo. Stampa tutti i numeri primi fino a 50.

#### Esercizio 8
Scrivi una funzione `fn scambia(a: &mut i32, b: &mut i32)` che scambi i
valori di due interi passati per riferimento mutabile.

#### Esercizio 9
Scrivi una funzione ricorsiva `fn fibonacci(n: u32) -> u64` che calcoli
l'n-esimo termine della successione di Fibonacci.

#### Esercizio 10
Scrivi una funzione `fn stampa_riga(n: u32, c: char)` che stampi n volte
il carattere c.

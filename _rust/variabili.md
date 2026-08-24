---
title: 'Rust: le variabili'
date: '2026-08-23T00:00:00+01:00'
author: Fabio Mattei
layout: page
---

## Immutabili di default

In Rust ogni variabile dichiarata con `let` è **immutabile di default**:
una volta assegnato un valore, non può essere cambiato. Per poterla
modificare va dichiarata esplicitamente con `mut`.

```
let nome = valore;        // immutabile
let mut nome = valore;    // mutabile
```

Questa scelta di design costringe a dichiarare intenzionalmente quali
variabili possono cambiare, riducendo gli errori causati da modifiche
non previste.

---

## Dichiarare una variabile immutabile

#### Esercizio 1
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
fn main() {
    let eta = 25;
    let altezza = 1.75;
    let attivo = true;

    println!("Età: {}", eta);
    println!("Altezza: {}", altezza);
    println!("Attivo: {}", attivo);
}
{% endhighlight %}

Il tipo di `eta`, `altezza` e `attivo` viene **dedotto** dal compilatore
in base al valore assegnato: rispettivamente `i32`, `f64` e `bool`.

---

## Variabili mutabili

#### Esercizio 2
Copia il seguente codice nell'editor e prova a rimuovere `mut`: osserva
l'errore del compilatore.

{% highlight rust %}
fn main() {
    let mut punti = 10;
    println!("Inizio: {}", punti);

    punti += 5;
    println!("Dopo +=5: {}", punti);

    punti *= 2;
    println!("Dopo *=2: {}", punti);

    punti -= 3;
    println!("Dopo -=3: {}", punti);
}
{% endhighlight %}

Senza `mut`, la riga `punti += 5;` non compila: il compilatore segnala
"cannot assign twice to immutable variable".

---

## Shadowing

Rust permette di dichiarare di nuovo una variabile con lo **stesso
nome**, anche con un tipo diverso: la nuova dichiarazione "nasconde"
(shadow) la precedente. Non è la stessa cosa di `mut`: si tratta di una
variabile completamente nuova.

#### Esercizio 3
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
fn main() {
    let x = 5;
    println!("{}", x);       // 5

    let x = x + 1;            // nuova variabile x, ombreggia la precedente
    println!("{}", x);       // 6

    let x = "adesso sono una stringa";  // può anche cambiare tipo
    println!("{}", x);
}
{% endhighlight %}

A differenza di `mut`, lo shadowing crea una nuova variabile: è utile
per trasformare un valore mantenendo lo stesso nome logico, senza
renderlo mutabile.

---

## Costanti

Una costante si dichiara con `const`. Va sempre annotata con il tipo
esplicito e può essere dichiarata anche fuori da una funzione.

#### Esercizio 4
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
const IVA: f64 = 0.22;

fn main() {
    let raggio: f64 = 5.0;
    let pi: f64 = 3.14159;
    let area = pi * raggio * raggio;

    println!("Area del cerchio: {:.2}", area);
    println!("IVA: {}", IVA);
}
{% endhighlight %}

A differenza di `let`, una `const` **non può mai** essere resa mutabile
con `mut` e deve essere calcolabile a tempo di compilazione.

---

## Traccia di esecuzione

Per il codice seguente, segui il valore di `x` passo per passo.

{% highlight rust %}
let mut x = 4;
x += 3;
x *= 2;
x -= 5;
{% endhighlight %}

| Istruzione   | x    |
|--------------|-----:|
| `let mut x = 4` | 4 |
| `x += 3`     | 7    |
| `x *= 2`     | 14   |
| `x -= 5`     | 9    |

---

## Esercizi

#### Esercizio 5
Dichiara una variabile immutabile `base` e una `altezza`, inizializzale
con valori a scelta, calcola e stampa `area = base * altezza`.

#### Esercizio 6
Dichiara una variabile mutabile `contatore = 0`. Incrementala di 1 per
cinque volte usando `+=` e stampa il valore finale.

#### Esercizio 7
Dichiara due variabili mutabili e scambia i loro valori usando una terza
variabile temporanea. Stampa i valori prima e dopo lo scambio.

#### Esercizio 8
Dichiara una costante `IVA` con valore `0.22`. Leggi un prezzo netto da
input e stampa il prezzo lordo (prezzo * (1.0 + IVA)).

#### Esercizio 9
Dichiara le variabili `a = 10` e `b = 3`. Stampa il risultato di `a / b`
(divisione intera tra interi) e di `a % b` (resto). Spiega il risultato.

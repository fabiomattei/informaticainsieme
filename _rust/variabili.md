---
title: 'Rust: le variabili'
date: '2026-08-23T00:00:00+01:00'
author: Fabio Mattei
layout: page
---

## Immutabili di default

Il concetto di variabile è un concetto importante in ogni linguaggio di programmazione;
solitamente la si definisce come un contenitore di informazione che può cambiare nel tempo.
In Rust le cose cambiano, le variabili in effetti non sono più **variabili** sono in un certo
senso delle costanti: una volta dichiarate e contenenti un valore non possono cambiare.
E' per questo che ogni variabile dichiarata con `let` si dice **immutabile di default**:
una volta assegnato un valore, non può essere cambiato. 

Si possono però creare delle variabili che possono mutare e a questo proposito si usa la parola
chiave **mut**.

```
let nome = valore;        // immutabile
let mut nome = valore;    // mutabile
```

Questa scelta di design costringe a dichiarare intenzionalmente quali
variabili possono cambiare, riducendo gli errori causati da modifiche
non previste.

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

Se guardiamo bene notiamo che in queste dichiarazioni di variabile, diversamente da
lunguaggi come C o C++, non abbiamo dovuto esplicitare il tipo delle variabili dichiarate.
Questo in Rust è facoltativo, il tipo della variabile può essere dichiarato oppure **dedotto** 
dal compilatore in base al valore assegnato.

Nell'esempio in alto i tipi sono rispettivamente rispettivamente `i32`, `f64` e `bool`.

## Variabili mutabili

Approfondiamo il concetto di variabile **mutabile** questo si avvicina di più al concetto
di variable che è proprio di lunguaggi come C, C++, Python, Ruby ecc.

Una variabile mutabile è un contenitore, di un certo tipo di dato, il cui contenuto
può essere assegnato non solo alla sua creazione ma più di una volta durante l'esecuzione di
un programma.

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

## Shadowing

Questo è un concetto particolare, non esistente negli altri lunguaggi.

Rust permette di dichiarare di nuovo una variabile già dichiara in precedenza, 
una variabile cioè, avente **lo stesso nome** di una variabile che già esiste.
La nuova variabile può avere anche con un tipo diverso.
La nuova dichiarazione **nasconde**, pone in ombra (shadow), la precedente. 
Non è la stessa cosa di `mut`: si tratta di una variabile completamente nuova che va 
a nascondere quella vecchia la quale da quel momento in poi è raggiungibile.

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

A differenza di `mut`, lo shadowing crea una nuova variabile.
Questo comportamento può utile per trasformare un valore mantenendo 
lo stesso nome logico, senza renderlo mutabile.

## Costanti

Una costante si dichiara con `const`. Questo concetto è analogo al concetto di
costante degli altri linguaggi. Va sempre annotata con il tipo
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
con `mut` e deve essere calcolabile a **tempo di compilazione**.

## Tabella di tracciamento

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

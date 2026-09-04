---
title: 'Rust: gli array'
date: '2026-08-23T00:00:00+01:00'
author: Fabio Mattei
layout: page
---

## Che cos'è un array

Un **array** in Rust è una collezione di elementi **dello stesso
tipo**, con una **dimensione fissa** decisa a tempo di compilazione.
Il suo tipo si scrive `[T; N]`, dove `T` è il tipo degli elementi e `N`
è il numero di elementi (una costante nota già durante la
compilazione).

Caratteristiche principali:

- la dimensione **non può cambiare** dopo la creazione (non esiste un
  equivalente di `.push()` per un array);
- viene allocato di norma sullo **stack**, non sull'heap: per questo è
  molto veloce da creare e da distruggere;
- proprio perché la dimensione è fissa e nota a compile-time, il
  compilatore può controllare molte cose in anticipo e generare codice
  molto efficiente.

Se invece la dimensione di una collezione deve poter cambiare durante
l'esecuzione del programma, la struttura giusta è `Vec<T>`, descritta
nella pagina [Vettori (Vec\<T\>)]({{ site.baseurl }}{% link _rust/vec.md %}.html).

---

## Dichiarazione e inizializzazione

Un array si dichiara indicando tipo degli elementi e dimensione tra
parentesi quadre, seguiti dai valori tra parentesi graffe... in
realtà tra parentesi quadre anche per i valori:

{% highlight rust %}
let numeri: [i32; 5] = [10, 20, 30, 40, 50];
{% endhighlight %}

Il tipo `[i32; 5]` si legge: "un array di 5 elementi di tipo `i32`".
Spesso Rust riesce a **inferire** tipo e dimensione dal valore
assegnato, quindi l'annotazione esplicita è opzionale:

{% highlight rust %}
let numeri = [10, 20, 30, 40, 50];  // dedotto: [i32; 5]
{% endhighlight %}

Esiste anche una sintassi comoda per creare un array in cui **tutti
gli elementi hanno lo stesso valore iniziale**:

{% highlight rust %}
let zeri = [0; 5];       // equivale a [0, 0, 0, 0, 0]
let piene = ['x'; 3];    // equivale a ['x', 'x', 'x']
{% endhighlight %}

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

Gli indici partono da 0, come in quasi tutti i linguaggi. Un array è
dichiarato `mut` quando si vuole poter **modificare il valore di un
elemento** (`numeri[2] = 99`); questo non cambia comunque la sua
dimensione, che resta sempre 5.

---

## Accesso fuori dai limiti: panic controllato

A differenza del C, dove leggere o scrivere fuori dai limiti di un
array è un **comportamento indefinito** (può funzionare per caso,
corrompere memoria, o crashare in modo imprevedibile), in Rust
l'operatore `[]` controlla sempre l'indice a runtime: se è fuori dai
limiti il programma termina subito con un **panic** e un messaggio
chiaro, invece di continuare con dati sbagliati.

#### Esercizio 2
Copia il seguente codice nell'editor e osserva l'errore a runtime.

{% highlight rust %}
fn main() {
    let numeri = [10, 20, 30, 40, 50];
    println!("{}", numeri[10]);  // panic: index out of bounds
}
{% endhighlight %}

Il messaggio indica esattamente indice richiesto e lunghezza
dell'array (`index out of bounds: the len is 5 but the index is 10`),
il che rende il bug immediato da individuare, invece di doverlo
scovare a posteriori come spesso capita in C.

Se si vuole evitare il panic e gestire il caso "indice non valido"
come un valore normale, si usa `.get(i)`, che restituisce un
`Option<&T>` invece di andare in panic (lo stesso metodo esiste anche
per `Vec<T>`, vedi la pagina dedicata):

{% highlight rust %}
fn main() {
    let numeri = [10, 20, 30, 40, 50];

    match numeri.get(10) {
        Some(v) => println!("Trovato: {}", v),
        None => println!("Indice non valido"),  // questo viene stampato
    }
}
{% endhighlight %}

---

## Array come valore: la dimensione fa parte del tipo

Una conseguenza importante di avere la dimensione fissata a
compile-time è che `[i32; 5]` e `[i32; 10]` sono **due tipi
diversi**, non intercambiabili. Una funzione che accetta un array di
5 elementi non può ricevere un array di 10 elementi:

{% highlight rust %}
fn somma5(v: [i32; 5]) -> i32 {
    let mut totale = 0;
    for n in v {
        totale += n;
    }
    totale
}

fn main() {
    let a = [1, 2, 3, 4, 5];
    println!("{}", somma5(a));  // ok, ha esattamente 5 elementi

    let b = [1, 2, 3];
    // println!("{}", somma5(b));  // ERRORE: tipi diversi, [i32; 3] != [i32; 5]
}
{% endhighlight %}

Questo è un limite reale degli array: se serve una funzione che
accetti array (o vettori) di **qualsiasi lunghezza**, si usa una
**slice** (vedi sotto) oppure, quando la dimensione può davvero
variare durante l'esecuzione, si passa a `Vec<T>`.

---

## Slice: una "vista" su parte di un array

Una **slice** (`&[T]`) è un riferimento a una sequenza contigua di
elementi, senza possederli: non ha una dimensione fissata nel tipo,
quindi funziona con array di qualunque lunghezza (e anche con `Vec<T>`,
come vedremo nella pagina dedicata).

#### Esercizio 3
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
fn somma(v: &[i32]) -> i32 {
    let mut totale = 0;
    for n in v {
        totale += n;
    }
    totale
}

fn main() {
    let numeri = [1, 2, 3, 4, 5];

    println!("{}", somma(&numeri));       // tutto l'array: 15
    println!("{}", somma(&numeri[1..3])); // solo indici 1 e 2: 5
    println!("{}", somma(&numeri[..2]));  // dall'inizio a indice 2 escluso: 3
    println!("{}", somma(&numeri[3..]));  // da indice 3 alla fine: 9
}
{% endhighlight %}

Grazie alle slice, `somma` funziona con array di qualsiasi lunghezza
(3, 5, 100...), a differenza della versione con `[i32; 5]` vista
sopra. Per questo motivo le funzioni che lavorano su array in Rust
**preferiscono quasi sempre** un parametro `&[T]` a un parametro
`[T; N]`.

---

## Metodi utili degli array

Gli array offrono diversi metodi comodi, ereditati dal fatto che
Rust li tratta come slice quando serve:

| Metodo          | Significato                                    |
|-----------------|-------------------------------------------------|
| `.len()`        | numero di elementi                              |
| `.is_empty()`   | `true` se l'array ha 0 elementi                 |
| `.get(i)`       | accesso sicuro (`Option<&T>`), niente panic     |
| `.contains(&x)` | `true` se `x` è presente                        |
| `.iter()`       | iteratore sui riferimenti agli elementi         |
| `.reverse()`    | inverte l'ordine degli elementi, in-place       |
| `.sort()`       | ordina in-place (richiede array `mut`)          |

#### Esercizio 4
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
fn main() {
    let mut numeri = [5, 2, 8, 1, 9];

    println!("Lunghezza: {}", numeri.len());
    println!("Contiene 8? {}", numeri.contains(&8));

    numeri.sort();
    println!("{:?}", numeri);

    numeri.reverse();
    println!("{:?}", numeri);
}
{% endhighlight %}

---

## Array a più dimensioni

Un array può contenere altri array, ottenendo così una griglia a più
dimensioni, utile per rappresentare matrici, tabelle o mappe di
gioco.

#### Esercizio 5
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
fn main() {
    // matrice 2 righe x 3 colonne
    let matrice: [[i32; 3]; 2] = [
        [1, 2, 3],
        [4, 5, 6],
    ];

    for riga in matrice {
        for valore in riga {
            print!("{} ", valore);
        }
        println!();
    }

    println!("Elemento riga 1, colonna 2: {}", matrice[1][2]);  // 6
}
{% endhighlight %}

Il tipo `[[i32; 3]; 2]` si legge dall'interno verso l'esterno: "un
array di 2 elementi, ciascuno dei quali è un array di 3 `i32`".

---

## Quando usare un array (e quando no)

| Situazione                                              | Struttura consigliata |
|-----------------------------------------------------------|------------------------|
| La dimensione è nota fin dall'inizio e non cambia mai      | `[T; N]` (array)       |
| Servono le massime prestazioni, senza allocazione sull'heap | `[T; N]` (array)       |
| La dimensione dipende da un input dell'utente o cambia durante l'esecuzione | `Vec<T>`               |
| Servono elementi aggiunti o rimossi dinamicamente          | `Vec<T>`               |

Nella pratica, gli array si usano soprattutto quando la dimensione è
davvero fissa "per natura" (coordinate `[f64; 3]`, giorni della
settimana `[&str; 7]`...); nella maggior parte degli altri casi
`Vec<T>` è la scelta più comoda e flessibile: se vuoi approfondire,
vai alla pagina [Vettori (Vec\<T\>)]({{ site.baseurl }}{% link _rust/vec.md %}.html).

---

## Esercizi

#### Esercizio 6
Dichiara un array `[i32; 6]` con dei valori a piacere e scrivi una
funzione `fn media(v: &[i32]) -> f64` che calcoli la media aritmetica
usando una slice (in modo che funzioni con array di qualsiasi
lunghezza).

#### Esercizio 7
Scrivi un programma che dichiari un array `[i32; 10]` inizializzato
tutto a 0 con la sintassi `[0; 10]`, poi lo riempia con i quadrati
dei numeri da 1 a 10 (`array[i] = (i+1) * (i+1)`) e infine lo stampi.

#### Esercizio 8
Usando l'array `let voti = [6, 8, 5, 9, 7, 4, 10];`, scrivi una
funzione `fn massimo(v: &[i32]) -> i32` che restituisca il voto più
alto, e una `fn insufficienti(v: &[i32]) -> usize` che conti quanti
voti sono minori di 6.

#### Esercizio 9
Crea una matrice `[[i32; 3]; 3]` (3x3) e scrivi un programma che
calcoli la somma degli elementi sulla diagonale principale
(`matrice[0][0] + matrice[1][1] + matrice[2][2]`).

#### Esercizio 10
Prova ad accedere con `[]` a un indice fuori dai limiti di un array e
osserva il messaggio di panic. Poi riscrivi lo stesso accesso usando
`.get(i)` e un `match`, in modo da stampare `"Indice non valido"`
invece di far terminare il programma.

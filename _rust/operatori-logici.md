---
title: 'Rust: gli operatori logici'
date: '2026-08-23T00:00:00+01:00'
author: Fabio Mattei
layout: page
---

## Combinare condizioni

Gli operatori logici combinano espressioni booleane per formare
condizioni più complesse. In Rust i tre operatori logici sono `&&`
(AND), `||` (OR) e `!` (NOT). A differenza del C, in Rust queste
espressioni devono essere **sempre** di tipo `bool`: non esiste
conversione implicita da numeri a booleani.

---

## L'operatore && (AND)

Il risultato è `true` solo se **entrambe** le condizioni sono vere.

| A       | B       | A && B  |
|---------|---------|---------|
| `true`  | `true`  | `true`  |
| `true`  | `false` | `false` |
| `false` | `true`  | `false` |
| `false` | `false` | `false` |

#### Esercizio 1
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
use std::io;

fn main() {
    let mut input = String::new();
    println!("Inserisci la tua età: ");
    io::stdin().read_line(&mut input).expect("Errore");
    let eta: i32 = input.trim().parse().expect("Non è un numero");

    if eta >= 18 && eta <= 65 {
        println!("Sei in età lavorativa.");
    } else {
        println!("Non sei in età lavorativa.");
    }
}
{% endhighlight %}

---

## L'operatore || (OR)

Il risultato è `true` se **almeno una** delle condizioni è vera.

| A       | B       | A \|\| B |
|---------|---------|----------|
| `true`  | `true`  | `true`   |
| `true`  | `false` | `true`   |
| `false` | `true`  | `true`   |
| `false` | `false` | `false`  |

#### Esercizio 2
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
use std::io;

fn main() {
    let mut input = String::new();
    println!("Inserisci il voto: ");
    io::stdin().read_line(&mut input).expect("Errore");
    let voto: i32 = input.trim().parse().expect("Non è un numero");

    if voto < 4 || voto > 10 {
        println!("Voto non valido.");
    } else {
        println!("Voto valido.");
    }
}
{% endhighlight %}

---

## L'operatore ! (NOT)

Inverte il valore booleano: `!true` = `false`, `!false` = `true`.

| A       | !A      |
|---------|---------|
| `true`  | `false` |
| `false` | `true`  |

#### Esercizio 3
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
fn main() {
    let autenticato = false;

    if !autenticato {
        println!("Accesso negato. Effettua il login.");
    } else {
        println!("Benvenuto!");
    }
}
{% endhighlight %}

---

## Valutazione cortocircuitata (short-circuit)

Rust valuta le condizioni da sinistra a destra e **si ferma appena il
risultato è determinato**:

- `A && B`: se `A` è `false`, `B` non viene valutato (il risultato è già `false`).
- `A || B`: se `A` è `true`, `B` non viene valutato (il risultato è già `true`).

#### Esercizio 4
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
fn main() {
    let n = 0;

    // la seconda condizione non viene valutata se n == 0
    if n != 0 && 100 / n > 5 {
        println!("Condizione vera");
    } else {
        println!("Condizione falsa (divisione per zero evitata)");
    }
}
{% endhighlight %}

---

## && vs & e || vs |

In Rust esistono anche gli operatori bitwise `&` e `|`. Non sono
operatori logici booleani: operano bit a bit sui valori interi (o, se
applicati a due `bool`, valutano **entrambi** gli operandi senza
cortocircuito). Usa sempre `&&` e `||` per le condizioni logiche.

---

## Esercizi

#### Esercizio 5
Leggi un anno e scrivi se è bisestile. Un anno è bisestile se è
divisibile per 4, tranne i centenari, che devono essere divisibili per
400. Condizione: `(anno % 4 == 0 && anno % 100 != 0) || anno % 400 == 0`.

#### Esercizio 6
Leggi un carattere e scrivi se è una lettera minuscola (tra `'a'` e
`'z'`). Usa `&&` per combinare i due confronti.

#### Esercizio 7
Leggi tre numeri interi. Stampa `true` se almeno uno di essi è negativo.
Usa `||`.

#### Esercizio 8
Leggi un numero intero e stampa `true` se **non** è divisibile né per 2
né per 3. Usa `!` e `||`.

#### Esercizio 9
Scrivi un programma che legga username e password (stringhe) e stampi
"Accesso consentito" solo se username è "admin" e password è "1234".
Usa `&&` e l'operatore `==` su `&str`.

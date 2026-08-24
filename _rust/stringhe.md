---
title: 'Rust: le stringhe di testo'
date: '2026-08-23T00:00:00+01:00'
author: Fabio Mattei
layout: page
---

## String vs &str

Rust ha due tipi principali per il testo:

- **`&str`** — un **riferimento** a una sequenza di testo UTF-8, di
  dimensione fissa e non modificabile direttamente. I letterali stringa
  (`"ciao"`) sono sempre di tipo `&str`.
- **`String`** — un tipo **di proprietà** (owned), modificabile,
  allocato sull'heap. Si crea con `String::from(...)` o `.to_string()`.

La differenza è legata al sistema di [ownership]({{ site.baseurl }}{% link _rust/ownership.md %}.html):
`String` possiede i propri dati, `&str` li prende in prestito da
qualcun altro.

---

## Creare stringhe

#### Esercizio 1
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
fn main() {
    let letterale: &str = "ciao";              // &str
    let posseduta: String = String::from("ciao"); // String
    let convertita: String = letterale.to_string(); // &str -> String

    println!("{}", letterale);
    println!("{}", posseduta);
    println!("{}", convertita);
}
{% endhighlight %}

---

## Concatenare stringhe

#### Esercizio 2
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
fn main() {
    let saluto = String::from("Ciao");
    let nome = "Alice";

    // + richiede una String a sinistra e un &str a destra
    let frase = saluto + ", " + nome + "!";

    println!("{}", frase);
    println!("Lunghezza: {}", frase.len());
}
{% endhighlight %}

L'operatore `+` **consuma** la `String` a sinistra (per via
dell'ownership): dopo questa riga `saluto` non è più utilizzabile. Per
concatenare senza consumare si usa `format!`:

{% highlight rust %}
let frase = format!("{}, {}!", saluto, nome);
{% endhighlight %}

---

## Accedere ai caratteri

A differenza del C, in Rust **non si può** indicizzare una stringa con
`s[0]`: una `String` è UTF-8, e un singolo byte non corrisponde sempre a
un carattere. Si scorre invece con `.chars()`.

#### Esercizio 3
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
fn main() {
    let parola = "liceo";

    let primo = parola.chars().next().unwrap();
    let ultimo = parola.chars().last().unwrap();
    println!("{}", primo);   // l
    println!("{}", ultimo);  // o

    // visita carattere per carattere
    for c in parola.chars() {
        print!("{} ", c);
    }
    println!();
}
{% endhighlight %}

`.unwrap()` estrae il valore da un `Option`, facendo *panic* se è
`None` (qui non succede: la stringa non è vuota).

---

## Ricerca e sostituzione

#### Esercizio 4
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
fn main() {
    let s = "il gatto sul tetto";

    // ricerca
    if let Some(pos) = s.find("gatto") {
        println!("Trovato a posizione: {}", pos);  // 3
    }

    // sottostringa (per indice di byte)
    let sub = &s[3..8];
    println!("{}", sub);  // gatto

    // sostituzione (restituisce una nuova String)
    let sostituita = s.replace("gatto", "cane");
    println!("{}", sostituita);  // il cane sul tetto
}
{% endhighlight %}

`s.replace(...)` non modifica `s`: restituisce una **nuova** stringa,
perché `s` qui è un `&str` immutabile.

---

## Maiuscolo, minuscolo e confronto

#### Esercizio 5
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
fn main() {
    let s = "Ciao Mondo";

    println!("{}", s.to_uppercase());  // CIAO MONDO
    println!("{}", s.to_lowercase());  // ciao mondo

    // confronto lessicografico
    println!("{}", "abc" < "abd");   // true
    println!("{}", "abc" == "abc");  // true
}
{% endhighlight %}

---

## Metodi utili

| Metodo               | Significato                                       |
|-----------------------|---------------------------------------------------|
| `.len()`              | numero di byte (non sempre = numero di caratteri) |
| `.chars().count()`    | numero di caratteri Unicode                        |
| `.is_empty()`         | `true` se la stringa è vuota                      |
| `.trim()`             | rimuove spazi bianchi a inizio/fine               |
| `.find(s)`            | posizione della prima occorrenza (`Option<usize>`) |
| `.replace(a, b)`      | sostituisce tutte le occorrenze di `a` con `b`    |
| `.split(sep)`         | divide la stringa in base a un separatore          |
| `.to_uppercase()` / `.to_lowercase()` | maiuscolo / minuscolo             |

---

## Esercizi

#### Esercizio 6
Leggi una stringa e stampa quante volte appare la lettera 'a'
(maiuscola o minuscola) nella stringa.

#### Esercizio 7
Leggi una parola e stampa se è un palindromo (uguale letta al
contrario). Suggerimento: confronta `s` con `s.chars().rev().collect::<String>()`.

#### Esercizio 8
Leggi una frase e stampa quante parole contiene, usando
`.split_whitespace().count()`.

#### Esercizio 9
Leggi una stringa e stampa separatamente le consonanti e le vocali che
contiene.

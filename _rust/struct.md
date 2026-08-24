---
title: 'Rust: le struct'
date: '2026-08-23T00:00:00+01:00'
author: Fabio Mattei
layout: page
---

## Raggruppare dati correlati

Una **struct** (struttura) raggruppa più campi di tipi diversi sotto un
unico nome. È utile quando si vuole trattare un insieme di dati
correlati come una singola entità — ad esempio le coordinate di un
punto, i dati di uno studente, le proprietà di un prodotto.

---

## Definire e usare una struct

#### Esercizio 1
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
struct Punto {
    x: f64,
    y: f64,
}

fn main() {
    let p1 = Punto { x: 3.0, y: 4.0 };

    println!("p1: ({}, {})", p1.x, p1.y);
}
{% endhighlight %}

I campi di una struct si accedono con il punto `.`. Per modificare un
campo, l'istanza intera deve essere dichiarata `mut`: non esistono campi
mutabili singolarmente.

---

## Struct con più campi

#### Esercizio 2
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
struct Persona {
    nome: String,
    cognome: String,
    eta: u32,
}

fn main() {
    let mut mario = Persona {
        nome: String::from("Mario"),
        cognome: String::from("Rossi"),
        eta: 25,
    };

    println!("{} {}, {} anni", mario.nome, mario.cognome, mario.eta);

    mario.eta = 26;
    println!("Nuova età: {}", mario.eta);
}
{% endhighlight %}

---

## Aggiungere metodi con impl

I metodi di una struct si definiscono in un blocco separato `impl`,
non dentro la struct come in C++ o Java. Il primo parametro `&self` (o
`&mut self`) rappresenta l'istanza su cui il metodo viene chiamato.

#### Esercizio 3
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
struct Punto {
    x: f64,
    y: f64,
}

impl Punto {
    fn distanza_dall_origine(&self) -> f64 {
        (self.x * self.x + self.y * self.y).sqrt()
    }

    fn stampa(&self) {
        println!("({}, {})", self.x, self.y);
    }
}

fn main() {
    let p = Punto { x: 3.0, y: 4.0 };
    p.stampa();
    println!("Distanza: {}", p.distanza_dall_origine());  // 5.0
}
{% endhighlight %}

`&self` prende in prestito l'istanza senza consumarla (vedi
[ownership e borrowing]({{ site.baseurl }}{% link _rust/ownership.md %}.html)):
il metodo può leggere i campi ma non modificarli. Per modificare serve
`&mut self`.

---

## Funzioni associate: new come costruttore

Una funzione dentro `impl` che **non** prende `self` come primo
parametro è una **funzione associata**, spesso usata come costruttore
convenzionale chiamato `new`.

#### Esercizio 4
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
struct Rettangolo {
    base: f64,
    altezza: f64,
}

impl Rettangolo {
    fn new(base: f64, altezza: f64) -> Rettangolo {
        Rettangolo { base, altezza }  // shorthand: base: base, altezza: altezza
    }

    fn area(&self) -> f64 {
        self.base * self.altezza
    }

    fn perimetro(&self) -> f64 {
        2.0 * (self.base + self.altezza)
    }
}

fn main() {
    let r = Rettangolo::new(5.0, 3.0);
    println!("Area: {}", r.area());           // 15
    println!("Perimetro: {}", r.perimetro());  // 16
}
{% endhighlight %}

`Rettangolo::new(...)` si chiama con `::`, non con `.`, perché non
opera su un'istanza già esistente.

---

## Vec di struct

Una delle applicazioni più comuni: un vettore di struct per
rappresentare una tabella di dati.

#### Esercizio 5
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
struct Studente {
    nome: String,
    voto: i32,
}

fn main() {
    let classe = vec![
        Studente { nome: String::from("Alice"), voto: 9 },
        Studente { nome: String::from("Bruno"), voto: 6 },
        Studente { nome: String::from("Carla"), voto: 8 },
    ];

    for s in &classe {
        println!("{}: {}", s.nome, s.voto);
    }

    let migliore = classe.iter().max_by_key(|s| s.voto).unwrap();
    println!("Migliore: {} ({})", migliore.nome, migliore.voto);
}
{% endhighlight %}

`for s in &classe` scorre il vettore **per riferimento**: senza `&`,
l'ownership degli `Studente` verrebbe consumata dal ciclo.

---

## Esercizi

#### Esercizio 6
Definisci una struct `Prodotto` con campi `nome: String`, `prezzo: f64`
e `quantita: u32`. Crea un vettore di 5 prodotti, calcola il valore
totale in magazzino (`prezzo * quantita`) e stampa il prodotto più
costoso.

#### Esercizio 7
Definisci una struct `Data` con campi `giorno`, `mese` e `anno` (tutti
`u32`). Scrivi un metodo `fn stampa(&self)` che stampi nel formato
`GG/MM/AAAA`.

#### Esercizio 8
Definisci una struct `Vettore2D` con campi `x` e `y` (`f64`). Aggiungi
i metodi `fn somma(&self, altro: &Vettore2D) -> Vettore2D` e
`fn modulo(&self) -> f64`. Testa con due vettori a scelta.

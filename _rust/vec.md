---
title: 'Rust: Vec<T> (vettori)'
date: '2026-09-04T00:00:00+01:00'
author: Fabio Mattei
layout: page
---

## Che cos'è un Vec

`Vec<T>` (si legge "vettore di T") è la collezione dinamica di Rust:
una sequenza di elementi dello **stesso tipo** la cui dimensione può
**crescere o diminuire** durante l'esecuzione del programma. A
differenza dell'[array]({{ site.baseurl }}{% link _rust/array.md %}.html)
(`[T; N]`), che ha dimensione fissa e vive di norma sullo stack, un
`Vec<T>`:

- alloca i suoi dati sull'**heap**, perché la dimensione può cambiare
  e non è nota a compile-time;
- si ridimensiona **automaticamente** quando serve (ad esempio con
  `.push()`);
- è la scelta di default in Rust ogni volta che non si sa in anticipo
  quanti elementi serviranno.

`Vec<T>` è generico: `Vec<i32>` contiene interi, `Vec<String>`
contiene stringhe, `Vec<Vec<i32>>` è un vettore di vettori (utile per
rappresentare matrici a dimensione variabile), e così via.

---

## Creare un Vec

Ci sono due modi principali per creare un vettore: partire vuoto e
riempirlo con `.push()`, oppure usare la macro `vec![...]` per
crearlo già con dei valori.

#### Esercizio 1
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
fn main() {
    // vettore vuoto, riempito dopo
    let mut a: Vec<i32> = Vec::new();
    a.push(1);
    a.push(2);
    a.push(3);
    println!("{:?}", a);  // [1, 2, 3]

    // vettore già inizializzato con dei valori
    let b = vec![10, 20, 30];
    println!("{:?}", b);  // [10, 20, 30]

    // vettore con N copie dello stesso valore
    let c = vec![0; 5];
    println!("{:?}", c);  // [0, 0, 0, 0, 0]
}
{% endhighlight %}

`Vec::new()` crea un vettore vuoto: qui serve annotare il tipo
(`Vec<i32>`) perché altrimenti il compilatore non avrebbe modo di
dedurlo dal nulla. Con `vec![...]`, invece, il tipo si deduce dai
valori scritti.

---

## Aggiungere e togliere elementi

`Vec<T>` cresce e si riduce con `.push()` e `.pop()`. È proprio
questa capacità — impossibile con un array — la ragione principale
per cui si sceglie `Vec<T>`.

#### Esercizio 2
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
fn main() {
    let mut v = vec![1, 2, 3];

    v.push(4);   // aggiunge in fondo
    v.push(5);

    println!("Dimensione: {}", v.len());          // 5
    println!("Primo: {}", v[0]);                   // 1
    println!("Ultimo: {}", v[v.len() - 1]);         // 5

    let ultimo = v.pop();   // rimuove l'ultimo, restituisce Option<T>
    println!("Rimosso: {:?}", ultimo);              // Some(5)
    println!("Dopo pop: {}", v.len());              // 4
}
{% endhighlight %}

`.pop()` restituisce un `Option<T>` e non `T` direttamente, perché
esiste il caso limite in cui il vettore sia vuoto: in quel caso
restituisce `None` invece di andare in panic, evitando così di
crashare quando si prova a togliere un elemento che non c'è.

Oltre a `push`/`pop`, esistono metodi per inserire o rimuovere in
posizioni arbitrarie, non solo in fondo:

{% highlight rust %}
fn main() {
    let mut v = vec![1, 2, 4];

    v.insert(2, 3);   // inserisce 3 in posizione 2: [1, 2, 3, 4]
    println!("{:?}", v);

    v.remove(0);      // rimuove l'elemento in posizione 0: [2, 3, 4]
    println!("{:?}", v);
}
{% endhighlight %}

`.insert()` e `.remove()` in mezzo al vettore sono più lente di
`.push()`/`.pop()` in fondo, perché richiedono di spostare tutti gli
elementi successivi: se possibile conviene lavorare in fondo al
vettore.

---

## Indicizzazione: [] vs get()

Ci sono due modi per leggere l'elemento a un certo indice di un
`Vec<T>`, e si comportano in modo molto diverso quando l'indice non
esiste.

**L'operatore `[]`** è il più diretto, ma è anche il più "fragile":
se l'indice richiesto è fuori dai limiti del vettore, il programma
va in **panic** e termina immediatamente. Vale la stessa regola già
vista per gli array: nessun comportamento indefinito come in C, ma
comunque un arresto brusco del programma.

#### Esercizio 3
Copia il seguente codice nell'editor e osserva l'errore a runtime.

{% highlight rust %}
fn main() {
    let frutti = vec!["mela", "pera", "banana"];
    println!("{}", frutti[10]);  // panic: index out of bounds
}
{% endhighlight %}

Il messaggio (`index out of bounds: the len is 3 but the index is
10`) è chiaro, ma a quel punto è **troppo tardi**: il programma si è
già fermato. `[]` va bene solo quando sei sicuro che l'indice esista
(ad esempio perché lo hai appena controllato con `.len()`, o perché
viene da un ciclo `for i in 0..v.len()`).

**Il metodo `.get(i)`**, al contrario, non va mai in panic: restituisce
sempre un valore, di tipo `Option<&T>`, che può essere di due forme:

- `Some(riferimento)` — l'indice esiste, e il riferimento punta
  all'elemento trovato;
- `None` — l'indice non esiste, senza alcun errore o crash: è un
  valore "normale" da gestire come qualsiasi altro.

In altre parole, `.get()` trasforma un possibile errore a runtime in
un caso da gestire esplicitamente nel codice, con `match` (o con `if
let`, come vedremo). È lo stesso `Option<T>` restituito da `.pop()`,
visto in un esercizio precedente: Rust lo usa sistematicamente ogni
volta che un'operazione potrebbe "non trovare" un risultato.

#### Esercizio 4
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
fn main() {
    let frutti = vec!["mela", "pera", "banana"];

    match frutti.get(1) {
        Some(f) => println!("Trovato: {}", f),
        None => println!("Indice non valido"),
    }

    match frutti.get(10) {
        Some(f) => println!("Trovato: {}", f),
        None => println!("Indice non valido"),  // questo viene stampato
    }

    for f in &frutti {
        print!("{} ", f);
    }
    println!();
}
{% endhighlight %}

Qui `f` è solo un nome di variabile a scelta (poteva chiamarsi `x`,
`valore`, come preferisci): il pattern `Some(f)` fa scattare quel
ramo del `match` **solo se** il valore è `Some(...)`, ed estrae ciò
che c'è dentro assegnandolo a `f`. Se `frutti.get(1)` vale
`Some(&"pera")`, allora dentro quel ramo `f` vale `&"pera"`: è per
questo che `println!("{}", f)` stampa `pera`. Questo "estrarre il
valore da dentro `Some`" è il punto centrale del pattern matching:
non stai leggendo una proprietà di `Option`, stai scomponendo il
valore stesso in base a quale variante (`Some` o `None`) è.

Quando interessa gestire solo il caso `Some` (ignorando `None`, o
trattandolo in modo molto semplice), `match` è spesso più verboso del
necessario. `if let` permette di scrivere lo stesso controllo in modo
più compatto:

{% highlight rust %}
fn main() {
    let frutti = vec!["mela", "pera", "banana"];

    if let Some(f) = frutti.get(1) {
        println!("Trovato: {}", f);
    } else {
        println!("Indice non valido");
    }
}
{% endhighlight %}

Un'altra alternativa comoda è `.unwrap_or()`, che restituisce il
valore dentro `Some`, oppure un valore di **default** indicato da te
se è `None` — utile quando basta un valore "di ripiego" invece di un
messaggio:

{% highlight rust %}
fn main() {
    let frutti = vec!["mela", "pera", "banana"];

    let f1 = frutti.get(1).unwrap_or(&"sconosciuto");
    let f2 = frutti.get(10).unwrap_or(&"sconosciuto");

    println!("{}", f1);  // pera
    println!("{}", f2);  // sconosciuto
}
{% endhighlight %}

Riepilogo:

| Espressione       | Se l'indice esiste       | Se l'indice NON esiste          |
|--------------------|----------------------------|-----------------------------------|
| `v[i]`             | restituisce il valore       | **panic**, il programma termina    |
| `v.get(i)`         | `Some(&valore)`             | `None`, nessun crash               |
| `v.get(i).unwrap_or(&default)` | il valore trovato | il valore di default indicato       |

Regola pratica: usa `[]` quando sei certo che l'indice esista (ad
esempio dopo un controllo su `.len()`), usa `.get(i)` quando l'indice
può ragionevolmente non esistere (input dell'utente, calcolo, ecc.).

---

## len() e capacity(): due cose diverse

`Vec<T>` tiene traccia di due numeri distinti:

- **`.len()`**: quanti elementi contiene *davvero* in questo momento;
- **`.capacity()`**: quanto spazio è già stato allocato sull'heap,
  prima di dover richiedere altra memoria al sistema operativo.

Per efficienza, quando un `Vec` deve crescere non alloca esattamente
un posto in più a ogni `.push()`: alloca **più spazio di quanto
serva subito**, così le push successive sono immediate finché c'è
capacità libera.

#### Esercizio 5
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
fn main() {
    let mut v: Vec<i32> = Vec::new();

    for i in 0..5 {
        v.push(i);
        println!("len = {}, capacity = {}", v.len(), v.capacity());
    }
}
{% endhighlight %}

Nota come `capacity` cresca "a scatti" e non di uno alla volta: è un
dettaglio di implementazione (l'esatta strategia di crescita non è
garantita dal linguaggio), ma il concetto — spazio riservato in
anticipo per evitare riallocazioni troppo frequenti — è comune a
quasi tutti i linguaggi con array dinamici (ad es. `ArrayList` in
Java o le liste di Python). Se si conosce già in anticipo quanti
elementi serviranno, `Vec::with_capacity(n)` permette di riservare
subito lo spazio giusto ed evitare riallocazioni inutili.

---

## Metodi utili di Vec

| Metodo          | Significato                                          |
|-----------------|-------------------------------------------------------|
| `.push(x)`      | aggiunge `x` in fondo                                  |
| `.pop()`        | rimuove e restituisce l'ultimo elemento (`Option<T>`)  |
| `.insert(i, x)` | inserisce `x` in posizione `i`                         |
| `.remove(i)`    | rimuove e restituisce l'elemento in posizione `i`       |
| `.len()`        | numero di elementi presenti                             |
| `.capacity()`   | spazio già allocato sull'heap                           |
| `.is_empty()`   | `true` se il vettore è vuoto                             |
| `.clear()`      | svuota il vettore (la capacity resta allocata)          |
| `.get(i)`       | accesso sicuro (`Option<&T>`)                            |
| `.contains(&x)` | `true` se `x` è presente                                 |
| `.sort()`       | ordina in-place, ordine crescente                        |
| `.reverse()`    | inverte l'ordine degli elementi, in-place                |
| `.iter()`       | iteratore sui riferimenti agli elementi (`&T`)           |

---

## Ordinamento con sort

#### Esercizio 6
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
fn main() {
    let mut v = vec![5, 2, 8, 1, 9, 3];

    v.sort();   // ordine crescente
    println!("{:?}", v);

    v.sort_by(|a, b| b.cmp(a));  // ordine decrescente
    println!("{:?}", v);
}
{% endhighlight %}

`.sort_by()` accetta una **chiusura** (closure) che confronta due
elementi: `a.cmp(a, b)` restituisce l'ordine "naturale" crescente,
mentre `b.cmp(a)` lo inverte, ottenendo un ordine decrescente.

---

## Iterare su un Vec: tre modi

Ci sono tre modi principali per scorrere un vettore, a seconda che si
voglia solo leggere, leggere e modificare, oppure "consumare" il
vettore prendendone la proprietà degli elementi.

#### Esercizio 7
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
fn main() {
    let mut v = vec![1, 2, 3];

    // 1) &v oppure v.iter(): riferimenti immutabili, v resta utilizzabile
    for x in &v {
        print!("{} ", x);
    }
    println!();

    // 2) &mut v oppure v.iter_mut(): riferimenti mutabili, si può modificare ogni elemento
    for x in &mut v {
        *x *= 10;
    }
    println!("{:?}", v);  // [10, 20, 30]

    // 3) v (senza &): consuma il vettore, che non è più utilizzabile dopo
    for x in v {
        print!("{} ", x);
    }
    println!();
    // println!("{:?}", v);  // ERRORE: v è stato consumato dal ciclo precedente
}
{% endhighlight %}

Il terzo caso (`for x in v`) chiama implicitamente `v.into_iter()` e
sposta (move) la proprietà di ogni elemento nel ciclo: dopo, il
vettore originale non esiste più. È lo stesso principio di ownership
visto nella pagina [Ownership e borrowing]({{ site.baseurl }}{% link _rust/ownership.md %}.html),
applicato a una collezione invece che a un singolo valore.

---

## Vec e slice: passare vettori alle funzioni

Come per gli array, una funzione che riceve `&Vec<T>` funziona solo
con vettori; una funzione che riceve una **slice** `&[T]` funziona
sia con vettori sia con array, quindi è quasi sempre la scelta
migliore per un parametro di sola lettura.

{% highlight rust %}
fn somma(v: &[i32]) -> i32 {
    let mut totale = 0;
    for n in v {
        totale += n;
    }
    totale
}

fn main() {
    let vettore = vec![1, 2, 3, 4, 5];
    let array = [10, 20, 30];

    println!("{}", somma(&vettore));  // funziona: Vec si converte in slice
    println!("{}", somma(&array));    // funziona: anche l'array
}
{% endhighlight %}

---

## Leggere un vettore da input

#### Esercizio 8
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
use std::io;

fn main() {
    let mut input = String::new();
    println!("Quanti numeri? ");
    io::stdin().read_line(&mut input).expect("Errore");
    let n: usize = input.trim().parse().expect("Non è un numero");

    let mut v: Vec<i32> = Vec::new();
    for _ in 0..n {
        let mut riga = String::new();
        io::stdin().read_line(&mut riga).expect("Errore");
        let x: i32 = riga.trim().parse().expect("Non è un numero");
        v.push(x);
    }

    let massimo = v.iter().max().expect("Vettore vuoto");
    println!("Massimo: {}", massimo);
}
{% endhighlight %}

Qui `Vec<T>` è indispensabile: non si conosce `n` finché il
programma non è in esecuzione, quindi un array a dimensione fissa
(`[i32; N]`) non potrebbe funzionare — servirebbe conoscere `N` già a
compile-time. `.iter().max()` scorre il vettore per riferimento e
restituisce un `Option<&i32>`: il massimo, se il vettore non è vuoto.

---

## Vec\<Vec\<T\>\>: matrici a dimensione variabile

Così come un array può contenere altri array, un `Vec` può contenere
altri `Vec`, ottenendo una struttura a griglia le cui righe (e
colonne) possono avere dimensioni decise a runtime.

#### Esercizio 9
Copia il seguente codice nell'editor e fallo eseguire.

{% highlight rust %}
fn main() {
    let mut matrice: Vec<Vec<i32>> = Vec::new();

    for riga in 0..3 {
        let mut nuova_riga = Vec::new();
        for colonna in 0..3 {
            nuova_riga.push(riga * 3 + colonna);
        }
        matrice.push(nuova_riga);
    }

    for riga in &matrice {
        for valore in riga {
            print!("{} ", valore);
        }
        println!();
    }
}
{% endhighlight %}

---

## Riepilogo: array vs Vec

| Aspetto                    | `[T; N]` (array)         | `Vec<T>`                     |
|-----------------------------|---------------------------|--------------------------------|
| Dimensione                  | fissa, nota a compile-time | dinamica, può cambiare a runtime |
| Memoria                     | tipicamente sullo stack     | sempre sull'heap                |
| Crescita (`push`)          | non disponibile             | `.push()`, `.pop()`, `.insert()`, `.remove()` |
| Tipo dipende dalla dimensione | sì (`[i32; 3] != [i32; 5]`) | no (`Vec<i32>` per qualsiasi lunghezza) |
| Da preferire quando...      | la dimensione è fissa "per natura" | la dimensione dipende da input o cambia nel tempo |

---

## Esercizi

#### Esercizio 10
Leggi N numeri in un `Vec<f64>` e calcola la media aritmetica.

#### Esercizio 11
Leggi N interi in un vettore e stampa separatamente i pari e i dispari.

#### Esercizio 12
Leggi N stringhe in un `Vec<String>`, ordinale con `.sort()` e stampale
in ordine alfabetico.

#### Esercizio 13
Implementa la ricerca lineare: leggi N interi, poi un valore da
cercare; stampa l'indice della prima occorrenza o -1 se non trovato.
(Suggerimento: puoi anche usare `.iter().position(|&x| x == valore)`.)

#### Esercizio 14
Scrivi una funzione `fn inverti(v: &mut Vec<i32>)` che inverte l'ordine
degli elementi in-place (senza usare `.reverse()`).

#### Esercizio 15
Scrivi un programma che crei un `Vec::with_capacity(10)` vuoto,
stampi `len()` e `capacity()` subito dopo la creazione, poi aggiunga
10 elementi con `.push()` e stampi di nuovo `len()` e `capacity()`.
Osserva che, avendo riservato spazio in anticipo, la capacity non
cambia durante i push.

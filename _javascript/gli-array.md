---
title: 'Gli array'
date: '2026-08-18T10:00:00+01:00'
author: Fabio Mattei
layout: page
---

JavaScript fornisce un tipo built-in chiamato **array**, molto simile alle liste (array indicizzati) già viste in PHP e Python: una sequenza **mutabile** di valori, in genere omogenei, ciascuno raggiungibile tramite un indice numerico.

Come per le variabili a ciascun array viene assegnato un nome. È possibile creare un array vuoto usando le parentesi quadre.

{% highlight javascript %}
const vuoto = []; // nuovo array vuoto di nome: vuoto
{% endhighlight %}

Se ho bisogno di creare un array cui intendo inserire dei valori al momento stesso della sua creazione è sufficiente che io elenchi tra parentesi quadre questi valori separati da virgole.

{% highlight javascript %}
const numeri = [0, 1, 2, 3];               // array di 4 elementi numerici
const lettere = ['a', 'b', 'c', 'd', 'e']; // array di 5 elementi di tipo stringa
const parole = ['mattina', 'pomeriggio'];  // array di 2 elementi di tipo stringa
{% endhighlight %}

Nota l'uso di `const`: come già visto nella pagina sulle [variabili]({{ site.baseurl }}{% link _javascript/variabili.md %}.html), `const` impedisce di riassegnare la variabile `numeri` ad un array diverso, ma **non impedisce affatto** di modificarne il contenuto (aggiungere, togliere o cambiare elementi): per questo, in JavaScript, gli array si dichiarano quasi sempre con `const`.

#### Cosa posso fare con un array?

Ogni array numera internamente i suoi elementi cominciando a contare dallo 0. Si fa riferimento all'indice usando la notazione: **nomearray\[indice\]**. Gli indici dunque vanno da 0 a n − 1 dove n sono gli elementi dell'array. Come PHP, e a differenza di Python, **JavaScript non supporta direttamente gli indici negativi** con la notazione `[]`: `lettere[-1]` non genera un errore, ma restituisce `undefined`, perché -1 non è un indice valido dell'array.

| indice | 0 | 1 | 2 | 3 | 4 |
|---|---|---|---|---|---|
| contenuto | 'a' | 'b' | 'c' | 'd' | 'e' |

{% highlight javascript %}
const primaLettera = lettere[0];                      // 'a'
const secondaLettera = lettere[1];                     // 'b'
const ultimaLettera = lettere[lettere.length - 1];     // 'e' (equivalente a lettere[-1] in Python)
console.log(lettere.at(-1));                            // 'e', at() supporta gli indici negativi!
{% endhighlight %}

Le cose funzionano anche al contrario, per esempio se eseguo le istruzioni:

{% highlight javascript %}
lettere[0] = 'u';
lettere[3] = 'k';
{% endhighlight %}

Mi ritroverò con i contenuti dell'array *lettere* variati nel seguente modo (ricorda: questo è legale anche se `lettere` è stato dichiarato con `const`, perché stiamo modificando il contenuto, non riassegnando la variabile).

| indice | 0 | 1 | 2 | 3 | 4 |
|---|---|---|---|---|---|
| contenuto | 'u' | 'b' | 'c' | 'k' | 'e' |

#### Aggiungere, togliere, unire

A differenza di PHP e Python, che offrono operatori dedicati (`+`, `array_merge`), in JavaScript gli array sono oggetti dotati di **metodi**:

- `array.push(valore)` → aggiunge un elemento **in coda** all'array (l'equivalente di `$array[] = valore` in PHP e di `.append()` in Python)
- `array.pop()` → rimuove e restituisce l'**ultimo** elemento
- `array1.concat(array2)` → concatenazione: crea un **nuovo** array formato dall'unione degli elementi di array1 con gli elementi di array2

{% highlight javascript %}
// concatenazione
const vocali = ['a', 'e'].concat(['i', 'o', 'u']);
// risultato ['a', 'e', 'i', 'o', 'u']

// aggiunta in coda
const classe = ['Gino', 'Sandra'];
classe.push('Marco');
console.log(classe); // ['Gino', 'Sandra', 'Marco']
{% endhighlight %}

## Slicing, facciamo gli array "a fette"

Attraverso il metodo **slice** si può prendere una fetta di un array, esattamente come lo slicing di Python (e a differenza di `array_slice` di PHP, il secondo parametro qui è un **indice finale**, non una lunghezza).

{% highlight javascript %}
const lettere = ['a', 'b', 'c', 'd', 'e'];
const lettereAlCentro = lettere.slice(1, 4); // ['b', 'c', 'd']
{% endhighlight %}

`array.slice(inizio, fine)` estrae dall'array una sua sottosezione che parte dall'indice `inizio` e arriva **fino a** (escluso) l'indice `fine`: identico, in questo, allo slicing delle liste Python.

## includes()

Il metodo `includes()` mi permette di controllare se un elemento è contenuto in un array oppure no:

{% highlight javascript %}
const lettere = ['a', 'b', 'c', 'd', 'e'];
if (lettere.includes('c')) {
    console.log("Il carattere c e' contenuto nell'array lettere");
}
if (!lettere.includes('k')) {
    console.log("Il carattere k NON e' contenuto nell'array lettere");
}
{% endhighlight %}

Gli array JavaScript supportano anche altre proprietà e funzioni:

| Funzione | Cosa fa | Esempio | Risultato |
|---|---|---|---|
| **length** (proprietà) | Calcola la lunghezza dell'array | `lettere.length` | 5 |
| **Math.min(...array)** | Trova l'elemento più piccolo | `Math.min(...numeri)` | il minimo |
| **Math.max(...array)** | Trova l'elemento più grande | `Math.max(...numeri)` | il massimo |
| **array.reverse()** | Inverte l'array **sul posto** | `lettere.reverse()` | `['e','d','c','b','a']` |
| **array.sort()** | Ordina l'array **sul posto** | `numeri.sort()` | array ordinato |

Nota `...numeri`: è l'operatore **spread**, che "spacchetta" gli elementi di un array come se fossero argomenti separati. Serve perché `Math.max()` e `Math.min()` accettano singoli argomenti, non un array intero.

## Visitiamo il nostro array

Visitare o percorrere l'array significa **prendere ad uno ad uno ciascuno degli elementi che lo compongono e applicare dei comandi a ciascuno di questi**. Il modo più diretto è il ciclo `for...of`, visto in [Il ciclo for]({{ site.baseurl }}{% link _javascript/il-ciclo-for.md %}.html):

{% highlight javascript %}
const lettere = ['a', 'b', 'c', 'd', 'e'];
for (const x of lettere) {
    console.log(x);
}
{% endhighlight %}

Ad ogni iterazione `x` assumerà il valore di uno dei caratteri contenuti nell'array lettere e lo scriverà sullo schermo.

{% highlight javascript %}
const numeri = [1, 2, 3, 4, 5, 6];
for (const x of numeri) {
    if (x % 2 === 0) {
        console.log(x);
    }
}
{% endhighlight %}

## I metodi map, filter e reduce

Oltre ai cicli espliciti, JavaScript mette a disposizione tre metodi molto potenti che permettono di **trasformare** un array **senza** scrivere un ciclo `for`: sono l'equivalente diretto delle funzioni `map`, `filter` e `reduce` di PHP e Python.

`map()` crea un **nuovo array** applicando una funzione a ciascun elemento:

{% highlight javascript %}
const numeri = [1, 2, 3, 4, 5];
const quadrati = numeri.map(n => n ** 2);
console.log(quadrati); // [1, 4, 9, 16, 25]
{% endhighlight %}

`n => n ** 2` è una **arrow function** (funzione anonima), che vedremo nel dettaglio nella pagina sulle [funzioni]({{ site.baseurl }}{% link _javascript/funzioni.md %}.html): per ora leggila come "presa una `n`, restituisce `n ** 2`".

`filter()` crea un **nuovo array** contenente solo gli elementi che soddisfano una condizione:

{% highlight javascript %}
const numeri = [1, 2, 3, 4, 5, 6];
const pari = numeri.filter(n => n % 2 === 0);
console.log(pari); // [2, 4, 6]
{% endhighlight %}

`reduce()` **riduce** l'intero array ad un singolo valore, accumulando un risultato passo dopo passo (è il metodo più simile ad un accumulatore scritto a mano):

{% highlight javascript %}
const numeri = [1, 2, 3, 4, 5];
const somma = numeri.reduce((acc, n) => acc + n, 0);
console.log(somma); // 15
{% endhighlight %}

Il `0` finale è il valore iniziale dell'accumulatore. Ad ogni passo, `reduce` chiama la funzione con l'accumulatore attuale (`acc`) e l'elemento corrente (`n`), e il risultato diventa il nuovo accumulatore.

## Esercizi sugli array

#### Esercizio 1:
Scrivi un programma JavaScript che inizializzi un array "nomi" contenente sei nomi a tua scelta. In seguito il programma scrive sullo schermo il nome inserito nell'array all'indice 3 e sostituisce il nome ad indice 5 con un altro a piacere.

#### Esercizio 2:
Scrivi un programma JavaScript in cui venga inizializzato un array di nomi contenente sei nomi a piacere e che scriva i nomi contenuti nell'array iniziale utilizzando un ciclo `for...of`.

#### Esercizio 3:
Scrivi un programma JavaScript in cui venga inizializzato un array di nomi contenente sei nomi a piacere e che, utilizzando una stringa da utilizzarsi come accumulatore, crei una stringa contenente tutti i nomi concatenati.

#### Esercizio 4:
Scrivi un programma JavaScript in cui venga inizializzato un array di nomi contenente sei nomi a piacere e che crei un nuovo array contenente tutti i nomi in questo modo: il primo nome compare una volta, il secondo nome due volte, il terzo nome tre volte ecc…

#### Esercizio 5:
Scrivi un programma JavaScript in cui venga inizializzato un array di nomi contenente sei nomi a piacere e che crei un nuovo array contenente i soli nomi che iniziano per vocale (usa `filter`).

#### Esercizio 6:
Inizializza un array comprendente numeri interi a caso a tua scelta, quindi scrivi un programma JavaScript che, usando `filter`, crei un nuovo array con i soli numeri dispari.

#### Esercizio 7:
Inizializza due array di numeri interi a caso a tua scelta quindi scrivi una piccola funzione che confronti i due array e ritorni vero se hanno almeno un elemento in comune.

#### Esercizio 8:
Scrivi un programma JavaScript che trovi il penultimo elemento più piccolo in un array.

#### Esercizio 9:
Scrivi un programma JavaScript che calcoli la frequenza di elementi di un array (conta il ripetersi di ciascun valore), usando un oggetto come accumulatore (li vedremo nella prossima pagina, [Gli oggetti]({{ site.baseurl }}{% link _javascript/gli-oggetti.md %}.html)).

#### Esercizio 10:
Scrivi un programma JavaScript che converta un array di numeri interi in un singolo intero, concatenando le loro cifre.
Esempio:
Array iniziale: [11, 33, 50]
Intero in Output: 113350

#### Esercizio 11:
Scrivi un programma JavaScript che, inizializzato un array di numeri interi, calcoli il prodotto di tutti i numeri contenuti (usa `reduce`).

#### Esercizio 12:
Scrivi un programma JavaScript che, dati 10 numeri in un array, li scriva in ordine inverso (usa `reverse()`).

#### Esercizio 13:
Scrivi un programma JavaScript che, dati 10 numeri, li smisti in due array separati che si chiamano: `numeriGrandi` e `numeriPiccoli`. Nel primo array vanno i numeri più piccoli di 100, nel secondo quelli più grandi. I numeri negativi non vanno memorizzati.

### Esercizi di tracciamento

Per i seguenti esercizi non dovete scrivere codice: dovete costruire la **tabella di tracciamento** del programma, mostrando come cambia il contenuto dell'array (o delle variabili) ad ogni iterazione del ciclo.

#### Esercizio 14:

Costruite la tabella di tracciamento del seguente programma (mostrate il contenuto di `numeri` e il valore di `x` ad ogni iterazione):

{% highlight javascript %}
const numeri = [3, 8, 1];
for (let x = 1; x < 4; x++) {
    numeri.push(x * 2);
}
console.log(numeri);
{% endhighlight %}

#### Esercizio 15:

Costruite la tabella di tracciamento del seguente programma (mostrate i valori di `lettere`, `indice` e `acc` ad ogni iterazione):

{% highlight javascript %}
const lettere = ['a', 'b', 'c', 'd'];
let indice = 0;
let acc = "";
while (indice < lettere.length) {
    acc = acc + lettere[indice];
    indice = indice + 1;
}
console.log(acc);
{% endhighlight %}

#### Esercizio 16:

Costruite la tabella di tracciamento del seguente programma (mostrate i valori di `numeri`, `x` e `massimo` ad ogni iterazione):

{% highlight javascript %}
const numeri = [4, 9, 2, 15, 6];
let massimo = numeri[0];
for (const x of numeri) {
    if (x > massimo) {
        massimo = x;
    }
}
console.log(massimo);
{% endhighlight %}

### Esercizi di ricerca degli errori (debugging)

Nei seguenti programmi è stato inserito un errore. Provate prima a individuarlo leggendo il codice (senza eseguirlo), poi verificate eseguendolo in Node.js ed infine correggetelo.

#### Esercizio 17:

Questo programma genera un errore di sintassi.

{% highlight javascript %}
const numeri = [1, 2, 3, 4, 5];
for (const x of numeri) {
    console.log(x);
{% endhighlight %}

#### Esercizio 18:

Questo programma non genera un errore, ma il valore stampato non è quello che ci si aspetterebbe (a differenza di Python, dove genererebbe un `IndexError`): individuate quale istruzione lo causa e spiegate perché.

{% highlight javascript %}
const numeri = [1, 2, 3, 4, 5];
console.log(numeri[5]);
{% endhighlight %}

#### Esercizio 19:

Questo programma non genera errori ma contiene un **errore logico**: dovrebbe stampare la somma di tutti gli elementi dell'array, ma stampa un valore sbagliato. Individuate l'errore.

{% highlight javascript %}
const numeri = [1, 2, 3, 4, 5];
let somma = 0;
for (const x of numeri) {
    somma = x;
}
console.log(somma);
{% endhighlight %}

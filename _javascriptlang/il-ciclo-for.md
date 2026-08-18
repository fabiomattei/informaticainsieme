---
title: 'Il ciclo for'
date: '2026-08-18T09:55:00+01:00'
author: Fabio Mattei
layout: page
---

Come PHP, e a differenza di Python (dove esiste un solo tipo di ciclo `for`), JavaScript mette a disposizione **più costrutti distinti** per iterare:

- `for`: un ciclo **C-style**, in cui inizializzazione, condizione e incremento sono scritti esplicitamente sulla stessa riga (identico a quello di PHP);
- `for...of`: un ciclo pensato apposta per scandire gli **elementi** di un array (o di una stringa), molto simile al `for ... in` di Python e al `foreach` di PHP;
- `for...in`: un ciclo che scandisce le **chiavi** (gli indici, o le proprietà di un oggetto) — attenzione, il nome è simile a `for...of` ma il comportamento è diverso!

## Il ciclo for (C-style)

{% highlight javascript %}
for (<inizializzazione>; <condizione>; <incremento>) {
    // corpo del ciclo
}
{% endhighlight %}

{% highlight javascript %}
// Esempio 1: stampa i numeri da 0 a 4
for (let i = 0; i < 5; i++) {
    console.log(i);
}
{% endhighlight %}

`i++` è una scorciatoia per `i = i + 1;`. Esiste anche `i--` per decrementare. Nota che la variabile del ciclo va dichiarata con `let` (non `const`, perché il suo valore cambia ad ogni iterazione).

## Il ciclo for...of

Il ciclo `for...of` ci permette di iterare su tutti gli **elementi** di un array (o di una stringa) ed eseguire un determinato blocco di codice per ciascuno.

{% highlight javascript %}
for (const elemento of array) {
    // elabora elemento
}
{% endhighlight %}

{% highlight javascript %}
// Esempio 2:
for (const a of [5, 10, 15, 20, 25]) {
    console.log(a);
}
{% endhighlight %}

{% highlight javascript %}
// Esempio 3:
const seq = [1, 2, 3, 4, 5];
for (const n of seq) {
    if (n % 2 === 0) {
        console.log(n + " e' pari");
    } else {
        console.log(n + " e' dispari");
    }
}
{% endhighlight %}

Nota l'uso di `const` invece di `let`: ad ogni iterazione viene creata una nuova variabile `n`, che non viene mai riassegnata all'interno del ciclo, quindi `const` è la scelta più corretta (esattamente come per le variabili viste in [Variabili]({{ site.baseurl }}{% link _javascriptlang/variabili.md %}.html)).

## Il ciclo for...in

`for...in`, nonostante il nome simile, scandisce le **chiavi** (gli indici numerici, nel caso di un array) invece che i valori. Lo useremo soprattutto con gli oggetti, nella pagina [Gli oggetti]({{ site.baseurl }}{% link _javascriptlang/gli-oggetti.md %}.html); su un array è raramente la scelta giusta, ma è bene conoscerne la differenza:

{% highlight javascript %}
const lettere = ["a", "b", "c"];
for (const indice in lettere) {
    console.log(indice);          // 0, 1, 2 (le chiavi, come stringhe!)
}
for (const lettera of lettere) {
    console.log(lettera);         // "a", "b", "c" (i valori)
}
{% endhighlight %}

## range

Non esiste, in JavaScript, una funzione `range()` incorporata come in Python (o come quella aggiunta da PHP). Il ciclo `for` C-style è il modo standard per generare una sequenza di numeri:

{% highlight javascript %}
// equivalente a range(7) in Python: da 0 a 6
for (let x = 0; x < 7; x++) {
    console.log(x);
}
{% endhighlight %}

{% highlight javascript %}
for (let x = 1; x <= 4; x++) {
    console.log("Quadrato di " + x + ": " + x ** 2);
}
// scrive:
// Quadrato di 1: 1
// Quadrato di 2: 4
// Quadrato di 3: 9
// Quadrato di 4: 16
{% endhighlight %}

## Variabili accumulatori e ciclo for

Utilizzare il ciclo for con una variabile accumulatore è molto semplice. Nel seguente esempio si vede come si usa l'accumulatore per sommare 6 numeri.

{% highlight javascript %}
// Esempio di uso di accumulatore per la somma
let acc = 0;
for (let x = 1; x <= 6; x++) {
    acc = acc + x;
}
console.log(acc); // nota che questa istruzione e' fuori dal ciclo
{% endhighlight %}

Nel seguente esempio si vede come si usa l'accumulatore per moltiplicare 6 numeri.

{% highlight javascript %}
// Esempio di uso di accumulatore per la moltiplicazione
let acc = 1;
for (let x = 1; x <= 6; x++) {
    acc = acc * x;
}
console.log(acc); // nota che questa istruzione e' fuori dal ciclo
{% endhighlight %}

## Il campione

Può capitare di dover scandire una sequenza di numeri e trovare il più grande. In questo caso utilizzo una variabile **campione** che ciclo dopo ciclo conterrà il valore migliore trovato.

{% highlight javascript %}
const numeri = [4, 19, 2, 55, 6, 3, 41, 20, 9, 12];
let massimo = numeri[0];
for (let x = 1; x < numeri.length; x++) {
    if (numeri[x] > massimo) {
        massimo = numeri[x];
    }
}
console.log(massimo);
{% endhighlight %}

## Esercizi

#### Esercizio 1:
Scrivere un programma che scriva tutti i numeri da 1 a 100

#### Esercizio 2:
Scrivere un programma che scriva tutti i numeri pari da 1000 a 5000.

#### Esercizio 3:
Scrivere un ciclo che sommi tutti i numeri dispari minori di 100. Scrivere la somma ottenuta.

#### Esercizio 4:
Scrivere un ciclo che, dato un numero N, scriva i dieci numeri pari successivi ad N

#### Esercizio 5:
Scrivere un ciclo che, dati 10 numeri (in un array), ne scriva il massimo.

#### Esercizio 6:
Scrivere un ciclo che, dati 10 numeri (in un array), scriva la somma dei numeri il cui valore è compreso fra 10 e 20.

#### Esercizio 7:
Scrivere un ciclo che, dati due numeri N e M con N < M, scriva tutti i numeri compresi tra N e M.

#### Esercizio 8:
Scrivere un ciclo che, dati due numeri N e M con N < M, sommi tutti i numeri compresi tra N e M. Scrivere la somma ottenuta.

#### Esercizio 9:
Scrivere un ciclo che, dato un numero N, scriva tutti i suoi divisori

#### Esercizio 10:
Scrivere un ciclo che, dato un numero N, ne calcoli la radice quadrata intera (ovvero il massimo intero x tale che x²≤N)

#### Esercizio 11:
Scrivere un ciclo che, dato un numero N, ne conti i divisori e ne scriva il numero

#### Esercizio 12:
Scrivere un ciclo che calcoli il fattoriale di un numero intero.

#### Esercizio 13:
Scrivere un programma che, dati due numeri, chieda (o simuli) l'inserimento della somma, e fino a quando non è quella corretta, il programma scriva la frase "Errato: riprova"

#### Esercizio 14:
Scrivere un programma che, dato un numero positivo N, determini il massimo intero K tale che la somma dei primi K interi sia minore o uguale a N.

Ad esempio, se N=20 allora K risulta 5, infatti

1 + 2 + 3 + 4 + 5 = 15 mentre

1 + 2 + 3 + 4 + 5 + 6 = 21

#### Esercizio 15:
Trovare il minor numero di banconote da 100€, 50€, 10€, 5€, necessarie per pagare una assegnata cifra C multipla di 5.

#### Esercizio 16:
Scrivi un programma JavaScript che, utilizzando due cicli for annidati, scriva la tabellina della addizione.

#### Esercizio 17:
Scrivi un programma JavaScript che, utilizzando due cicli for annidati, scriva la tabellina della moltiplicazione.

#### Esercizio 18:
Scrivi un programma JavaScript che trovi i numeri primi compresi tra 1 e 100000.

#### Esercizio 19:
Scrivere un programma che, dato un numero intero N, calcoli la somma di tutte le cifre dispari che lo compongono

#### Esercizio 20:
Scrivere un programma che, dati due numeri interi, calcoli il massimo comune divisore e scriva il risultato

#### Esercizio 21:
Scrivi un algoritmo che, dato un numero, scriva "numero primo" se questo è un numero primo, "numero non primo" in caso opposto.

### Esercizi di tracciamento

Per i seguenti esercizi non dovete scrivere codice: dovete costruire la **tabella di tracciamento** del programma, cioè una tabella che mostra riga per riga come cambia il valore di ogni variabile ad ogni iterazione del ciclo `for`.

#### Esercizio 22:

Costruite la tabella di tracciamento del seguente programma (mostrate i valori di `x` e `acc` ad ogni iterazione):

{% highlight javascript %}
let acc = 0;
for (let x = 1; x < 6; x++) {
    acc = acc + x;
}
console.log(acc);
{% endhighlight %}

#### Esercizio 23:

Costruite la tabella di tracciamento del seguente programma (mostrate i valori di `x` e `acc` ad ogni iterazione). Attenzione al valore di partenza e al passo:

{% highlight javascript %}
let acc = 1;
for (let x = 10; x > 0; x -= 2) {
    acc = acc + x;
}
console.log(acc);
{% endhighlight %}

#### Esercizio 24:

Costruite la tabella di tracciamento del seguente programma (mostrate i valori di `x`, `cont` e `somma` ad ogni iterazione):

{% highlight javascript %}
let somma = 0;
let cont = 0;
for (let x = 1; x < 11; x++) {
    if (x % 3 === 0) {
        somma = somma + x;
        cont = cont + 1;
    }
}
console.log(cont, somma);
{% endhighlight %}

#### Esercizio 25:

Costruite la tabella di tracciamento del seguente programma. Fate attenzione al valore di `massimo` prima ancora che il ciclo cominci:

{% highlight javascript %}
const valori = [3, 7, 2, 9, 4];
let massimo = 0;
for (const x of valori) {
    if (x > massimo) {
        massimo = x;
    }
}
console.log(massimo);
{% endhighlight %}

### Esercizi di ricerca degli errori (debugging)

Nei seguenti programmi è stato inserito un **errore di sintassi**. Provate prima a individuarlo leggendo il codice (senza eseguirlo), poi verificate eseguendolo in Node.js ed infine correggetelo.

#### Esercizio 26:

{% highlight javascript %}
for (let x = 1; x < 10; x++)
    console.log(x);
{% endhighlight %}

Questo in realtà è codice valido (le graffe sono facoltative per un blocco di una sola istruzione, come già visto per `if`): riscrivilo aggiungendo le graffe, come buona pratica.

#### Esercizio 27:

{% highlight javascript %}
for (let x = 1, x < 10, x++) {
    console.log(x);
}
{% endhighlight %}

#### Esercizio 28:

{% highlight javascript %}
let acc = 0;
for (let x = 1; x < 11; x++) {
    acc = acc + x;
    console.log("Il valore parziale e'" acc);
}
{% endhighlight %}

#### Esercizio 29:

{% highlight javascript %}
for (let x = 1; x < 10; x++) {
    console.log(x)
{% endhighlight %}

#### Esercizio 30:

Questo programma non contiene errori di sintassi (viene eseguito senza generare errori), ma contiene un **errore logico**: individuatelo e spiegate perché il programma non stampa il risultato che ci si aspetterebbe.

{% highlight javascript %}
let acc = 0;
for (let x = 1; x < 11; x++) {
    acc = x + x;
}
console.log(acc);
{% endhighlight %}

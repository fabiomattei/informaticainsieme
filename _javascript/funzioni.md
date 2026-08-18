---
title: Funzioni
date: '2026-08-18T10:15:00+01:00'
author: Fabio Mattei
layout: page
---

JavaScript è un linguaggio e in quanto tale è costituito da un vocabolario che comprende molte parole, tra queste ricordiamo `console.log`, `if`, `while` e `for`. Attraverso queste parole il programmatore comunica all'interprete, e quest'ultimo al computer, le operazioni da svolgere.

Il vocabolario di JavaScript non è fisso ma estensibile: possiamo cioè definire nuove parole con nuovi significati. Queste vengono chiamate funzioni e raggruppano al loro interno una sequenza di istruzioni ordinata (algoritmo) da eseguire quando vengono invocate.

#### La mia prima funzione

Iniziamo con un esempio, definiamo una funzione `hello` che si limita a scrivere sul display alcune stringhe di testo:

{% highlight javascript %}
function hello() {
    console.log("Ciao!");
    console.log("Ciao!!!");
    console.log("Ciao a te!");
}
{% endhighlight %}

La parola chiave **function** ci permette di definire una nuova funzione, esattamente come in PHP. Questa viene seguita dalla parola che vogliamo utilizzare come nome per la funzione, in questo caso `hello`. Il nome è seguito da una coppia di parentesi, che vedremo tra poco a cosa servono, e dal blocco di istruzioni da eseguire, racchiuso tra **graffe** `{ }`, esattamente come per `if`, `while` e `for`.

A questo punto l'interprete sa che quando incontra l'invocazione della funzione `hello` deve eseguire le istruzioni in essa contenute.

{% highlight javascript %}
console.log("Precede la chiamata a funzione");
hello();
console.log("Segue la chiamata a funzione");
{% endhighlight %}

Output:

{% highlight shell %}
Precede la chiamata a funzione
Ciao!
Ciao!!!
Ciao a te!
Segue la chiamata a funzione
{% endhighlight %}

## Parametri e argomenti

Molte delle istruzioni JavaScript viste in precedenza sono a loro volta funzioni (o metodi). Quando chiamate `"ciao".toUpperCase()`, non passate nessun argomento; quando chiamate `Number("42")`, passate un valore chiamato argomento scrivendolo tra parentesi.

Proviamo a definire la funzione `hello` in questo modo:

{% highlight javascript %}
function hello(nome) {
    console.log("Ciao " + nome);
}
{% endhighlight %}

La definizione della funzione `hello` in questo programma ha un parametro chiamato `nome`. Un parametro è una variabile in cui viene memorizzato un argomento nel momento in cui viene chiamata la funzione. Se la funzione viene chiamata passandole la stringa "Alice" come argomento, l'esecuzione del programma entra nella funzione e la variabile `nome` viene impostata a "Alice". L'istruzione `console.log` va dunque a stampare la stringa "Ciao Alice". L'argomento, memorizzato nel parametro, viene dimenticato al terminare dell'esecuzione della funzione.

## I valori di ritorno

{% highlight javascript %}
const lunghezza = "Ciao".length;
{% endhighlight %}

Nell'esempio in alto, si legge la proprietà `length` della stringa 'Ciao' al fine di ottenere la lunghezza della stringa. Il valore calcolato viene restituito al chiamante e da questi memorizzato nella variabile `lunghezza`.

In generale il valore che una funzione calcola viene chiamato valore di ritorno. Quando si crea una funzione è possibile specificare quale sia il valore di ritorno attraverso l'istruzione **return**, esattamente come in PHP e Python. Facciamo un esempio:

{% highlight javascript %}
function raddoppia(numero) {
    return numero * 2;
}
{% endhighlight %}

La funzione `raddoppia` accetta un parametro e restituisce il doppio dell'argomento che le viene passato.

Facciamo un secondo esempio:

{% highlight javascript %}
function pariODispari(numero) {
    if (numero % 2 === 0) {
        return 'Pari';
    } else {
        return 'Dispari';
    }
}
{% endhighlight %}

La funzione `pariODispari` accetta un parametro e restituisce la stringa 'Pari' se l'argomento passato è pari e la stringa 'Dispari' in caso contrario. È possibile inserire due o più istruzioni di return in una funzione. Quando l'interprete incontra l'istruzione `return` esce dall'ambito della funzione e torna nel programma da cui questa era stata chiamata. Nessuna istruzione all'interno della funzione viene eseguita dopo il return.

{% highlight javascript %}
const primoNumero = pariODispari(4);   // primoNumero = 'Pari'
const secondoNumero = pariODispari(7); // secondoNumero = 'Dispari'
{% endhighlight %}

Se una funzione JavaScript termina senza incontrare un `return`, restituisce il valore speciale `undefined` (non `null`, come invece fa PHP).

## Le arrow function

Oltre alla sintassi `function` classica, JavaScript offre una forma più compatta per definire funzioni, chiamata **arrow function** (funzione freccia), introdotta con `=>`. Non esiste un vero equivalente in PHP (che ha una sintassi simile solo per le funzioni anonime `fn`) né in Python (che ha `lambda`, ma limitata ad una singola espressione).

{% highlight javascript %}
// Funzione classica
function raddoppia(numero) {
    return numero * 2;
}

// Stessa funzione, come arrow function
const raddoppiaFreccia = (numero) => {
    return numero * 2;
};

// Se il corpo è una sola espressione, si può omettere "return" e le graffe
const raddoppiaCompatta = numero => numero * 2;
{% endhighlight %}

Le tre versioni si comportano in modo identico: `raddoppia(5)`, `raddoppiaFreccia(5)` e `raddoppiaCompatta(5)` restituiscono tutte `10`. Le arrow function sono particolarmente comode per funzioni brevi, come quelle passate a `map`, `filter` e `reduce`, già viste nella pagina [Gli array]({{ site.baseurl }}{% link _javascript/gli-array.md %}.html):

{% highlight javascript %}
const numeri = [1, 2, 3, 4, 5];
const quadrati = numeri.map(n => n ** 2);
{% endhighlight %}

## Parametri facoltativi (valori di default)

È possibile definire delle funzioni con dei parametri facoltativi, parametri cioè per cui viene specificato un argomento di default che viene utilizzato a meno che non specificato diversamente dal chiamante, esattamente come in PHP.

{% highlight javascript %}
function moltiplica(fattore1, fattore2 = 2) {
    return fattore1 * fattore2;
}
{% endhighlight %}

Il parametro `fattore2` viene inizializzato a 2 se non specificato diversamente dal chiamante.

{% highlight javascript %}
let prodotto = moltiplica(3);
console.log(prodotto); // stampa il valore 6
prodotto = moltiplica(3, 3);
console.log(prodotto); // stampa il valore 9
{% endhighlight %}

A differenza di PHP e Python, dove i parametri con default vanno sempre specificati **dopo** i parametri ordinari, in JavaScript questa è solo una **convenzione** fortemente consigliata, non una regola imposta dall'interprete: un parametro senza default dopo uno con default è comunque legale, ma va evitato perché rende la funzione confusa da chiamare.

## Ambito locale e ambito globale

Parametri e variabili definiti all'interno di una funzione hanno un ambito (o raggio d'azione, in inglese *scope*) locale alla funzione stessa. Le variabili dichiarate all'esterno di tutte le funzioni hanno invece un ambito globale.

**Qui c'è una differenza importante rispetto a PHP**: in PHP il codice dentro una funzione **non vede affatto** le variabili globali, nemmeno in lettura, a meno di dichiararle con `global`. In **JavaScript è l'esatto contrario, esattamente come in Python**: il codice dentro una funzione può *leggere* liberamente una variabile globale senza bisogno di dichiararla.

{% highlight javascript %}
let uova = 1234567;
function pollaio() {
    console.log(uova); // corretto, funziona subito: stampa 1234567 (diverso da PHP!)
}
pollaio();
{% endhighlight %}

Se però dentro la funzione **assegniamo** un valore ad una variabile con lo stesso nome (con `let` o `const`), stiamo creando una **nuova variabile locale**, distinta da quella globale, che "nasconde" quella esterna solo all'interno della funzione:

{% highlight javascript %}
let uova = 7; // variabile globale
function pollaio() {
    let uova = 32765; // nuova variabile locale, non collegata a quella globale!
    console.log(uova); // stampa 32765
}
pollaio();
console.log(uova); // stampa 7 (invariata)
{% endhighlight %}

Valgono le seguenti proprietà, identiche a PHP e Python:

- istruzioni nell'ambito globale non possono usare variabili che appartengono ad un ambito locale;
- istruzioni contenute all'interno di un ambito locale non possono accedere a variabili appartenenti ad un diverso ambito locale;
- è possibile utilizzare lo stesso nome per variabili diverse se si trovano in ambiti diversi.

{% highlight javascript %}
function pollaio() {
    const uova = 32765;
}
pollaio();
console.log(uova); // ReferenceError: uova is not defined
{% endhighlight %}

Come potete notare la variabile `uova` appartiene all'ambito della funzione `pollaio`. La funzione viene invocata, ma una volta terminata la variabile locale `uova` viene distrutta: non può dunque essere utilizzata dal flusso di programma principale.

## Collaborare attraverso le funzioni

Quando un gruppo di sviluppatori lavora ad un software un aspetto molto delicato è quello della divisione dei compiti e del lavoro. Un approccio che spesso viene utilizzato è quello di strutturare il software identificandone le varie caratteristiche quindi organizzare queste ultime all'interno delle varie funzioni. A questo punto il gruppo stabilisce un linguaggio comune per la comunicazione esplicitando la firma di tutte le funzioni: il nome, i parametri che accettano e i valori che restituiscono al chiamante. Tutto ciò va fatto prima dell'implementazione delle funzioni stesse.

## Inizia scrivendo un test

Supponiamo di voler scrivere una funzione che deve fare la trasformazione di gradi Celsius in gradi Fahrenheit. Noi tutti sappiamo che: Tf = Tc * 9 / 5 + 32.

Ci serviamo dell'istruzione **console.assert**, l'equivalente JavaScript di `assert` in PHP e Python:

{% highlight javascript %}
function trasfCelsInFahr(temp) {
    return (temp * 9 / 5) + 32;
}
console.assert(trasfCelsInFahr(0) === 32.0);
console.assert(trasfCelsInFahr(20) === 68.0);
console.assert(trasfCelsInFahr(30) === 86.0);
console.log("Tutti i test sono passati!");
{% endhighlight %}

A differenza di PHP e Python, dove `assert` interrompe il programma se il test fallisce, `console.assert()` in JavaScript **si limita a scrivere un messaggio di errore sulla console** se il test fallisce, ma il programma continua ad essere eseguito. È comunque uno strumento utile per verificare rapidamente che l'implementazione sia corretta.

## Organizza il tuo codice in file diversi

Quando si scrive il codice è bene scriverlo in modo che questo sia facile da tenere sotto controllo e che sia riutilizzabile in una o più applicazioni. Per questo è importante organizzare il codice in più file. Immaginiamo di lavorare su di un progetto di fisica. Creiamo una cartella che conterrà tutti i file del nostro progetto, che chiameremo `progettodifisica`.

- progettodifisica/
- progettodifisica/principale.js
- progettodifisica/grandezze/conversioni.js

La cartella `progettodifisica` contiene un file `principale.js` il quale contiene la sezione principale del codice, quello cioè che mando in esecuzione quando ho necessità di utilizzare il software.

{% highlight javascript %}
// grandezze/conversioni.js
function trasfCelsInFahr(temp) {
    return temp * 9 / 5 + 32;
}

module.exports = { trasfCelsInFahr };
{% endhighlight %}

{% highlight javascript %}
// principale.js

// carica le funzioni esportate dal file conversioni.js
const { trasfCelsInFahr } = require('./grandezze/conversioni.js');

function main() {
    const temp = 20;
    const fahr = trasfCelsInFahr(temp);
    console.log('Temp. Fahrenheit = ' + fahr);
}

main();
{% endhighlight %}

L'istruzione **require**, con Node.js, carica un altro file JavaScript: è l'equivalente Node.js di `require_once` in PHP e di `import` in Python. Ogni file deve dichiarare esplicitamente cosa vuole rendere disponibile agli altri file tramite `module.exports`: a differenza di PHP, dove tutte le funzioni definite in un file diventano automaticamente disponibili dopo un `require`, in Node.js **solo** ciò che viene esportato esplicitamente è visibile da fuori.

## Esercizi

### Funzioni e numeri

#### Esercizio n1:

Scrivi una funzione `saluta` che prenda la stringa `nome` come parametro e restituisca al chiamante la stringa composta da `'Ciao ' + nome`.

#### Esercizio n2:

Scrivi una funzione `calcolaMaggiore` che prenda due numeri come parametro (`num1` e `num2`) e restituisca al chiamante il più grande tra i due.

#### Esercizio n3:

Scrivi una funzione `calcolaMaggiore` che prenda tre numeri come parametro e restituisca al chiamante il più grande tra i tre.

#### Esercizio n4:
Scrivi una funzione che accetti un parametro numerico e calcoli il fattoriale del numero ricevuto, restituendolo al chiamante.

#### Esercizio n5:

Implementa una funzione che, preso come parametro un numero intero, restituisca al chiamante il corrispondente numero di Fibonacci.

#### Esercizio n6:

Implementa una funzione che, preso come parametro un numero intero, restituisca al chiamante `true` se il numero è primo e `false` altrimenti.

#### Esercizio n7:

Scrivi con approccio Test Driven Development (partendo dai `console.assert`) una funzione che calcoli il massimo comune divisore tra due numeri.

#### Esercizio n8:

Scrivi con approccio Test Driven Development una funzione che calcoli il minimo comune multiplo tra due numeri.

#### Esercizio n9:

Scrivere una funzione con approccio TDD che calcoli la distanza tra due punti sul piano cartesiano.
firma: `function distanza(ax, ay, bx, by)`

#### Esercizio n10:

Cosa fa il seguente script? Prova a rispondere prima di eseguirlo.

{% highlight javascript %}
let biciclette = 123;
function scriviBiciclette() {
    console.log(biciclette);
}
scriviBiciclette();
{% endhighlight %}

#### Esercizio n11:

Cosa fa il seguente script? Prova a rispondere prima di eseguirlo, ricordando la differenza tra assegnare e dichiarare una variabile.

{% highlight javascript %}
let biciclette = 123;
function scriviBiciclette() {
    let biciclette = 321;
    console.log(biciclette);
}
scriviBiciclette();
{% endhighlight %}

#### Esercizio n12:

Crea una funzione `divisibilePer` che prenda come parametro due numeri e che restituisca `true` se il primo numero è divisibile per il secondo e `false` in caso contrario.

#### Esercizio n13:

Scrivi una funzione `sommaNumeri` che prenda come parametri due numeri interi `a` e `b` e calcoli la somma di tutti i numeri compresi tra `a` e `b`, estremi compresi.
Esempio:
sommaNumeri(4, 6) restituisce 15
sommaNumeri(1, 4) restituisce 10

#### Esercizio n14:

Scrivi una funzione `celsiusToFahrenheit` che accetti come parametro una temperatura in gradi Celsius e restituisca la corrispondente temperatura in gradi Fahrenheit. Scrivi poi una funzione `fahrenheitToCelsius` che faccia l'operazione opposta.

### Funzioni e stringhe

#### Esercizio s1:

Scrivi una funzione a cui viene passato un carattere come parametro, e che restituisca al chiamante la stringa 'vocale' se il carattere è una vocale o la stringa 'consonante' in caso contrario.

#### Esercizio s2:

Crea una funzione `tipoStringa` che prenda come parametro una stringa di testo e restituisca:
* la stringa "solo lettere" se il parametro è costituito completamente da lettere
* la stringa "solo numeri" se il parametro è costituito completamente da numeri
* la stringa "mista" se il parametro è costituito sia da lettere sia da numeri

#### Esercizio s3:

Scrivi una funzione che accetti una stringa di testo come parametro e la restituisca invertita al chiamante.

#### Esercizio s4:

Scrivi una funzione che accetti un parametro di tipo stringa e restituisca il numero di vocali contenute in questa.

### Funzioni e array

#### Esercizio l1:
Scrivi una funzione che accetti un array come parametro, calcoli la somma dei numeri contenuti nell'array e restituisca il risultato.

#### Esercizio l2:
Scrivi una funzione che accetti un array come parametro e restituisca un array contenente i soli numeri pari dell'array ricevuto (usa `filter`).

### Refactoring

#### Esercizio r1:

Riorganizza il seguente codice JavaScript in funzioni. Bisogna suddividere l'algoritmo in algoritmi più piccoli e semplici da capire in modo da rendere il codice più leggibile.

{% highlight javascript %}
console.log("La mia calcolatrice");
const operazioni = [
    { op: '+', num1: 4, num2: 6 },
    { op: '*', num1: 3, num2: 7 },
];
for (const { op, num1, num2 } of operazioni) {
    let risultato;
    if (op === '+') {
        risultato = num1 + num2;
    } else if (op === '-') {
        risultato = num1 - num2;
    } else if (op === '*') {
        risultato = num1 * num2;
    } else if (op === '/') {
        risultato = num1 / num2;
    } else {
        risultato = 'Operatore sconosciuto';
    }
    console.log(risultato);
}
console.log("Grazie! Alla prossima volta");
{% endhighlight %}

### Esercizi di tracciamento

Per i seguenti esercizi non dovete scrivere codice: dovete costruire la **tabella di tracciamento** del programma, indicando per ciascuna riga eseguita l'ambito (globale o il nome della funzione) in cui ci si trova e il valore di ogni variabile.

#### Esercizio t1:

Costruite la tabella di tracciamento del seguente programma, indicando ad ogni istruzione l'ambito in cui viene eseguita e il valore delle variabili coinvolte:

{% highlight javascript %}
function raddoppia(numero) {
    numero = numero * 2;
    return numero;
}

let x = 5;
let y = raddoppia(x);
x = raddoppia(y);
{% endhighlight %}

Che valore hanno `x` e `y` alla fine dell'esecuzione?

#### Esercizio t2:

Costruite la tabella di tracciamento del seguente programma, ricordando che ogni chiamata a funzione crea un nuovo ambito locale (mostrate i valori di `n`, `acc` e `risultato` ad ogni chiamata):

{% highlight javascript %}
function sommaFinoA(n) {
    let acc = 0;
    let x = 1;
    while (x <= n) {
        acc = acc + x;
        x = x + 1;
    }
    return acc;
}

const risultato = sommaFinoA(4);
console.log(risultato);
{% endhighlight %}

### Esercizi di ricerca degli errori (debugging)

Nei seguenti programmi è stato inserito un errore. Provate prima a individuarlo leggendo il codice (senza eseguirlo), poi verificate eseguendolo in Node.js ed infine correggetelo.

#### Esercizio e1:

Questo programma genera un errore di sintassi.

{% highlight javascript %}
function saluta(nome) {
    console.log("Ciao " + nome);

saluta("Marco");
{% endhighlight %}

#### Esercizio e2:

Questo programma non genera errori ma contiene un **errore logico**: la funzione dovrebbe restituire il doppio del numero, ma chi la chiama riceve sempre `undefined`. Individuate l'errore.

{% highlight javascript %}
function raddoppia(numero) {
    numero * 2;
}

const risultato = raddoppia(5);
console.log(risultato);
{% endhighlight %}

#### Esercizio e3:

Questo programma non genera errori ma contiene un **errore logico**: la funzione dovrebbe restituire il maggiore tra i due numeri passati, ma per alcuni valori restituisce il risultato sbagliato. Individuate l'errore.

{% highlight javascript %}
function calcolaMaggiore(num1, num2) {
    if (num1 > num2) {
        return num1;
    } else {
        return num1;
    }
}

console.log(calcolaMaggiore(3, 8));
{% endhighlight %}

#### Esercizio e4:

Questo programma genera un errore in esecuzione (`ReferenceError`): individuate quale istruzione lo causa e spiegate perché, ricordando le regole sull'ambito locale.

{% highlight javascript %}
function pollaio() {
    const uova = 32765;
}

pollaio();
console.log(uova);
{% endhighlight %}

---
title: 'Il ciclo while'
date: '2026-08-18T09:50:00+01:00'
author: Fabio Mattei
layout: page
---

Il costrutto **while** in JavaScript permette di eseguire dei comandi un certo numero di volte. Si chiama ciclo perché i comandi contenuti al suo interno vengono ripetuti **ciclicamente**.

La sintassi è la seguente:

{% highlight javascript %}
while (<espressione booleana>) {
    <comando 1>
    <comando 2>
    <comando 3>
}
{% endhighlight %}

While è un ciclo con **controllo in testa**, questo significa che *il controllo sull'espressione booleana viene fatto prima di entrare nel ciclo*, esattamente come in PHP e Python.

Le istruzioni all'interno del ciclo vengono eseguite se il risultato dell'espressione booleana è **vero**. In caso contrario (espressione booleana falsa) il blocco comandi interno al ciclo viene ignorato e l'esecuzione del programma continua con la prima istruzione successiva al ciclo, cioè quella che segue la graffa di chiusura.

Scrivi ed esegui il seguente programma:

{% highlight javascript %}
let cont = 0;
while (cont < 10) {
    cont = cont + 1;
    console.log(cont);
}
{% endhighlight %}

| cont | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|---|---|---|---|---|

Questo programma ci mostra un ciclo le cui istruzioni contenute all'interno vengono eseguite *fintanto che cont < 10*. La **variabile cont** si dice **contatore** dato che il suo scopo è contare il numero di volte che il ciclo è stato eseguito. Questo costrutto è molto diffuso.

Facciamo un secondo esempio:

{% highlight javascript %}
let cont = 0;
while (cont < 10) {
    cont = cont + 1;
}
console.log(cont);
{% endhighlight %}

Noterai che l'istruzione `console.log(cont)` non è posta all'interno del ciclo (non è contenuta tra le graffe) dato che si trova dopo la graffa di chiusura del while. Questo ciclo scriverà sulla console il solo numero 10.

Ricorda che in JavaScript sono le **graffe**, non l'indentazione, a delimitare cosa appartiene al ciclo!

Scriviamo ora un programma che calcoli la somma dei primi 10 numeri interi

{% highlight javascript %}
let n = 10;
let cont = 1;            // variabile contatore
let acc = 0;              // variabile accumulatore
while (cont <= n) {
    acc = acc + cont;     // incremento l'accumulatore di cont
    cont = cont + 1;      // incremento il contatore di 1
}
console.log(acc);         // al termine del ciclo scrivo il valore di acc
{% endhighlight %}

Nel precedente esempio abbiamo introdotto il concetto di **accumulatore**. Si dice accumulatore una variabile che *accumula dopo ciascuna esecuzione delle istruzioni all'interno del ciclo i risultati di un calcolo*. Nell'esempio ad `acc` viene sommato di volta in volta il contenuto della variabile contatore. Possiamo vedere come le variabili si comportano in una tabella di tracciamento:

| **n** | 10 |  |  |  |  |  |  |  |  |  |  |
|---|---|---|---|---|---|---|---|---|---|---|---|
| **cont** | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 |
| **acc** | 0 | 1 | 3 | 6 | 10 | 15 | 21 | 28 | 36 | 45 | 55 |

{% highlight javascript %}
// Questo programma calcola la sequenza di Fibonacci.
let accA = 0;
let accB = 1;
let cont = 0;
let maxCont = 20;
while (cont < maxCont) {
    cont = cont + 1;
    // Occorre tenere traccia finché ci sono cambiamenti
    let vecchioAccA = accA;
    let vecchioAccB = accB;
    accA = vecchioAccB;
    accB = vecchioAccA + vecchioAccB;
    process.stdout.write(vecchioAccA + " ");
}
console.log();
{% endhighlight %}

L'algoritmo di Fibonacci funziona su di un gioco di due variabili accumulatori: `accA` e `accB`. Nota `process.stdout.write()`: a differenza di `console.log()`, che va sempre a capo, questa funzione di Node.js scrive senza andare a capo, utile quando vogliamo stampare più valori sulla stessa riga separati da uno spazio.

{% highlight javascript %}
// Attende sino a quando la password non corrisponde a quella corretta.
let password = "foobar";
const passwordCorretta = "unicorn";
let tentativi = 0;
// Simuliamo alcuni tentativi dell'utente, dato che leggere da tastiera
// in modo sincrono con Node.js richiede strumenti che vedremo più avanti.
const tentativiUtente = ["prova", "sbagliato", "unicorn"];
while (password !== passwordCorretta) {
    password = tentativiUtente[tentativi];
    tentativi = tentativi + 1;
}
console.log("Benvenuto");
{% endhighlight %}

#### Esercizio 1:

Scrivere un programma che, dato un numero intero N, calcoli la somma di tutti i numeri da 1 ad N

#### Esercizio 2:

Scrivere un programma che, dati un numero intero N ed un numero intero M (con N<M), calcoli la somma di tutti i numeri da N ad M (estremi inclusi)

#### Esercizio 3:

Scrivere un programma che, dato un numero intero N, calcoli il fattoriale di N (1 * 2 * 3 * 4 …. * N)

#### Esercizio 4:

Scrivere un programma che, dati un numero intero N ed un numero intero M (con N<M), calcoli la somma di tutti i numeri pari compresi tra N ed M

#### Esercizio 5:

Scrivere un programma JavaScript che, dati 10 numeri (in un array), calcoli l'ammontare dei soli numeri pari.

#### Esercizio 6:

Scrivere un programma che, dati N numeri, calcoli il numero massimo e il numero minimo tra questi.

#### Esercizio 7:

Scrivere un programma che, dati N numeri, calcoli la somma di tutti i numeri mostrando le somme parziali ogni 3 numeri.

#### Esercizio 8:

Leonardo Pisano propose nel tredicesimo secolo il seguente problema:

Immaginiamo di chiudere una coppia di conigli in un recinto sapendo che per ogni coppia di conigli valgono le seguenti condizioni:

- Inizia a generare dal secondo mese di età
- Genera una nuova coppia ogni mese
- Non muore mai;

Quante coppie di conigli ci saranno dopo un anno?

E quante dopo un numero di mesi N?

### Esercizi di tracciamento

Per i seguenti esercizi non dovete scrivere codice: dovete costruire la **tabella di tracciamento** del programma, cioè una tabella che mostra riga per riga come cambia il valore di ogni variabile ad ogni iterazione del ciclo, esattamente come abbiamo fatto per l'esempio dell'accumulatore.

#### Esercizio 9:

Costruite la tabella di tracciamento del seguente programma (mostrate i valori di `cont` e `acc` ad ogni iterazione):

{% highlight javascript %}
let cont = 1;
let acc = 1;
while (cont <= 5) {
    acc = acc * cont;
    cont = cont + 1;
}
console.log(acc);
{% endhighlight %}

Qual è il valore stampato alla fine? Che calcolo sta eseguendo questo programma?

#### Esercizio 10:

Costruite la tabella di tracciamento del seguente programma (mostrate i valori di `a`, `b` e `cont` ad ogni iterazione):

{% highlight javascript %}
let a = 1;
let b = 10;
let cont = 0;
while (a < b) {
    a = a + 2;
    b = b - 1;
    cont = cont + 1;
}
console.log(cont, a, b);
{% endhighlight %}

Attenzione: la condizione del ciclo dipende da due variabili che cambiano entrambe ad ogni iterazione.

#### Esercizio 11:

Costruite la tabella di tracciamento del seguente programma. Fate attenzione: in questo caso il ciclo potrebbe non venire mai eseguito, oppure eseguito un numero di volte diverso da quello che vi aspettate.

{% highlight javascript %}
let x = 20;
let cont = 0;
while (x > 1) {
    x = Math.floor(x / 2);
    cont = cont + 1;
}
console.log(cont);
{% endhighlight %}

### Esercizi di ricerca degli errori (debugging)

Nei seguenti programmi è stato inserito un **errore di sintassi**. Provate prima a individuarlo leggendo il codice (senza eseguirlo), poi verificate eseguendolo in Node.js ed infine correggetelo.

#### Esercizio 12:

{% highlight javascript %}
let cont = 0;
while (cont < 5) {
    console.log(cont);
    cont = cont + 1;
{% endhighlight %}

#### Esercizio 13:

{% highlight javascript %}
let cont = 0;
while (cont < 5 {
    console.log(cont);
    cont = cont + 1;
}
{% endhighlight %}

#### Esercizio 14:

Questo programma non contiene errori di sintassi (viene eseguito senza generare errori), ma contiene un **errore logico**: individuatelo e spiegate perché il ciclo non termina mai.

{% highlight javascript %}
let cont = 0;
while (cont < 5) {
    console.log(cont);
    cont = 0;
}
{% endhighlight %}

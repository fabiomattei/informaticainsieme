---
title: Le stringhe di testo
date: '2026-08-18T09:20:00+01:00'
author: Fabio Mattei
layout: page
---

# Un susseguirsi di caratteri

Una stringa di testo altro non è che un susseguirsi di caratteri. Internamente JavaScript rappresenta ogni carattere con un codice numerico (secondo lo standard **Unicode**, un'estensione della tabella ASCII vista in PHP e Python, capace di rappresentare anche gli emoji e gli alfabeti non latini).

Per questo motivo si dice che le stringhe di testo sono tipi di dato composti: sono composte da dati più piccoli, i caratteri.

# Inizializzare una stringa

{% highlight javascript %}
let saluto = "ciao";
{% endhighlight %}

Assegnare una stringa di testo corrisponde al memorizzare i caratteri che la compongono all'interno di un'area di memoria e ad etichettare quell'area di memoria attraverso un nome. In JavaScript le stringhe si possono scrivere con apici doppi `"..."`, apici singoli `'...'` o, in più rispetto a PHP e Python, con gli **apici inversi** (backtick) `` `...` ``, che permettono l'**interpolazione** di variabili direttamente dentro la stringa:

{% highlight javascript %}
let nome = "Marco";
console.log("Ciao " + nome);   // concatenazione classica: Ciao Marco
console.log(`Ciao ${nome}`);   // interpolazione con i backtick: Ciao Marco
{% endhighlight %}

Le stringhe scritte con i backtick si chiamano **template literal**: qualunque espressione JavaScript scritta dentro `${...}` viene calcolata e inserita al suo posto. È il modo più moderno e leggibile per costruire stringhe che contengono variabili, e da qui in avanti lo useremo spesso.

{% highlight javascript %}
let a = 3;
let b = 4;
console.log(`${a} + ${b} = ${a + b}`);   // 3 + 4 = 7 (l'espressione viene calcolata!)
{% endhighlight %}

# Lunghezza di una stringa

Dato che una stringa è composta da caratteri può capitare di voler contare i caratteri che la compongono. In JavaScript questo si ottiene tramite la **proprietà** `length` (non una funzione, quindi senza parentesi, come già visto nella pagina sui [tipi di variabile]({{ site.baseurl }}{% link _javascript/tipi-di-variabile.md %}.html)):

{% highlight javascript %}
let numeroCaratteri = "ciao".length;
console.log(numeroCaratteri);  // 4
{% endhighlight %}

# Accediamo ad un singolo carattere contenuto in una stringa

Qualche volta ci capita di avere necessità di leggere un singolo carattere all'interno della stringa. In quel caso utilizziamo l'operatore `[]`, esattamente come in PHP e Python:

{% highlight javascript %}
let saluto = "ciao";
let singolaLettera = saluto[1];
console.log(singolaLettera);  // i
{% endhighlight %}

JavaScript numera ciascun carattere contenuto in una stringa con un indice. Il conteggio inizia da 0 e finisce a n-1, dove n sono i caratteri che compongono la stringa. A differenza di Python, JavaScript **non supporta gli indici negativi**: `saluto[-1]` non genera un errore, ma restituisce `undefined`, perché -1 non è un indice valido. Per ottenere l'ultimo carattere si usa `saluto[saluto.length - 1]`, oppure il metodo `at()`:

{% highlight javascript %}
let saluto = "ciao";
console.log(saluto[-1]);              // undefined
console.log(saluto[saluto.length - 1]); // o (equivalente a saluto[-1] in Python)
console.log(saluto.at(-1));           // o (at() supporta anche gli indici negativi!)
{% endhighlight %}

# Confrontiamo le stringhe

Gli operatori di confronto per le stringhe di testo sono gli stessi visti per i numeri:

* `==` e `===` (uguale — vedremo la differenza nella pagina sulla [condizione]({{ site.baseurl }}{% link _javascript/la-condizione.md %}.html))
* `>` (viene dopo di)
* `>=` (viene dopo di o è uguale a)
* `<` (viene prima di)
* `<=` (viene prima di o è uguale a)
* `!=` e `!==` (è diverso)

{% highlight javascript %}
"ciao" === "ciao";       // true
"ciao" > "ciao";         // false
"mattino" > "sera";      // false
"mattino" > "mattina";   // false
{% endhighlight %}

Il confronto tra stringhe di testo avviene considerando un carattere per volta di ciascuna stringa, in base al loro codice Unicode: le lettere maiuscole vengono sempre prima delle minuscole, esattamente come in PHP e Python.

# Visitiamo le stringhe

Attraverso l'uso degli indici possiamo visitare una stringa di testo, cioè possiamo considerare un carattere per volta all'interno di un ciclo per farci delle operazioni.

{% highlight javascript %}
let saluto = "ciao";
let indice = 0;
while (indice < saluto.length) {
    let lettera = saluto[indice];
    if (lettera === "a" || lettera === "e" || lettera === "i" || lettera === "o" || lettera === "u") {
        console.log("vocale");
    } else {
        console.log("consonante");
    }
    indice = indice + 1;
}
{% endhighlight %}

A differenza di PHP, dove una stringa non è un array e serve `str_split()`, in JavaScript una stringa **è già iterabile direttamente**: possiamo scandirla con `for...of` (che vedremo nella pagina sui [cicli for]({{ site.baseurl }}{% link _javascript/il-ciclo-for.md %}.html)), in modo molto simile al `for` di Python:

{% highlight javascript %}
let saluto = "ciao";
for (const lettera of saluto) {
    if (lettera === "a" || lettera === "e" || lettera === "i" || lettera === "o" || lettera === "u") {
        console.log("vocale");
    } else {
        console.log("consonante");
    }
}
{% endhighlight %}

## Alcuni metodi utili sulle stringhe

Le stringhe JavaScript sono oggetti dotati di **metodi**, funzioni che si invocano con la sintassi `stringa.metodo()`, molto simile alle funzioni PHP applicate alla stringa ma scritte "dopo" la stringa invece che "prima":

| Metodo | Cosa fa | Esempio | Risultato |
|---|---|---|---|
| `toUpperCase()` | converte in maiuscolo | `"ciao".toUpperCase()` | `"CIAO"` |
| `toLowerCase()` | converte in minuscolo | `"CIAO".toLowerCase()` | `"ciao"` |
| `trim()` | rimuove spazi iniziali/finali | `"  ciao  ".trim()` | `"ciao"` |
| `includes()` | verifica se contiene una sottostringa | `"ciao".includes("ia")` | `true` |
| `indexOf()` | trova la posizione di una sottostringa | `"ciao".indexOf("a")` | `2` |
| `slice(inizio, fine)` | estrae una porzione di stringa (fine escluso) | `"ciao".slice(1, 3)` | `"ia"` |
| `split(separatore)` | divide la stringa in un array | `"a,b,c".split(",")` | `["a", "b", "c"]` |

## Esercizi

#### Esercizio 1:
Scrivi un programma JavaScript che utilizzando `console.log` e il carattere asterisco legga 3 numeri e generi il relativo istogramma
Es: input 3, 5, 6
{% highlight shell %}
***
*****
******
{% endhighlight %}

Suggerimento: `"#".repeat(3)` restituisce 3 volte il carattere `#`.

#### Esercizio 2:
Scrivi un programma JavaScript che, date due stringhe, le scriva in ordine di lunghezza (prima la più corta).

#### Esercizio 3:
Scrivi un programma JavaScript che, data una stringa, ne calcoli la lunghezza e la riscriva tante volte quanto è la sua lunghezza.

#### Esercizio 4:
Scrivi un programma JavaScript che, date due stringhe, le scriva in ordine alfabetico (con le stringhe si possono utilizzare gli operatori `<` e `>`).

#### Esercizio 5:
Scrivi un programma JavaScript che, date tre stringhe, le scriva in ordine alfabetico.

#### Esercizio 6:
Scrivi un programma JavaScript che, dati un numero intero n e una stringa s, scriva la stringa s solo se la sua lunghezza è maggiore di n.

#### Esercizio 7:
Scrivi un programma JavaScript che visiti una stringa di testo e scriva sul display "vocale" ogni volta che incontra una vocale e "consonante" ogni volta che incontra una consonante.

#### Esercizio 8:
Scrivi un programma JavaScript che legga due stringhe di testo e componga una nuova stringa di testo alternando i caratteri delle stringhe iniziali.
Esempio
Str1: casa
Str2: rosa
Output: craossaa

#### Esercizio 9:
Scrivi un programma JavaScript che, data una stringa di testo, conti quante vocali ci sono al suo interno.

#### Esercizio 10:
Scrivi un programma JavaScript che, data una stringa di testo, crei una nuova stringa che sostituisca tutte le S della stringa (maiuscole e minuscole) con il carattere $ e tutte le E (maiuscole e minuscole) con il carattere €.

#### Esercizio 11:
Scrivi un programma che, letto un numero, calcoli la somma delle cifre che lo compongono.
Es input = 124 output = 7

Suggerimento: puoi trasformare un numero in stringa con `String(numero)`, argomento della prossima pagina sulle [conversioni di tipo]({{ site.baseurl }}{% link _javascript/conversioni-di-tipo.md %}.html).

#### Esercizio 12:
Scrivi un programma che lette una stringa di testo `messaggio` ed un numero intero `k` (compreso tra 1 e 25) applichi alla stringa di testo `messaggio` l'algoritmo del cifrario di Cesare con chiave k.

### Esercizi di tracciamento

Per i seguenti esercizi non dovete scrivere codice: dovete costruire la **tabella di tracciamento** del programma, cioè una tabella che mostra riga per riga come cambia il valore di ogni variabile ad ogni iterazione del ciclo.

#### Esercizio 13:

Costruite la tabella di tracciamento del seguente programma (mostrate i valori di `parola`, `indice` e `acc` ad ogni iterazione):

{% highlight javascript %}
let parola = "js";
let indice = 0;
let acc = "";
while (indice < parola.length) {
    acc = parola[indice] + acc;
    indice = indice + 1;
}
console.log(acc);
{% endhighlight %}

Cosa calcola questo programma?

#### Esercizio 14:

Costruite la tabella di tracciamento del seguente programma (mostrate i valori di `frase`, `lettera` e `conta` ad ogni iterazione):

{% highlight javascript %}
let frase = "casa mia";
let conta = 0;
for (const lettera of frase) {
    if (lettera === "a") {
        conta = conta + 1;
    }
}
console.log(conta);
{% endhighlight %}

### Esercizi di ricerca degli errori (debugging)

Nei seguenti programmi è stato inserito un **errore di sintassi**. Provate prima a individuarlo leggendo il codice (senza eseguirlo), poi verificate eseguendolo in Node.js ed infine correggetelo.

#### Esercizio 15:

{% highlight javascript %}
let saluto = "ciao";
console.log(saluto[0]
{% endhighlight %}

#### Esercizio 16:

{% highlight javascript %}
let saluto = 'ciao";
console.log(saluto);
{% endhighlight %}

#### Esercizio 17:

Questo programma non contiene errori di sintassi (viene eseguito senza generare errori), ma contiene un **errore logico**: individuatelo e spiegate perché il ciclo non termina mai.

{% highlight javascript %}
let saluto = "ciao";
let indice = 0;
while (indice < saluto.length) {
    console.log(saluto[indice]);
}
{% endhighlight %}

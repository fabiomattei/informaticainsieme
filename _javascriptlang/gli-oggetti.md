---
title: 'Gli oggetti'
date: '2026-08-18T10:10:00+01:00'
author: Fabio Mattei
layout: page
---

Se le variabili sono cassetti e gli array sono cassettiere, gli **oggetti** sono cassettiere con etichette: sono l'equivalente JavaScript dei dizionari di Python e degli **array associativi** di PHP.

Un oggetto è una struttura dati che può accogliere molti dati. Ciascun dato è associato ad una etichetta, che chiameremo **proprietà** (o **chiave**), che ci permette di identificarlo in maniera univoca all'interno della struttura.

A differenza degli array associativi PHP, che sono comunque array (`array`), in JavaScript gli oggetti sono un **tipo a parte** (`object`), sintatticamente diverso dagli array (che pure sono, internamente, un caso particolare di oggetto).

#### Definire gli oggetti

Gli oggetti si definiscono elencando tra **parentesi graffe** una serie di coppie chiave-valore separate da **virgole**, dove ciascuna coppia è formata da una chiave e un valore separati dal simbolo **due punti** `:` (non dalla freccia `=>` di PHP).

{% highlight javascript %}
const o1 = {};                              // oggetto vuoto
const o2 = { a: 1 };                        // contenente un elemento
const o3 = { a: 1, b: 2, c: 3 };            // contenente 3 elementi

const ilMioOggetto = {
    lati: 6,
    parola: "Saluti",
    area: 4.3,
};
{% endhighlight %}

In questo esempio `o3` è un oggetto che contiene 3 proprietà. `a`, `b` e `c` sono le chiavi (scritte **senza apici**, se sono nomi validi), mentre 1, 2 e 3 sono i valori. Le chiavi di un oggetto sono sempre, internamente, delle stringhe; i valori possono essere di qualsiasi tipo, compresi altri oggetti o array.

{% highlight javascript %}
// valore array
const d = { studenti: ['Jack', 'Jane'], voti: [8, 9] };

// oggetto annidato
const persona = {
    nome: "Mario",
    indirizzo: {
        via: "Via Roma",
        citta: "Torino"
    }
};
{% endhighlight %}

#### Accedere alle proprietà di un oggetto

Una volta creato un oggetto, è possibile ottenere il valore di una proprietà in **due modi**: con la notazione **punto**, oppure con la notazione **parentesi quadre** (identica a quella degli array associativi PHP):

{% highlight javascript %}
const d = {
    a: 1,
    b: 2,
    c: 3,
    campana: 'rumorosa',
};

d.a;          // notazione punto: ritorna 1
d['a'];       // notazione parentesi quadre: ritorna 1, identico
d.campana;    // ritorna 'rumorosa'
d['campana']; // ritorna 'rumorosa', identico
{% endhighlight %}

La notazione punto è la più usata e leggibile, ma **richiede** che la chiave sia un nome valido e **noto in anticipo**. Se la chiave è contenuta in una variabile, o contiene spazi/simboli, serve necessariamente la notazione con le parentesi quadre:

{% highlight javascript %}
const chiave = 'campana';
console.log(d[chiave]);   // 'rumorosa': funziona, la chiave viene da una variabile
console.log(d.chiave);    // undefined: cerca una proprietà letteralmente chiamata "chiave", che non esiste!
{% endhighlight %}

Se viene specificata una chiave inesistente, JavaScript restituisce semplicemente `undefined`, senza generare né un warning (come PHP) né un'eccezione (come il `KeyError` di Python):

{% highlight javascript %}
const d = { a: 1, b: 2, c: 3 };
console.log(d.x);   // undefined, nessun errore
{% endhighlight %}

#### L'operatore in

Per controllare se una specifica chiave è stata definita all'interno di un oggetto si usa l'operatore `in`, molto simile all'operatore `in` di Python (a differenza di PHP, che richiede la funzione `array_key_exists()`):

{% highlight javascript %}
if ('x' in d) { // la chiave 'x' è presente in d
    console.log("Chiave x definita");
}

if (!('x' in d)) { // la chiave 'x' non è presente in d
    console.log("Chiave x non definita");
}
{% endhighlight %}

Attenzione: `'x' in d` controlla solo la presenza della **chiave**, non il valore associato, esattamente come `array_key_exists()` in PHP.

#### Aggiungere e modificare le proprietà di un oggetto

È possibile aggiungere o modificare proprietà usando la notazione punto o quella con parentesi quadre. Come per gli array, questo è consentito anche se l'oggetto è dichiarato con `const`: stiamo modificando il contenuto, non riassegnando la variabile.

{% highlight javascript %}
// definisco un oggetto che contiene 3 proprietà
const d = { a: 1, b: 2, c: 3 };

// aggiungo una proprietà
d.k = 2020;
console.log(d);  // { a: 1, b: 2, c: 3, k: 2020 }

// modifico una proprietà
d.a = 123;
console.log(d);  // { a: 123, b: 2, c: 3, k: 2020 }
{% endhighlight %}

#### Rimuovere una proprietà da un oggetto

È possibile rimuovere una proprietà usando l'operatore **delete**, con la sintassi: **delete oggetto.chiave** (l'equivalente di `unset()` in PHP):

{% highlight javascript %}
const d = { a: 1, b: 2, c: 3 };

// rimuove la proprietà (chiave e valore) 'a'
delete d.a;
console.log(d);  // { b: 2, c: 3 }
{% endhighlight %}

#### Visita di un oggetto

Visitare un oggetto significa utilizzare un ciclo per scandire tutte le proprietà che sono al suo interno al fine di fare con queste delle operazioni.

Per visitare **solo le chiavi**, usiamo il ciclo `for...in`, già introdotto in [Il ciclo for]({{ site.baseurl }}{% link _javascriptlang/il-ciclo-for.md %}.html) — a differenza degli array, dove `for...in` è quasi sempre sconsigliato, sugli oggetti è l'uso più comune e corretto:

{% highlight javascript %}
const statiECapitali = {
    Piemonte: 'Torino',
    Lombardia: 'Milano',
    Lazio: 'Roma',
    Sicilia: 'Palermo'
};

console.log("Lista delle regioni:");

for (const regione in statiECapitali) {
    console.log(regione); // scrive Piemonte, Lombardia, Lazio, Sicilia
}
{% endhighlight %}

Se volessi ottenere, all'interno del ciclo, sia le chiavi sia i valori a queste associati, posso usare la chiave per accedere al valore, oppure usare `Object.entries()` insieme a `for...of`:

{% highlight javascript %}
console.log("Lista delle regioni e delle relative capitali:");

for (const regione in statiECapitali) {
    console.log(regione + ": " + statiECapitali[regione]);
}

// forma alternativa, con Object.entries() e destrutturazione
for (const [regione, capitale] of Object.entries(statiECapitali)) {
    console.log(regione + ": " + capitale);
}
{% endhighlight %}

`Object.entries()` trasforma un oggetto in un array di coppie `[chiave, valore]`, ciascuna delle quali viene poi **destrutturata** direttamente nelle variabili `regione` e `capitale`: è il modo più moderno e leggibile, ma è del tutto normale iniziare con la forma `for...in` più esplicita.

## Esercizi

#### Esercizio 1:
- definisci un oggetto "persona1" che al suo interno abbia due proprietà, `nome` con valore "Mario" e `cognome` con valore "Serenelli".
- definisci un oggetto "persona2" che al suo interno abbia due proprietà, `nome` con valore "Maria" e `cognome` con valore "Giacobini".
- aggiungi a persona1 la proprietà `indirizzo` con valore "Via Giuseppe Verdi"
- scrivi un ciclo che permetta di scrivere tutto il contenuto di persona1
- scrivi un ciclo che permetta di scrivere tutto il contenuto di persona2
- definisci un array che contenga persona1 e persona2 appena definiti.

#### Esercizio 2:
Scrivi un algoritmo che unisca due oggetti creandone uno nuovo che contenga tutte le proprietà del primo e tutte quelle del secondo. Suggerimento: puoi usare l'operatore **spread**, `{...oggetto1, ...oggetto2}`, che "spacchetta" le proprietà di un oggetto dentro un altro.
dic1 = { a: 10, b: 20 }
dic2 = { c: 30, d: 40 }
Risultato atteso: { a: 10, b: 20, c: 30, d: 40 }

#### Esercizio 3:
Scrivi un programma JavaScript che, dato un numero n, generi un oggetto le cui proprietà abbiano la forma x: x*x:
Esempio (n = 5):
Output: { 1: 1, 2: 4, 3: 9, 4: 16, 5: 25 }

#### Esercizio 4:
Scrivi uno script JavaScript che controlli se due oggetti hanno le stesse chiavi (tutte le chiavi del primo sono definite nel secondo e viceversa). Suggerimento: `Object.keys(oggetto)` restituisce un array con tutte le chiavi.

#### Esercizio 5:
Scrivi uno script JavaScript che, ricevuto un oggetto i cui valori sono tutti numeri interi, trovi il massimo valore e il minimo valore.

#### Esercizio 6:
Scrivi un programma JavaScript che controlli se un oggetto è vuoto oppure no (suggerimento: `Object.keys(oggetto).length === 0`).

#### Esercizio 7:
Crea un array di oggetti che descriva il seguente problema di Knapsack:
m1: peso 23 valore 54
m2: peso 27 valore 59
m3: peso 19 valore 40
m4: peso 26 valore 57

#### Esercizio 8:

dato l'oggetto:
{% highlight javascript %}
const voti = {
    Fisica: 8,
    Matematica: 6,
    Storia: 7
};
{% endhighlight %}

Scrivere il nome della disciplina con il voto più basso.

### Esercizi di tracciamento

Per i seguenti esercizi non dovete scrivere codice: dovete costruire la **tabella di tracciamento** del programma, mostrando come cambia il contenuto dell'oggetto (o delle variabili) ad ogni iterazione o istruzione.

#### Esercizio 9:

Costruite la tabella di tracciamento del seguente programma (mostrate il contenuto di `d` dopo ogni riga):

{% highlight javascript %}
const d = { a: 1, b: 2 };
d.c = 3;
d.a = 10;
delete d.b;
{% endhighlight %}

#### Esercizio 10:

Costruite la tabella di tracciamento del seguente programma (mostrate i valori di `chiave` e `acc` ad ogni iterazione):

{% highlight javascript %}
const voti = { Fisica: 8, Matematica: 6, Storia: 7 };
let acc = 0;
for (const chiave in voti) {
    acc = acc + voti[chiave];
}
console.log(acc);
{% endhighlight %}

### Esercizi di ricerca degli errori (debugging)

Nei seguenti programmi è stato inserito un errore. Provate prima a individuarlo leggendo il codice (senza eseguirlo), poi verificate eseguendolo in Node.js ed infine correggetelo.

#### Esercizio 11:

Questo programma genera un errore di sintassi.

{% highlight javascript %}
const d = { a: 1, b: 2, c: 3 };
if (!('x' in d) {
    console.log('Chiave x non definita');
}
{% endhighlight %}

#### Esercizio 12:

Questo programma non genera un errore (a differenza di Python, che qui genererebbe un `KeyError` bloccante): individuate quale istruzione stampa un valore inatteso e spiegate perché.

{% highlight javascript %}
const d = { a: 1, b: 2, c: 3 };
console.log(d.x);
{% endhighlight %}

#### Esercizio 13:

Questo programma non genera errori ma contiene un **errore logico**: dovrebbe contare quante chiavi ha l'oggetto, ma il valore stampato è sempre sbagliato. Individuate l'errore.

{% highlight javascript %}
const d = { a: 1, b: 2, c: 3 };
let conta = 0;
for (const chiave in d) {
    conta = 1;
}
console.log(conta);
{% endhighlight %}

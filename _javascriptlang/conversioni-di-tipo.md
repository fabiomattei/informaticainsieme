---
title: 'Conversioni di tipo'
date: '2026-08-18T09:25:00+01:00'
author: Fabio Mattei
layout: page
---

## Perché convertire i tipi

In JavaScript ogni valore ha un tipo: `number`, `string`, `boolean` e così via, come visto nella pagina sui [tipi di variabile]({{ site.baseurl }}{% link _javascriptlang/tipi-di-variabile.md %}.html). A volte è necessario trasformare un valore da un tipo a un altro. Il caso più comune riguarda i dati che arrivano da fuori il programma (un form HTML, come vedremo in [Il DOM]({{ site.baseurl }}{% link _javascriptlang/il-dom.md %}.html)): sono **sempre stringhe**, anche se rappresentano un numero.

{% highlight javascript %}
let testo = "42";
console.log(typeof testo);   // string
let numero = Number(testo);
console.log(typeof numero);  // number
{% endhighlight %}

L'operatore `typeof` restituisce il tipo di un valore ed è utile per fare debug (è l'equivalente di `gettype()` in PHP e di `type()` in Python).

---

## Number() — da stringa o booleano a numero

`Number(x)` converte `x` in un valore numerico. A differenza di PHP, dove `(int) "3.14"` legge silenziosamente solo il prefisso valido, `Number()` è **tutto o niente**: se l'intera stringa non è un numero valido, il risultato è `NaN` ("Not a Number").

#### Esercizio 1
Copia il seguente codice nell'editor ed eseguilo.

{% highlight javascript %}
console.log(Number("42"));     // 42
console.log(Number("3.14"));   // 3.14
console.log(Number("-7"));     // -7
console.log(Number("3.14abc")); // NaN, a differenza di PHP che restituirebbe 3
console.log(Number(""));       // 0 (attenzione a questo caso speciale!)
console.log(Number(true));     // 1
console.log(Number(false));    // 0
{% endhighlight %}

`NaN` è un valore particolare: rappresenta il risultato di un'operazione numerica non valida. Attenzione: `NaN` **non è mai uguale a se stesso** (`NaN === NaN` restituisce `false`!). Per verificare se un valore è `NaN` si usa la funzione `Number.isNaN()`:

{% highlight javascript %}
let risultato = Number("abc");
console.log(risultato === NaN);        // false, sempre! (trappola comune)
console.log(Number.isNaN(risultato));  // true, modo corretto per verificare
{% endhighlight %}

Esistono anche due funzioni più permissive, `parseInt()` e `parseFloat()`, che si comportano più simili al cast `(int)` di PHP: leggono i caratteri numerici iniziali e si fermano al primo carattere non valido.

{% highlight javascript %}
console.log(parseInt("42px"));    // 42 (si ferma a "px", come farebbe PHP)
console.log(parseFloat("3.14abc")); // 3.14
console.log(Number("42px"));      // NaN (Number() invece è tutto o niente)
{% endhighlight %}

---

## String() — da numero o booleano a stringa

`String(x)` converte `x` nella sua rappresentazione testuale.

#### Esercizio 2
Copia il seguente codice nell'editor ed eseguilo.

{% highlight javascript %}
let eta = 25;
console.log("Ho " + eta + " anni");          // Ho 25 anni (conversione automatica!)
console.log("Ho " + String(eta) + " anni");  // stesso risultato, conversione esplicita
{% endhighlight %}

A differenza di Python, dove `"Ho " + 25 + " anni"` genera un `TypeError`, in JavaScript l'operatore `+` converte automaticamente i numeri in stringa quando almeno uno dei due operandi è già una stringa. Questo rende la conversione a `string` meno indispensabile che in Python, esattamente come avviene per `.` in PHP, ma è comunque buona pratica renderla esplicita quando il tipo non è ovvio a chi legge il codice. Meglio ancora: usare i **template literal** visti nella pagina sulle [stringhe]({{ site.baseurl }}{% link _javascriptlang/stringhe.md %}.html), che rendono la conversione ancora più leggibile:

{% highlight javascript %}
let eta = 25;
console.log(`Ho ${eta} anni`);   // Ho 25 anni
{% endhighlight %}

---

## Boolean() — da qualsiasi valore a booleano

`Boolean(x)` converte `x` in `true` o `false` secondo le regole di **truthiness** di JavaScript: quasi tutto è `true`, tranne un piccolo insieme di valori considerati "falsy".

#### Esercizio 3
Copia il seguente codice nell'editor ed eseguilo.

{% highlight javascript %}
console.log(Boolean(0));          // false
console.log(Boolean(""));         // false (stringa vuota)
console.log(Boolean(null));       // false
console.log(Boolean(undefined));  // false
console.log(Boolean(NaN));        // false

console.log(Boolean(1));          // true
console.log(Boolean(-5));         // true
console.log(Boolean("ciao"));     // true
console.log(Boolean("0"));        // true! caso diverso da PHP
console.log(Boolean([]));         // true! caso diverso da PHP
{% endhighlight %}

Attenzione a due trappole rispetto a PHP: la stringa `"0"` è `true` in JavaScript (mentre in PHP `(bool) "0"` vale `false`), e **anche un array vuoto è `true`** (mentre in PHP `(bool) []` vale `false`). In JavaScript, tra i valori "falsy" ci sono solo: `0`, `""`, `null`, `undefined`, `NaN` e `false` stesso — gli array e gli oggetti, anche vuoti, sono sempre `true`.

---

## Tabella riepilogativa

| Funzione | Converte a | Note |
|---|---|---|
| `Number(x)` | `number` | "tutto o niente": restituisce `NaN` se `x` non è interamente numerico |
| `parseInt(x)` / `parseFloat(x)` | `number` | Come `Number()`, ma legge solo il prefisso numerico valido |
| `String(x)` | `string` | Spesso implicita con l'operatore `+` |
| `Boolean(x)` | `boolean` | `false` solo per `0`, `""`, `null`, `undefined`, `NaN`, `false` |
| `typeof x` | `string` | Restituisce il nome del tipo di `x` (senza parentesi: è un operatore, non una funzione) |
| `Number.isNaN(x)` | `boolean` | Unico modo corretto per verificare se `x` è `NaN` |

---

## Esercizi

#### Esercizio 4
Scrivi un programma che legga (o definisca come stringhe) due numeri e stampi la loro somma, differenza, prodotto e quoziente, dopo averli convertiti correttamente con `Number()`.

#### Esercizio 5
Scrivi un programma che, dato un numero decimale, stampi separatamente la parte intera (`Math.floor()`) e la parte decimale.

#### Esercizio 6
Scrivi un programma che chieda (o definisca) un carattere e stampi il suo codice Unicode con `charCodeAt()`, l'equivalente JavaScript di `ord()` in PHP. Se il carattere è una lettera minuscola, stampa anche la corrispondente lettera maiuscola usando `String.fromCharCode()`, l'equivalente di `chr()`.

#### Esercizio 7
Scrivi un programma che legga il nome dell'utente e la sua età come stringa, poi costruisca e stampi la frase:
`"Tra 10 anni, Nome avrà X anni."`
dove X è l'età aumentata di 10, dopo essere stata convertita in numero.

#### Esercizio 8
Scrivi un programma che, dati tre voti (numeri decimali), stampi la media arrotondata a due cifre decimali (`Math.round()`) e il valore intero della media (`Math.floor()`).

### Esercizi di tracciamento

Per i seguenti esercizi non dovete scrivere codice: dovete costruire la **tabella di tracciamento** del programma, indicando per ogni variabile sia il **valore** sia il **tipo** dopo ogni riga di codice.

#### Esercizio 9:

Costruite la tabella di tracciamento del seguente programma (mostrate valore e tipo di `x` dopo ogni riga):

{% highlight javascript %}
let x = "10";
x = Number(x);
x = x + 5;
x = String(x);
x = x + " anni";
{% endhighlight %}

#### Esercizio 10:

Costruite la tabella di tracciamento del seguente programma (mostrate valore e tipo di `n` dopo ogni riga):

{% highlight javascript %}
let n = 7.9;
n = Math.floor(n);
n = n / 2;
{% endhighlight %}

### Esercizi di ricerca degli errori (debugging)

Nei seguenti programmi è stato inserito un errore. Provate prima a individuarlo leggendo il codice (senza eseguirlo), poi verificate eseguendolo in Node.js ed infine correggetelo.

#### Esercizio 11:

Questo programma non genera un errore, ma il valore stampato non è quello che ci si aspetterebbe: individuate perché.

{% highlight javascript %}
let testo = "3.14abc";
let numero = Number(testo);
console.log(numero);
{% endhighlight %}

#### Esercizio 12:

Questo programma non genera errori ma contiene un **errore logico**: il risultato stampato è `false`, ma il programmatore si aspettava `true`. Individuate l'errore.

{% highlight javascript %}
let risultato = Number("testo");
console.log(risultato === NaN);
{% endhighlight %}

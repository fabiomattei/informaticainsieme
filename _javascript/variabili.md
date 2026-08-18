---
title: Variabili
date: '2026-08-18T09:10:00+01:00'
author: Fabio Mattei
layout: page
---

Le variabili sono **contenitori di informazioni**. Possiamo pensare ad una variabile come ad un cassetto, dotato di etichetta, che può contenere una informazione. Lo pensiamo come un cassetto dotato di etichetta perché ogni variabile ha un nome, il nome ci serve per ricordare cosa contiene e per trovarla facilmente tra le tante variabili che andremo a creare per ciascun nostro programma.

A differenza di PHP, dove ogni variabile inizia con il simbolo `$`, in JavaScript **il nome della variabile è semplice testo**, molto simile a Python. La differenza rispetto a Python è che in JavaScript, per creare una variabile, dobbiamo dichiararla usando una delle parole chiave `let`, `const` o `var`.

{% highlight javascript %}
let x = 5;
let y = "John";
let z = 3.564;
{% endhighlight %}

Facciamo bene attenzione, questa non è una equazione matematica ma una assegnazione, si legge in questo modo: **dichiaro la variabile x e le assegno il numero 5**. Significa che nella memoria del computer verrà conservato uno spazio (il cassetto) che avrà come etichetta **x**. All'interno di questo spazio sarà posto il **numero 5**.

Quindi ricordiamo:
* la parola chiave `let` (o `const`, o `var`) **dichiara** la variabile
* il simbolo alla sinistra del simbolo = è il **nome della variabile**
* il simbolo alla destra del simbolo = il **valore da conservare** all'interno della variabile
* ogni istruzione va terminata con **;**

## let, const e var: tre modi per dichiarare una variabile

JavaScript mette a disposizione tre parole chiave diverse, e questa è una delle prime cose che confonde chi arriva da PHP o Python, dove esiste un solo modo di creare una variabile.

* **`let`** — dichiara una variabile il cui valore **può cambiare** in seguito. È la scelta di default per la maggior parte delle variabili.
* **`const`** — dichiara una variabile il cui valore **non può più essere riassegnato** dopo la prima assegnazione. Va usata ogni volta che sappiamo già che quel valore non dovrà cambiare.
* **`var`** — è il modo **storico**, presente fin dalla prima versione del linguaggio (1995). Ha un comportamento più permissivo e meno prevedibile rispetto a `let` (lo vedremo parlando di funzioni): nel codice moderno **si evita**, e in questo corso non la useremo. La incontrerai comunque leggendo codice più vecchio.

{% highlight javascript %}
let eta = 15;
eta = 16;          // corretto: let permette di riassegnare

const nome = "Mario";
nome = "Luigi";     // ERRORE! non si può riassegnare una const
{% endhighlight %}

Un dettaglio importante: `const` impedisce di **riassegnare** la variabile, ma se il valore è un array o un oggetto (li vedremo più avanti) il **contenuto** può comunque essere modificato. `const` blocca l'etichetta sul cassetto, non impedisce di rovistare dentro il cassetto.

## Regole per i nomi delle variabili

* può contenere lettere, cifre, underscore (`_`) e dollaro (`$`)
* non può iniziare con una cifra
* è **case-sensitive** (`eta` e `Eta` sono due variabili diverse)
* per convenzione, i nomi composti da più parole si scrivono in **camelCase** (es. `numeroAlunni`), non con l'underscore come in PHP e Python (`numero_alunni`)

## Esercizi

#### Esercizio 1:
Copia il seguente codice nell'editor. Una volta finito eseguilo da riga di comando con `node nomefile.js`.

{% highlight javascript %}
let nome = "Mario";
let eta = 15;
console.log(nome);
console.log(eta);
{% endhighlight %}

#### Esercizio 2:
Crea tre variabili: il tuo nome, la tua età e la classe che frequenti, quindi stampale a video usando `console.log`.

#### Esercizio 3:
Crea due variabili numeriche `a` e `b`, scambia i loro valori (senza indovinare i valori a mano, usando una terza variabile di appoggio) e stampali.

#### Esercizio 4:
Prova a scrivere questo programma ed eseguilo: cosa succede, e perché?

{% highlight javascript %}
const punteggio = 0;
punteggio = punteggio + 10;
console.log(punteggio);
{% endhighlight %}

### Esercizi di tracciamento

Per i seguenti esercizi non dovete scrivere codice: dovete costruire la **tabella di tracciamento** del programma, cioè una tabella che mostra come cambia il valore di ogni variabile ad ogni istruzione eseguita.

#### Esercizio 5:

Costruite la tabella di tracciamento del seguente programma (mostrate il valore di `x` dopo ogni riga):

{% highlight javascript %}
let x = 3;
x = x + 2;
x = x * 2;
{% endhighlight %}

#### Esercizio 6:

Costruite la tabella di tracciamento del seguente programma (mostrate i valori di `a` e `b` dopo ogni riga):

{% highlight javascript %}
let a = 2;
let b = 5;
a = b;
b = a;
{% endhighlight %}

Cosa noti di strano nel risultato finale? Come lo risolveresti?

### Esercizi di ricerca degli errori (debugging)

Nei seguenti programmi è stato inserito un **errore**. Provate prima a individuarlo leggendo il codice (senza eseguirlo), poi verificate eseguendolo in Node.js ed infine correggetelo.

#### Esercizio 7:

{% highlight javascript %}
x = 5;
console.log(x);
{% endhighlight %}

Questo codice non genera un errore di sintassi, ma in modalità normale crea comunque una variabile globale implicita: è comunque considerato un errore grave, perché senza `let`/`const` la variabile sfugge a qualunque controllo. Riscrivilo dichiarando correttamente `x`.

#### Esercizio 8:

{% highlight javascript %}
let nome = "Giacomo;
console.log(nome);
{% endhighlight %}

#### Esercizio 9:

{% highlight javascript %}
const eta = 15;
eta = 16;
console.log(eta);
{% endhighlight %}

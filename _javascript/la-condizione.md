---
title: 'La condizione'
date: '2026-08-18T09:40:00+01:00'
author: Fabio Mattei
layout: page
---

## Il costrutto if

Il costrutto **if** ci consente di cambiare la sequenza logica di istruzioni da eseguire in un programma. Il blocco di istruzioni racchiuso tra graffe viene eseguito se la condizione risulta verificata.

#### Esercizio 1:
copia il seguente codice nell'editor. Una volta finito eseguilo.

{% highlight javascript %}
let a = 33;
let b = 200;
if (b > a) {
    console.log("b e' maggiore di a");
}
{% endhighlight %}

Esattamente come in PHP, la condizione va racchiusa tra **parentesi tonde**, e il blocco di istruzioni da eseguire va racchiuso tra **parentesi graffe** `{ }`. L'indentazione (gli spazi che precedono l'istruzione `console.log`) in JavaScript **non ha alcun significato per l'interprete**: serve solo a rendere il codice leggibile per noi umani. Sono le graffe a delimitare i blocchi di codice, non gli spazi — a differenza di Python, dove l'indentazione è obbligatoria e sintatticamente significativa.

#### Esercizio 2:
copia il seguente codice nell'editor. Una volta finito eseguilo.

{% highlight javascript %}
let a = 33;
let b = 200;
if (b > a)
    console.log("b e' maggiore di a");
{% endhighlight %}

Come in PHP, questo codice è perfettamente valido: se il blocco `if` contiene una sola istruzione, le graffe sono facoltative e quell'unica istruzione appartiene comunque all'if. È comunque buona pratica usare sempre le graffe, anche per un blocco di una sola riga, per evitare errori quando in futuro si aggiungono altre istruzioni.

#### Operatori di confronto

Sono gli operatori che possiamo utilizzare all'interno di una condizione.

- Uguale (valore, con conversione automatica di tipo): `a == b`
- Identico (valore e tipo, **senza** conversione): `a === b`
- Diverso (con conversione): `a != b`
- Diverso (senza conversione): `a !== b`
- Minore: `a < b`
- Minore o uguale: `a <= b`
- Maggiore: `a > b`
- Maggiore o uguale: `a >= b`

`===` esiste anche in PHP con lo stesso significato, ma in JavaScript è ancora più importante: `==` applica delle regole di conversione automatica tra tipi diversi che sono spesso **sorprendenti**.

{% highlight javascript %}
console.log(5 == "5");    // true! "5" viene convertita in numero
console.log(5 === "5");   // false, tipi diversi (number vs string)
console.log(0 == false);  // true! false viene convertito in 0
console.log(0 === false); // false, tipi diversi
console.log("" == false); // true! entrambi "falsy"
{% endhighlight %}

Per questo motivo, **in JavaScript si usa quasi sempre `===` e `!==`**, mai `==` e `!=`, per evitare queste conversioni implicite: è una regola ancora più stringente di quella vista per `===` in PHP, dove `==` resta comunque un confronto ragionevole tra valori dello stesso tipo logico.

## Il costrutto else if

Importante per dare un'alternativa in caso la proposizione contenuta nel primo if non venga verificata. Si scrive **in due parole separate**: `else if` (a differenza di PHP, dove si scrive tutto attaccato: `elseif`).

#### Esercizio 3:
copia il seguente codice nell'editor. Una volta finito eseguilo.

{% highlight javascript %}
let a = 33;
let b = 33;
if (b > a) {
    console.log("b e' piu' grande di a");
} else if (a === b) {
    console.log("a e b sono uguali");
}
{% endhighlight %}

## Il costrutto else

Nel caso nessuna delle proposizioni contenute nelle condizioni precedenti sia stata verificata, si esegue il codice contenuto nel blocco else:

#### Esercizio 4:
copia il seguente codice nell'editor. Una volta finito eseguilo.

{% highlight javascript %}
let a = 33;
let b = 33;
if (b > a) {
    console.log("b e' piu' grande di a");
} else if (a === b) {
    console.log("a e b sono uguali");
} else {
    console.log("a e' piu' grande di b");
}
{% endhighlight %}

#### Esercizio 5:
scrivi un programma che lette due stringhe di testo ne scriva la prima in ordine alfabetico

#### Esercizio 6:
scrivi un programma che letti due numeri scriva il più grande tra i due

#### Esercizio 7:
scrivi un programma che letto un numero intero determini se è pari o dispari (utilizzare l'operatore resto: `%`)

#### Esercizio 8:
Dati 4 numeri determinare se la somma dei primi due è minore o uguale alla somma del terzo e del quarto

#### Esercizio 9:
Un'automobile percorre 20 km con un litro di benzina. Calcolare la spesa necessaria a percorrere 100 km. Se la spesa è maggiore di €100, applicare uno sconto del 5%

#### Esercizio 10:
Letti i lati di un triangolo determinare se è scaleno, isoscele o equilatero

#### Esercizio 11:
Letti due numeri naturali A e B, sottrarre il più piccolo dal più grande.

#### Esercizio 12:
Letti due numeri determinare se sono entrambi compresi tra 100 e 200

#### Esercizio 13:
Letto un numero intero, trovare il suo valore assoluto (funzione `Math.abs()`).

#### Esercizio 14:
Letti due numeri interi A e B verificare se A è il quadrato di B

#### Esercizio 15:
Un'azienda elettrica ha stabilito le seguenti tariffe:

| KW/H         |                                                                                   |
|--------------|-----------------------------------------------------------------------------------|
| 0 – 500      | 20                                                                                |
| 501 – 1000   | 20 + 0,08 per ogni KW/H oltre i 500                                               |
| 1001 – oltre | 20 + 0,08 per ogni KW/H compreso tra 500 e 1000 + 0,05 per ogni KW/H oltre i 1000 |

Scrivere un programma che letto il consumo mensile calcoli e stampi l'importo della bolletta.

#### Esercizio 16:
Scrivi un programma JavaScript che legga il valore di una spesa e calcoli lo sconto secondo la seguente tabella:

| Spesa                |  Sconto        |
|----------------------|----------------|
| Al di sotto di 100 € | nessuno sconto |
| Tra 100 e 300        | sconto del 10% |
| Tra i 300 e i 500    | sconto del 15% |
| Tra i 500 e i 800    | sconto del 20% |

#### Esercizio 17:
Tenendo conto degli scaglioni fiscali definiti correntemente:

| Reddito         | Aliquota |
|-----------------|----------|
| 0 - 15000 €     | 23%      |
| 15001 - 28000 € | 25%      |
| 28001 - 50000 € | 35%      |
| 50001 in su   € | 43%      |

Scrivere un programma che letto il reddito di 5 cittadini italiani calcoli l'ammontare delle tasse che ciascun cittadino deve pagare ed il totale pagato da tutti i cittadini.

### Esercizi di tracciamento

Per i seguenti esercizi non dovete eseguire il codice: dovete dire quale blocco di istruzioni viene eseguito e cosa viene stampato sulla console, motivando la risposta in base al valore delle variabili.

#### Esercizio 18:

Dato il seguente programma, indicate cosa viene stampato quando `a = 15` e quando `a = 25`:

{% highlight javascript %}
let a = 15;
if (a < 10) {
    console.log("piccolo");
} else if (a < 20) {
    console.log("medio");
} else {
    console.log("grande");
}
{% endhighlight %}

#### Esercizio 19:

Costruite la tabella di tracciamento del seguente programma (mostrate i valori di `a`, `b` e `messaggio` dopo ogni riga), sapendo che `a = 7` e `b = 12`:

{% highlight javascript %}
let a = 7;
let b = 12;
let messaggio;
if (a > b) {
    messaggio = "a maggiore";
} else if (a === b) {
    messaggio = "uguali";
} else {
    messaggio = "b maggiore";
}
console.log(messaggio);
{% endhighlight %}

Nota: `let messaggio;` dichiara la variabile senza assegnarle un valore; il suo valore iniziale è `undefined` finché non viene assegnato dentro l'if.

### Esercizi di ricerca degli errori (debugging)

Nei seguenti programmi è stato inserito un **errore di sintassi**. Provate prima a individuarlo leggendo il codice (senza eseguirlo), poi verificate eseguendolo in Node.js ed infine correggetelo.

#### Esercizio 20:

{% highlight javascript %}
let a = 33;
let b = 200;
if (b > a) {
    console.log("b e' maggiore di a");
{% endhighlight %}

#### Esercizio 21:

{% highlight javascript %}
let a = 33;
let b = 33;
if (b > a) {
    console.log("b e' piu' grande di a");
} elseif (a === b) {
    console.log("a e b sono uguali");
}
{% endhighlight %}

#### Esercizio 22:

{% highlight javascript %}
let a = 33;
let b = 33;
if (b > a) {
    console.log("b e' piu' grande di a");
} else if (a === b) {
    console.log("a e b sono uguali");
else {
    console.log("a e' piu' grande di b");
}
{% endhighlight %}

#### Esercizio 23:

Questo programma non contiene errori di sintassi (viene eseguito senza generare errori), ma contiene un **errore logico**: individuatelo e spiegate perché il messaggio stampato è sbagliato quando `voto` vale, ad esempio, 6.

{% highlight javascript %}
let voto = 6;
if (voto > 6) {
    console.log("sufficiente");
} else {
    console.log("insufficiente");
}
{% endhighlight %}

#### Esercizio 24:

Questo programma non genera un errore di sintassi, ma il risultato è sempre `false`, anche quando dovrebbe essere `true`: individuate l'errore, ricordando la differenza tra `==` e `===`.

{% highlight javascript %}
let eta = "18";
if (eta === 18) {
    console.log("maggiorenne");
} else {
    console.log("minorenne");
}
{% endhighlight %}

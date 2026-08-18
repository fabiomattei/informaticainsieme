---
title: 'Tipi di variabile'
date: '2026-08-18T09:15:00+01:00'
author: Fabio Mattei
layout: page
---

Una informazione, conservata in una variabile, ha sempre un tipo associato. **Il tipo della variabile determina l'insieme di valori che una variabile può assumere e le operazioni che possono manipolare tali valori**.

- numeri (`number`) Es: 1, 5, 7, 1983, 3.14
- valori booleani (`boolean`) Es: true, false
- valori stringa (`string`) Es: "ciao", 'buongiorno'
- array (`object`, un caso speciale) Es: [1, 2, 3]
- oggetti (`object`) Es: {nome: "Mario"}
- il valore `undefined` (variabile dichiarata ma senza valore)
- il valore `null` (assenza di valore, assegnata volutamente)

A differenza di PHP e Python, che distinguono `int` e `float`, JavaScript ha **un solo tipo numerico**: `number`. Non esiste distinzione tra numeri interi e numeri a virgola mobile: `3` e `3.0` sono, internamente, esattamente lo stesso valore.

## Operazioni sui numeri

Per assegnare un numero ad una variabile basta utilizzare l'operatore `=` come visto nella pagina dedicata alle [variabili]({{ site.baseurl }}{% link _javascript/variabili.md %}.html).

{% highlight javascript %}
let m = 3;      // number
let n = 3.0;    // number, stesso tipo, stesso valore di 3
{% endhighlight %}

Le operazioni possibili su una variabile numerica sono:

- `m + m` → somma
- `m * m` → prodotto
- `m / m` → divisione (restituisce sempre un valore decimale se il risultato non è esatto)
- `Math.floor(m / m)` → quoziente intero (non esiste un operatore dedicato come `intdiv` di PHP)
- `m % m` → modulo (resto della divisione)
- `m ** m` → potenza

#### Esercizio 1:
copia il seguente codice nell'editor. Una volta finito eseguilo con `node nomefile.js`.

{% highlight javascript %}
let a = 2;
let b = 3;
let area = a * b;
let perimetro = a * 2 + b * 2;
console.log(area, perimetro);
{% endhighlight %}

Nota come `console.log` accetti **più argomenti separati da virgola**, stampandoli tutti sulla stessa riga separati da uno spazio: molto simile a `print` di Python, diverso da `echo` di PHP.

#### Operazioni sulle stringhe di testo

In informatica una stringa di testo è **una sequenza di caratteri con un ordine stabilito**. Facciamo qualche esempio:

{% highlight javascript %}
let m = "Prova";
let n = "casa";
{% endhighlight %}

Le operazioni possibili su una variabile stringa sono:

- `m + n` → concatena la stringa m ed n (es. "Provacasa"). A differenza di PHP, che usa il punto `.`, in JavaScript l'operatore di concatenazione è il **più** `+`, lo stesso usato per la somma tra numeri
- `m.repeat(3)` → concatena 3 volte la stringa m (es. "ProvaProvaProva")
- `m.length` → restituisce la lunghezza di m (es. 5). Nota: **non** è una funzione con le parentesi come `strlen()` di PHP, è una **proprietà**, quindi si scrive senza `()`
- `m[0]···m[m.length-1]` → restituisce i singoli caratteri della stringa. es: (`m[0]` → P)

{% highlight javascript %}
let nome = "Giacomo";                  // assegnamento
let cognome = "Leopardi";              // assegnamento
let nomeCognome = nome + cognome;      // concatenazione "GiacomoLeopardi"
let nomeRipetuto = nome.repeat(3);     // ripetizione "GiacomoGiacomoGiacomo"
let lunghezza = nome.length;           // lunghezza 7 (proprietà, non funzione!)
let iniziale = nome[0];                // carattere G
{% endhighlight %}

#### Esercizio 2:
copia il seguente codice nell'editor ed eseguilo

{% highlight javascript %}
let a = " c i a o ";
let b = " mondo ";
let stringaConcatenata = a + b;
console.log(stringaConcatenata);
{% endhighlight %}

In questi esempi incontriamo `console.log`, che ci permette di stampare sul video il valore di una variabile.

#### Esercizio 3:
Stampare a video il perimetro di un quadrato avente lato l=4

#### Esercizio 4:
Stampare a video l'area di un quadrato avente lato l=5

#### Esercizio 5:
Stampare a video n volte, con n=10, la stringa s (es. con s="ciao" stamperà "ciaociaociaociaociaociaociaociaociaociao")

#### Esercizio 6:
Stampare a video una stringa lunga 4 caratteri al contrario (es. se s="lodi", il programma stampa "idol")

#### Esercizio 7:
Supponete di correre 10 km in 42 min e 42 sec. Stampate la vostra velocità media in km/minuto, km/h, miglia/minuto e miglia/h.

- Calcolate quanti secondi ci sono in 42 minuti e 42 secondi.
- A quante miglia corrispondono 10 km? (Suggerimento: ci sono 1,61 km in un miglio)
- La vostra velocità media è calcolata come distanza/tempo

### Esercizi di tracciamento

Per i seguenti esercizi non dovete scrivere codice: dovete costruire la **tabella di tracciamento** del programma, cioè una tabella che mostra come cambia il valore (ed eventualmente il tipo) di ogni variabile ad ogni istruzione eseguita.

#### Esercizio 8:

Costruite la tabella di tracciamento del seguente programma (mostrate il valore di `m` dopo ogni riga):

{% highlight javascript %}
let m = 3;
m = m + 1.5;
m = m * 2;
{% endhighlight %}

#### Esercizio 9:

Costruite la tabella di tracciamento del seguente programma (mostrate i valori di `nome`, `cognome` e `nomeCognome` dopo ogni riga):

{% highlight javascript %}
let nome = "Giacomo";
let cognome = "Leopardi";
let nomeCognome = nome + cognome;
nome = "Alessandro";
nomeCognome = nome + cognome;
{% endhighlight %}

#### Esercizio 10:

Costruite la tabella di tracciamento del seguente programma (mostrate i valori di `a`, `area` e `perimetro` dopo ogni riga):

{% highlight javascript %}
let a = 2;
let b = 3;
let area = a * b;
let perimetro = a * 2 + b * 2;
a = 5;
area = a * b;
{% endhighlight %}

### Esercizi di ricerca degli errori (debugging)

Nei seguenti programmi è stato inserito un errore. Provate prima a individuarlo leggendo il codice (senza eseguirlo), poi verificate eseguendolo in Node.js ed infine correggetelo.

#### Esercizio 11:

Questo programma genera un errore di sintassi.

{% highlight javascript %}
let a = 2;
let b = 3;
let area = a*b
console.log(area);
{% endhighlight %}

#### Esercizio 12:

{% highlight javascript %}
let nome = "Giacomo;
let cognome = "Leopardi";
console.log(nome + cognome);
{% endhighlight %}

#### Esercizio 13:

Questo programma genera un errore in esecuzione (`TypeError`): individuate quale istruzione lo causa e spiegate perché, ricordando che `length` è una proprietà e non un metodo.

{% highlight javascript %}
let nome = "Giacomo";
let lunghezza = nome.length();
console.log(lunghezza);
{% endhighlight %}

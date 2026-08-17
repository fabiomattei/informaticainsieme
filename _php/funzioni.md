---
title: Funzioni
date: '2026-08-17T10:15:00+01:00'
author: Fabio Mattei
layout: page
---

PHP è un linguaggio e in quanto tale è costituito da un vocabolario che comprende molte parole, tra queste ricordiamo echo, if, while e for. Attraverso queste parole il programmatore comunica all'interprete, e quest'ultimo al computer, le operazioni da svolgere.

Il vocabolario di PHP non è fisso ma estensibile: possiamo cioè definire nuove parole con nuovi significati. Queste vengono chiamate funzioni e raggruppano al loro interno una sequenza di istruzioni ordinata (algoritmo) da eseguire quando vengono invocate.

#### La mia prima funzione

Iniziamo con un esempio, definiamo una funzione hello che si limita a scrivere sul display alcune stringhe di testo:

{% highlight php %}
<?php
function hello() {
    echo "Ciao!\n";
    echo "Ciao!!!\n";
    echo "Ciao a te!\n";
}
{% endhighlight %}

La parola chiave **function** ci permette di definire una nuova funzione. Questa viene seguita dalla parola che vogliamo utilizzare come nome per la funzione, in questo caso hello. La parola hello viene seguita da una coppia di parentesi che si aprono e subito dopo si chiudono, vedremo in seguito qual'è il loro scopo.

Dato che, come abbiamo detto in precedenza, una funzione raggruppa al suo interno una sequenza di istruzioni da eseguire quando questa viene invocata, troviamo, al chiudersi delle parentesi, una **graffa** `{` e prima della fine della definizione la graffa di chiusura `}`: è il blocco di istruzioni da eseguire, esattamente come per `if`, `while` e `for`.

A questo punto l'interprete sa che quando incontra l'invocazione della funzione hello deve eseguire le istruzioni in essa contenute.

{% highlight php %}
<?php
echo "Precede la chiamata a funzione\n";
hello();
echo "Segue la chiamata a funzione\n";
{% endhighlight %}

Output:

{% highlight shell %}
Precede la chiamata a funzione
Ciao!
Ciao!!!
Ciao a te!
Segue la chiamata a funzione
{% endhighlight %}

Una delle finalità principali delle funzioni è quella di raggruppare istruzioni che vengono utilizzate molte volte. Se non fosse stata definita una funzione avreste dovuto copiare e incollare il codice al suo interno molte volte.

Definire funzioni aiuta ad essere più efficaci nella scrittura di codice dato che aiuta a tenere gli script più brevi.

## Parametri e argomenti

Molte delle istruzioni PHP viste in precedenza sono a loro volta delle funzioni (o costrutti simili). Quando chiamate `strlen()`, le passate un valore chiamato argomento scrivendolo tra parentesi.

{% highlight php %}
<?php
strlen("Io sono un argomento");
{% endhighlight %}

Proviamo a definire la funzione hello in questo modo:

{% highlight php %}
<?php
function hello($nome) {
    echo "Ciao " . $nome;
}
{% endhighlight %}

La definizione della funzione hello in questo programma ha un parametro chiamato $nome. Un parametro è una variabile in cui viene memorizzato un argomento nel momento in cui viene chiamata la funzione. Se la funzione viene chiamata passandole la stringa "Alice" come argomento, l'esecuzione del programma entra nella funzione e la variabile $nome viene impostata a "Alice". L'istruzione echo va dunque a stampare la stringa Ciao Alice. L'argomento, memorizzato nel parametro, viene dimenticato al terminare dell'esecuzione della funzione.

## I valori di ritorno

{% highlight php %}
<?php
$lunghezza = strlen('Ciao');
{% endhighlight %}

Nell'esempio in alto, si utilizza la funzione strlen, passandole come argomento la stringa 'Ciao' al fine di far calcolare la lunghezza della stringa passata. Il valore calcolato viene restituito al chiamante e da questi memorizzato nella variabile lunghezza.

In generale il valore che una funzione calcola viene chiamato valore di ritorno. Quando si crea una funzione è possibile specificare quale sia il valore di ritorno attraverso l'istruzione **return**. Facciamo un esempio:

{% highlight php %}
<?php
function raddoppia($numero) {
    return $numero * 2;
}
{% endhighlight %}

La funzione raddoppia accetta un parametro e restituisce il doppio dell'argomento che le viene passato.

Facciamo un secondo esempio:

{% highlight php %}
<?php
function pariodispari($numero) {
    if ($numero % 2 == 0) {
        return 'Pari';
    } else {
        return 'Dispari';
    }
}
{% endhighlight %}

La funzione pariodispari accetta un parametro e restituisce la stringa 'Pari' se l'argomento passato è pari e la stringa 'Dispari' in caso contrario. È possibile inserire due o più istruzioni di return in una funzione. Quando l'interprete incontra l'istruzione return esce dall'ambito della funzione e torna nel programma da cui questa era stata chiamata. Nessuna istruzione all'interno della funzione viene eseguita dopo il return.

{% highlight php %}
<?php
$primonumero = pariodispari(4);   // $primonumero = 'Pari'
$secondonumero = pariodispari(7); // $secondonumero = 'Dispari'
{% endhighlight %}

Se una funzione PHP termina senza incontrare un `return`, restituisce il valore speciale `null`, esattamente come le funzioni Python restituiscono implicitamente `None`.

## Parametri facoltativi

È possibile definire delle funzioni con dei parametri facoltativi, parametri cioè per cui viene specificato un argomento di default che viene utilizzato a meno che non specificato diversamente dal chiamante.

Facciamo un esempio

{% highlight php %}
<?php
function moltiplica($fattore1, $fattore2 = 2) {
    return $fattore1 * $fattore2;
}
{% endhighlight %}

Il parametro $fattore2 viene inizializzato a 2 se non specificato diversamente dal chiamante.

{% highlight php %}
<?php
$prodotto = moltiplica(3);
echo $prodotto; // stampa il valore 6
$prodotto = moltiplica(3, 3);
echo $prodotto; // stampa il valore 9
{% endhighlight %}

Notate che i parametri facoltativi vanno sempre specificati dopo aver specificato tutti i parametri ordinari, esattamente come in Python: dunque se si definisce una funzione come nell'esempio seguente l'interprete restituisce un messaggio di errore.

{% highlight php %}
<?php
function moltiplica($fattore1 = 2, $fattore2) {
    return $fattore1 * $fattore2;
}
{% endhighlight %}

## Ambito locale e ambito globale

Parametri e variabili definiti all'interno di una funzione hanno un ambito (o raggio d'azione, in inglese scope) locale alla funzione stessa. Le variabili assegnate all'esterno di tutte le funzioni hanno invece un ambito globale. Fin qui, esattamente come in Python.

**Qui però c'è la differenza più importante tra PHP e Python**: in Python, il codice dentro una funzione può *leggere* liberamente una variabile globale senza bisogno di dichiararla. In **PHP è l'esatto contrario**: per impostazione predefinita, il codice dentro una funzione **non vede affatto** le variabili globali, nemmeno in lettura. Per poterle usare bisogna dichiararle esplicitamente con la parola chiave **global**.

Pensate ad un ambito come ad una sorta di contenitore di variabili. Quando un ambito viene distrutto tutti i valori conservati nelle variabili di quell'ambito vengono dimenticati. Esiste un solo ambito globale e viene creato quando inizia il programma. Esistono tanti ambiti locali quante sono le funzioni che definiamo.

Valgono le seguenti proprietà:

- istruzioni nell'ambito globale non possono usare variabili che appartengono ad un ambito locale;
- istruzioni nell'ambito locale **non** possono accedere a variabili definite in ambito globale, a meno di dichiararle con `global`;
- istruzioni contenute all'interno di un ambito locale non possono accedere a variabili appartenenti ad un diverso ambito locale;
- è possibile utilizzare lo stesso nome per variabili diverse se si trovano in ambiti diversi.

## Le variabili locali non possono essere utilizzate nell'ambito globale

{% highlight php %}
<?php
function pollaio() {
    $uova = 32765;
}
pollaio();
echo $uova; // Warning: Undefined variable $uova
{% endhighlight %}

Come potete notare la variabile $uova appartiene all'ambito della funzione pollaio. La funzione viene invocata, ma una volta terminata la variabile locale $uova viene distrutta, non può dunque essere utilizzata dal flusso di programma principale. Fin qui il comportamento è identico a Python.

## Gli ambiti locali non possono usare variabili di altri ambiti locali

{% highlight php %}
<?php
function pollaio() {
    $uova = 32765;
    echo $uova; // corretto
}

function allevamento() {
    $pecore = 1234567;
    echo $pecore; // corretto
    echo $uova;   // Warning: Undefined variable $uova
}
{% endhighlight %}

Come potete vedere la funzione allevamento tenta di accedere alla variabile $uova che appartiene all'ambito della funzione pollaio. Questo non è legale, come in Python.

## Le variabili globali NON possono essere lette da un ambito locale (a differenza di Python!)

{% highlight php %}
<?php
$uova = 1234567;
function pollaio() {
    echo $uova; // Warning: Undefined variable $uova -- NON funziona!
}
pollaio();
{% endhighlight %}

In Python questo stesso codice funzionerebbe senza problemi, dato che le funzioni Python possono leggere le variabili globali senza dichiararle. In PHP, per poter leggere (o scrivere) la variabile globale $uova dentro la funzione, dobbiamo dichiararla esplicitamente con la parola chiave **global**:

{% highlight php %}
<?php
$uova = 1234567;
function pollaio() {
    global $uova;
    echo $uova; // corretto, ora funziona: stampa 1234567
}
pollaio();
{% endhighlight %}

La riga `global $uova;` dice all'interprete: "la variabile $uova che uso qui dentro non è una nuova variabile locale, è quella definita in ambito globale". In alternativa, si può accedere alle variabili globali tramite l'array superglobale `$GLOBALS`, senza bisogno della parola chiave `global`:

{% highlight php %}
<?php
$uova = 1234567;
function pollaio() {
    echo $GLOBALS['uova']; // corretto, forma alternativa
}
pollaio();
{% endhighlight %}

## Variabili locali e globali con lo stesso nome

{% highlight php %}
<?php
$uova = 7; // variabile globale
function pollaio() {
    $uova = 32765;
    echo $uova; // corretto, stampa 32765 (variabile locale, non collegata a quella globale!)
}
function allevamento() {
    $uova = 1234567;
    echo $uova; // corretto, stampa 1234567
}
pollaio();
allevamento();
echo $uova; // corretto, stampa 7 (invariata)
{% endhighlight %}

La variabile $uova viene definita sia nell'ambito globale, che nell'ambito della funzione pollaio che nell'ambito della funzione allevamento. Dato che nessuna delle due funzioni ha dichiarato `global $uova;`, ciascuna crea semplicemente una propria variabile locale con lo stesso nome, senza alcun legame con quella globale.

## Collaborare attraverso le funzioni

Quando un gruppo di sviluppatori lavora ad un software un aspetto molto delicato è quello della divisione dei compiti e del lavoro. Un approccio che spesso viene utilizzato è quello di strutturare il software identificandone le varie caratteristiche quindi organizzare queste ultime all'interno delle varie funzioni. Se per esempio la classe 3C volesse creare un software per la gestione della biblioteca della scuola, potrebbe dividere il lavoro in varie sezioni (interfaccia utente, salvataggio dati, regole per l'utilizzo del servizio, servizi di notifiche, servizi di calendario). A questo punto il gruppo stabilisce un linguaggio comune per la comunicazione esplicitando la firma di tutte le funzioni. Questo consiste nell'esplicitare il nome delle funzioni che andranno a costituire il software, i parametri che queste accettano e i valori che queste riportano al chiamante. Tutto ciò va fatto prima dell'implementazione delle funzioni stesse.

## Inizia scrivendo un test

Supponiamo di voler scrivere una funzione che deve fare la trasformazione di gradi Celsius in gradi Fahrenheit. Noi tutti sappiamo che: Tf = Tc * 9 / 5 + 32.

Ci serviamo dell'istruzione **assert**, che esiste anche in PHP con lo stesso scopo che ha in Python.

{% highlight php %}
<?php
function trasfCelsInFahr($temp) {
    return ($temp * 9 / 5) + 32;
}
assert(trasfCelsInFahr(0) == 32.0);
assert(trasfCelsInFahr(20) == 68.0);
assert(trasfCelsInFahr(30) == 86.0);
echo "Tutti i test sono passati!\n";
{% endhighlight %}

Quando vado a lanciare il programma questo manderà in esecuzione tutti i test e nel caso uno di questi non funzioni, cioè se la mia implementazione non è corretta, `assert()` genera un `AssertionError`. Questo espediente mi consente di correggere l'implementazione della funzione dato che è molto rapido eseguire i test per vedere se tutto funziona come atteso.

## Scrivere la documentazione per ciascuna funzione

È molto importante lasciare traccia su quanto viene calcolato da una funzione. In PHP la convenzione più diffusa per documentare una funzione si chiama **PHPDoc** e usa un commento speciale che inizia con `/**`:

{% highlight php %}
<?php
/**
 * Trasforma una temperatura da gradi Celsius a gradi Fahrenheit.
 *
 * @param float $temp temperatura in gradi Celsius
 * @return float temperatura in gradi Fahrenheit
 */
function trasfCelsInFahr($temp) {
    return ($temp * 9 / 5) + 32;
}
{% endhighlight %}

## Organizza il tuo codice in file diversi

Quando si scrive il codice è bene scriverlo in modo che questo sia facile da tenere sotto controllo e che sia riutilizzabile in una o più applicazioni. Per questo è importante organizzare il codice in più file. Immaginiamo di lavorare su di un progetto di fisica. Creiamo una cartella che conterrà tutti i file del nostro progetto, che chiameremo progettodifisica.

- progettodifisica/
- progettodifisica/principale.php
- progettodifisica/grandezze/conversioni.php

La cartella progettodifisica contiene un file principale.php il quale contiene la sezione principale del codice, quello cioè che mando in esecuzione quando ho necessità di utilizzare il software.

{% highlight php %}
<?php
// principale.php

// carica il file conversioni.php
require_once 'grandezze/conversioni.php';

function main() {
    echo "Temp Celsius: ";
    $temp = (float) trim(fgets(STDIN));
    $fahr = trasfCelsInFahr($temp);
    echo 'Temp. Fahrenheit = ' . $fahr;
}

main();
{% endhighlight %}

La funzione main contiene il flusso principale del programma.

L'istruzione **require_once** dà indicazioni all'interprete di caricare il file che abbiamo appena definito, esattamente una sola volta anche se viene richiesto più volte (è l'equivalente PHP dell'istruzione `from ... import` di Python). È buona norma metterla in alto, all'inizio del file. Esiste anche `include_once`, che a differenza di `require_once` non blocca il programma se il file non viene trovato.

{% highlight php %}
<?php
// grandezze/conversioni.php

function trasfCelsInFahr($temp) {
    return $temp * 9 / 5 + 32;
}

function trasfFahrInCels($temp) {
    // da implementare
}
{% endhighlight %}

In seguito troviamo un file di libreria in cui abbiamo precedentemente scritto l'implementazione delle funzioni che mi occorrono per fare conversioni tra grandezze. Questo file è contenuto nella cartella grandezze e si chiama conversioni.php. A differenza di Python, PHP non ha un blocco equivalente a `if __name__ == '__main__':` per distinguere l'esecuzione diretta di un file dal suo utilizzo come libreria: la convenzione è semplicemente quella di mettere in un file solo definizioni di funzioni, e in un altro file (come principale.php) il codice che le usa davvero.

## Esercizi

### Funzioni e numeri

#### Esercizio n1:

Scrivi una funzione saluta che prenda la stringa $nome come parametro e restituisca al chiamante la stringa composta da 'Ciao ' . $nome.

#### Esercizio n2:

Scrivi una funzione calcolamaggiore che prenda due numeri come parametro ($num1 e $num2) e restituisca al chiamante il più grande tra i due.

#### Esercizio n3:

Scrivi una funzione calcolamaggiore che prenda tre numeri come parametro ($num1, $num2 e $num3) e restituisca al chiamante il più grande tra i tre.

#### Esercizio n4:
Scrivi una funzione che accetti un parametro di tipo numerico e calcoli il fattoriale del numero ricevuto e lo restituisca al chiamante.

#### Esercizio n5:

Implementa una funzione che preso come parametro un numero intero restituisca al chiamante il corrispondente numero di Fibonacci.

#### Esercizio n6:

Implementa una funzione che preso come parametro un numero intero restituisca al chiamante true se il numero è primo e false altrimenti.

#### Esercizio n7:

Scrivi con approccio Test Driven Development una funzione che calcoli il massimo comune divisore tra due numeri.

#### Esercizio n8:

Scrivi con approccio Test Driven Development una funzione che calcoli il minimo comune multiplo tra due numeri.

#### Esercizio n9:

Scrivere una funzione con approccio TDD che calcoli la distanza tra due punti sul piano cartesiano.
firma: function distanza($ax, $ay, $bx, $by)

#### Esercizio n10:

Cosa fa il seguente script?

{% highlight php %}
<?php
$biciclette = 123;
function scrivi_biciclette() {
    echo $biciclette;
}
scrivi_biciclette();
{% endhighlight %}

#### Esercizio n11:

Cosa fa il seguente script?

{% highlight php %}
<?php
$biciclette = 123;
function scrivi_biciclette() {
    $biciclette = 321;
    echo $biciclette;
}
scrivi_biciclette();
{% endhighlight %}

#### Esercizio n12:

Crea una funzione **divisibile_per** che prenda come parametro due numeri e che restituisca true se il primo numero è divisibile per il secondo e false in caso contrario

#### Esercizio n13:

Scrivi una funzione PHP somma_numeri che prenda come parametri due numeri interi $a e $b e calcoli la somma di tutti i numeri compresi tra $a e $b con $a e $b compresi
Esempio:
somma_numeri(4, 6) restituisce 15
somma_numeri(1, 4) restituisce 10

#### Esercizio n14:

Scrivi una funzione PHP somma_pari che prenda come parametri due numeri interi $a e $b e calcoli la somma di tutti i numeri pari compresi tra $a e $b con $a e $b compresi
Esempio:
somma_pari(4, 6) restituisce 10
somma_pari(1, 5) restituisce 6

#### Esercizio n15:

Scrivi una funzione PHP is_prime che prenda come parametro un numero intero $a e restituisca true se $a è primo e false in caso contrario
Esempio:
is_prime(11) restituisce true
is_prime(4) restituisce false

#### Esercizio n16:

Scrivi una funzione celsiusToFahrenheit che accetti come parametro una temperatura in gradi Celsius e restituisca la corrispondente temperatura in gradi Fahrenheit. Scrivi poi una funzione fahrenheitToCelsius che faccia l'operazione opposta. Scrivi infine il main che facendo uso delle funzioni scriva le scale di conversione di temperatura per i gradi Celsius che vanno da -20°C a 100°C a passo 5 e per i gradi Fahrenheit che vanno da -5 a 205 a passo 10.

#### Esercizio n17:
Scrivi una funzione PHP che ricevuto un numero come parametro restituisca true se questo è perfetto.
Nella teoria dei numeri un numero perfetto è un numero positivo che è uguale alla somma dei propri divisori positivi escluso il numero stesso.
Esempi : Il primo numero perfetto è 6, perché 1, 2, e 3 sono i suoi divisori positivi e 1 + 2 + 3 = 6.
Il numero perfetto successivo è 28 = 1 + 2 + 4 + 7 + 14.
Questi è seguito dai numeri perfetti 496 e 8128.

### Funzioni e stringhe

#### Esercizio s1:

Scrivi una funzione a cui viene passato un carattere come parametro, e che restituisca al chiamante la stringa 'vocale' se il carattere è una vocale o la stringa 'consonante' in caso contrario.

#### Esercizio s2:

Crea una funzione **tipo_stringa** che prenda come parametro una stringa di testo e restituisca:
* la stringa "solo lettere" se il parametro è costituito completamente da lettere
* la stringa "solo numeri" se il parametro è costituito completamente da numeri
* la stringa "mista" se il parametro è costituito sia da lettere sia da numeri

Esempi:
tipo_stringa("1322132") => "solo numeri"
tipo_stringa("acbac") => "solo lettere"
tipo_stringa("132acbac12") => "mista"

#### Esercizio s3:

Scrivi una funzione che accetti una stringa di testo come parametro e la restituisca invertita al chiamante:
Esempio
Parametro : "1234abcd"
Valore restituito : "dcba4321"

#### Esercizio s4:

Scrivi una funzione PHP che accetti un parametro di tipo stringa e restituisca il numero di vocali contenute in questa.
Esempio:
conta_vocali("ciao") restituisce 3

#### Esercizio s5:

Scrivi una funzione PHP che accetti un parametro di tipo stringa e restituisca il numero di consonanti contenute in questa.
Esempio:
conta_consonanti("ciao") restituisce 1

#### Esercizio s6:

Scrivi una funzione **esprimi_giudizio($voto)** che preso come parametro il voto di uno studente restituisca una stringa di testo con giudizio secondo la seguente tabella:

| 1, 2, 3, 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|
| Gravemente insufficiente | Insufficiente | Sufficiente | Discreto | Buono | Distinto | Ottimo |

### Funzioni e array

#### Esercizio l1:
Scrivi una funzione PHP che accetti un array come parametro, calcoli la somma dei numeri contenuti nell'array e restituisca il risultato
Esempio:
parametro: [8, 2, 3, 0, 7]
Output : 20

#### Esercizio l2:
Scrivi una funzione PHP che accetti un array come parametro, calcoli il prodotto dei numeri contenuti nell'array e restituisca il risultato
Esempio:
parametro: [8, 2, 3, -1, 7]
Output : -336

#### Esercizio l3:
Scrivi una funzione PHP che accetti un array come parametro e restituisca un array contenente i soli numeri pari dell'array ricevuto.
Esempio:
parametro: [1, 2, 3, 4, 5, 6, 7, 8, 9]
Output : [2, 4, 6, 8]

### Esercizio l4:
Scrivi una funzione PHP che accetti un array come parametro e restituisca un array contenente gli elementi dell'array ricevuto non ripetuti (funzione `array_unique()`).
Esempio:
parametro: [1,2,3,3,3,3,4,5]
Output : [1, 2, 3, 4, 5]

### Top Down

#### Esercizio td1:

Scrivi una funzione in PHP chiamata calcolastipendio che prende come parametro il numero di ore lavorate in una settimana e la paga oraria.

function calcolastipendio($ore, $paga)

La funzione se il numero di ore è minore o uguale a 40 "ritorna" il prodotto tra il primo numero e il secondo. Se è maggiore: per le prime 40 ore ritorna il primo numero per il secondo, per le ore di straordinario ritorna il primo numero per il secondo maggiorato del 50%.

Esempi:
calcolastipendio(20, 20) => 400
calcolastipendio(40, 20) => 800
calcolastipendio(50, 20) => 1100

#### Esercizio td2:

Utilizziamo le funzioni per calcolare delle spese di viaggio: Definisci una funzione chiamata **costo_hotel** che prende come parametro il numero delle notti. L'hotel costa 140 per notte. La funzione deve calcolare il costo totale dell'hotel. Definisci una funzione chiamata **costo_aereo** questa prende come parametro il nome della città in cui si vola e ritorna a seconda della destinazione: "Charlotte": 183 "Tampa": 220 "Pittsburgh": 222 "Los Angeles": 475. Definisci una funzione **noleggio_macchina** che prenda come parametro il numero di giorni. Ogni giorno di noleggio macchina costa 40. Se si noleggia la macchina per più di 7 giorni si ottiene uno sconto di 50. Se si noleggia la macchina per più di 10 giorni si ottiene uno sconto di 10 al giorno. Infine crea una funzione **costo_viaggio** che prenda come parametro una stringa rappresentante la destinazione e il numero di giorni di viaggio e chiamando le funzioni prima definite calcoli il costo totale del viaggio.

### Refactoring

#### Esercizio r1:

Riorganizza il seguente codice PHP in funzioni. Bisogna suddividere l'algoritmo in algoritmi più piccoli e semplici da capire in modo da rendere il codice più leggibile.

{% highlight php %}
<?php
echo "La mia calcolatrice\n";
$finito = false;
while (!$finito) {
    echo "Operatore (+ - * /): ";
    $op = trim(fgets(STDIN));
    echo "Primo numero: ";
    $num1 = trim(fgets(STDIN));
    echo "Secondo numero: ";
    $num2 = trim(fgets(STDIN));
    if ($op == '+') {
        $risultato = $num1 + $num2;
    } elseif ($op == '-') {
        $risultato = $num1 - $num2;
    } elseif ($op == '*') {
        $risultato = $num1 * $num2;
    } elseif ($op == '/') {
        $risultato = $num1 / $num2;
    } else {
        $risultato = 'Operatore sconosciuto';
    }
    echo "Finito? (S, N): ";
    $risp = trim(fgets(STDIN));
    if ($risp == 'S') {
        $finito = true;
    }
}
echo "Grazie! Alla prossima volta";
{% endhighlight %}

### Esercizi di tracciamento

Per i seguenti esercizi non dovete scrivere codice: dovete costruire la **tabella di tracciamento** del programma, indicando per ciascuna riga eseguita l'ambito (globale o il nome della funzione) in cui ci si trova e il valore di ogni variabile.

#### Esercizio t1:

Costruite la tabella di tracciamento del seguente programma, indicando ad ogni istruzione l'ambito in cui viene eseguita e il valore delle variabili coinvolte:

{% highlight php %}
<?php
function raddoppia($numero) {
    $numero = $numero * 2;
    return $numero;
}

$x = 5;
$y = raddoppia($x);
$x = raddoppia($y);
{% endhighlight %}

Che valore hanno `$x` e `$y` alla fine dell'esecuzione?

#### Esercizio t2:

Costruite la tabella di tracciamento del seguente programma, facendo attenzione alla differenza tra la variabile globale `$contatore` e la variabile locale (parametro) definita nella funzione:

{% highlight php %}
<?php
$contatore = 0;

function incrementa($contatore) {
    $contatore = $contatore + 1;
    return $contatore;
}

$contatore = incrementa($contatore);
$contatore = incrementa($contatore);
echo $contatore;
{% endhighlight %}

#### Esercizio t3:

Costruite la tabella di tracciamento del seguente programma ricordando che ogni chiamata a funzione crea un nuovo ambito locale (mostrate i valori di `$n`, `$acc` e `$risultato` ad ogni chiamata):

{% highlight php %}
<?php
function somma_fino_a($n) {
    $acc = 0;
    $x = 1;
    while ($x <= $n) {
        $acc = $acc + $x;
        $x = $x + 1;
    }
    return $acc;
}

$risultato = somma_fino_a(4);
echo $risultato;
{% endhighlight %}

### Esercizi di ricerca degli errori (debugging)

Nei seguenti programmi è stato inserito un errore. Provate prima a individuarlo leggendo il codice (senza eseguirlo), poi verificate eseguendolo in PHP ed infine correggetelo.

#### Esercizio e1:

Questo programma genera un errore di sintassi.

{% highlight php %}
<?php
function saluta($nome) {
    echo "Ciao " . $nome;

saluta("Marco");
{% endhighlight %}

#### Esercizio e2:

Questo programma genera un errore di sintassi.

{% highlight php %}
<?php
function raddoppia($numero)
    return $numero * 2;

echo raddoppia(5);
{% endhighlight %}

#### Esercizio e3:

Questo programma genera solo un **warning** (in Python genererebbe un `NameError` bloccante): individuate quale istruzione lo causa e spiegate perché, ricordando le regole sull'ambito locale e globale.

{% highlight php %}
<?php
function pollaio() {
    $uova = 32765;
}

pollaio();
echo $uova;
{% endhighlight %}

#### Esercizio e4:

Questo programma non genera errori ma contiene un **errore logico**: la funzione dovrebbe restituire il doppio del numero, ma chi la chiama riceve sempre `null`. Individuate l'errore.

{% highlight php %}
<?php
function raddoppia($numero) {
    $numero * 2;
}

$risultato = raddoppia(5);
echo $risultato;
{% endhighlight %}

#### Esercizio e5:

Questo programma non genera errori ma contiene un **errore logico**: la funzione dovrebbe restituire il maggiore tra i due numeri passati, ma per alcuni valori restituisce il risultato sbagliato. Individuate l'errore.

{% highlight php %}
<?php
function calcolamaggiore($num1, $num2) {
    if ($num1 > $num2) {
        return $num1;
    } else {
        return $num1;
    }
}

echo calcolamaggiore(3, 8);
{% endhighlight %}

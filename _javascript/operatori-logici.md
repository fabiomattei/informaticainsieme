---
title: 'Operatori logici'
date: '2026-08-18T09:45:00+01:00'
author: Fabio Mattei
layout: page
---

È possibile combinare tra loro le espressioni logiche attraverso gli operatori logici. In JavaScript gli operatori logici principali sono `&&` (and), `||` (or) e `!` (not) — esattamente gli stessi simboli usati in PHP.

| A | B | A && B |
|---|---|---|
| true | true | true |
| true | false | false |
| false | true | false |
| false | false | false |

| A | B | A \|\| B |
|---|---|---|
| true | true | true |
| true | false | true |
| false | true | true |
| false | false | false |

| A | !A |
|---|---|
| true | false |
| false | true |

#### Esempio:
Scrivere un programma JavaScript che, data la temperatura dell'acqua, scriva lo stato in cui questa si trova: solida, liquida, gassosa

{% highlight javascript %}
let temperatura = -5;
if (temperatura <= 0) {
    console.log("Stato solido");
}
if (temperatura > 0 && temperatura <= 100) {
    console.log("Stato liquido");
}
if (temperatura > 100) {
    console.log("Stato Gassoso");
}
{% endhighlight %}

Notiamo che la stringa di testo "Stato liquido" viene scritta se, e soltanto se, la temperatura è sia maggiore di 0 sia minore o uguale a 100. Devono essere verificate entrambe le espressioni affinché l'espressione totale sia verificata.

## Valutazione "corto circuito" e valori truthy/falsy

C'è una particolarità di JavaScript che non esiste in PHP: `&&` e `||` non restituiscono sempre `true` o `false`, ma **il valore di uno dei due operandi**. Questo perché JavaScript valuta le espressioni in "corto circuito": si ferma non appena il risultato è determinato.

{% highlight javascript %}
console.log(0 && "ciao");        // 0 (falsy: si ferma subito, non valuta il secondo)
console.log("ciao" && "mondo");  // "mondo" (il primo è truthy, quindi valuta e restituisce il secondo)
console.log(0 || "predefinito"); // "predefinito" (il primo è falsy, restituisce il secondo)
console.log("valore" || "predefinito"); // "valore" (il primo è truthy, si ferma subito)
{% endhighlight %}

Questo comportamento viene spesso usato per fornire un **valore di default** in modo compatto:

{% highlight javascript %}
let nomeUtente = "";
let nomeVisualizzato = nomeUtente || "Ospite";
console.log(nomeVisualizzato);   // "Ospite", perché nomeUtente è una stringa vuota (falsy)
{% endhighlight %}

Nelle condizioni degli `if`, però, il comportamento pratico è identico a PHP: il risultato viene comunque interpretato come `true` o `false` secondo le regole di truthiness viste nella pagina sulle [conversioni di tipo]({{ site.baseurl }}{% link _javascript/conversioni-di-tipo.md %}.html).

#### Esercizio 1:
Al fine di calcolare le imposte da versare al fisco lo stato italiano predispone 5 scaglioni. La tassazione viene calcolata limitatamente alla porzione di reddito che ricade in ciascuno scaglione IRPEF.

| Scaglioni reddito Irpef | Aliquota |
|---|---|
| da 0 a **15.000** euro | 23% |
| da **15.000,01** a **28.000** euro | 27% |
| Da **28.000,01** a **55.000** euro | 38% |
| da **55.000,01** a **75.000** euro | 41% |
| oltre **75.000** euro | 43% |

#### Esercizio 2:
scrivi un programma che, dato un numero, scriva se questo è positivo, negativo o nullo.

#### Esercizio 3:
scrivi un programma che, dato un numero a virgola mobile, scriva se questo è positivo, negativo o nullo e aggiunga il messaggio small se il valore assoluto del numero è minore di uno oppure large se è superiore ad un milione.

#### Esercizio 4:
Scrivere un programma che, dato un numero intero, visualizzi il numero delle sue cifre verificando prima se il numero è >= a 10, poi se il numero è >= 100 e così via fino a 10 miliardi. Se il numero è negativo moltiplicarlo prima per -1 (`Math.abs()`).

#### Esercizio 5:
Scrivere un programma che, dati 3 numeri interi, visualizzi "tutti uguali" se questi sono tutti uguali e "tutti differenti" se sono tutti differenti. Scriva "non uguali non differenti" in caso contrario.

#### Esercizio 6:
Scrivere un programma che, dati 3 numeri interi, scriva "in ordine" se sono ordinati in senso crescente o decrescente e "non in ordine" in caso contrario.
Es: 1, 2, 5 → in ordine; 6, 4, 2 → in ordine; 7, 2, 9 → non in ordine

#### Esercizio 7:
Scrivere un programma che, data una temperatura (intera) e l'unità di misura (Celsius o Fahrenheit), indichi se l'acqua a quella temperatura si trovi allo stato solido, liquido o gassoso.

#### Esercizio 8:
Scrivere un programma che, data la descrizione di una carta da gioco usando la notazione abbreviata:
A: Asso
1..10: Valore della carta
J: fante (Jack)
Q: regina (Queen)
K: re (King)
D: Quadri (diamonds)
H: cuori (hearts)
S: picche (Spades)
C: Clubs (fiori)
visualizzi la descrizione completa della carta.
Es: QS: Queen of spades

#### Esercizio 9:
Scrivere un programma che, dati due istanti di tempo espressi in ora e minuti, determini quale istante preceda l'altro e li scriva in ordine.
Es: Tempo1: 12:30 Tempo2: 07:45 → Output 07:45 → 12:30

#### Esercizio 10:
Scrivere un programma JavaScript che, data una data di nascita, visualizzi il segno zodiacale corrispondente.

#### Esercizio 11:
Scrivere un programma JavaScript che, data una data, visualizzi la stagione cui la data appartiene.

#### Esercizio 12:
Un anno viene detto bisestile se divisibile per 4. Gli anni come 1996 sono bisestili. Ma gli anni divisibili per 100 non lo sono, ad esempio il 1900 non lo è. Eccezione dell'eccezione gli anni divisibili per 400 lo sono, ad esempio l'anno 2000.
Scrivere un programma che, dato un anno, scriva se questo è bisestile oppure no.

### Esercizi di tracciamento

Per i seguenti esercizi non dovete eseguire il codice: dovete calcolare a mano il valore (true o false) di ciascuna sotto-espressione e il risultato finale dell'espressione logica, oppure costruire la tabella di tracciamento richiesta.

#### Esercizio 13:

Calcolate il valore (true o false) delle seguenti espressioni, sapendo che `a = 5`, `b = 10` e `c = 5`:

- `a === c`
- `a < b && b < c`
- `a < b || b < c`
- `!(a === c)`
- `a < b && !(a === c)`

#### Esercizio 14:

Costruite la tabella di tracciamento del seguente programma per `temperatura = 50` (mostrate il valore di ciascuna condizione valutata e quale ramo viene eseguito):

{% highlight javascript %}
let temperatura = 50;
if (temperatura <= 0) {
    console.log("Stato solido");
}
if (temperatura > 0 && temperatura <= 100) {
    console.log("Stato liquido");
}
if (temperatura > 100) {
    console.log("Stato Gassoso");
}
{% endhighlight %}

#### Esercizio 15:

Costruite la tabella di tracciamento del seguente programma (mostrate i valori di `x`, `y` ed il valore booleano calcolato ad ogni riga), sapendo che `x = 7` e `y = -3`:

{% highlight javascript %}
let x = 7;
let y = -3;
let positivi = x > 0 && y > 0;
let almenoUnoPositivo = x > 0 || y > 0;
let opposti = (x > 0 && y < 0) || (x < 0 && y > 0);
{% endhighlight %}

### Esercizi di ricerca degli errori (debugging)

Nei seguenti programmi è stato inserito un errore. Provate prima a individuarlo leggendo il codice (senza eseguirlo), poi verificate eseguendolo in Node.js ed infine correggetelo.

#### Esercizio 16:

Questo programma genera un errore di sintassi.

{% highlight javascript %}
let temperatura = 50;
if (temperatura <= 0) {
    console.log("Stato solido");
}
if (temperatura > 0 && temperatura <= 100)
    console.log("Stato liquido");
}
{% endhighlight %}

#### Esercizio 17:

Questo programma non contiene errori di sintassi, ma contiene un **errore logico**: un numero come 0 (nullo) viene sempre classificato come "positivo". Individuate l'errore.

{% highlight javascript %}
let numero = 0;
if (numero >= 0) {
    console.log("positivo");
} else {
    console.log("negativo");
}
{% endhighlight %}

#### Esercizio 18:

Questo programma dovrebbe stampare "in intervallo" solo se il numero è compreso tra 10 e 20 (estremi inclusi), ma contiene un **errore logico** che fa sì che stampi sempre "in intervallo". Individuate l'errore e correggetelo.

{% highlight javascript %}
let numero = 50;
if (numero >= 10 || numero <= 20) {
    console.log("in intervallo");
} else {
    console.log("fuori intervallo");
}
{% endhighlight %}

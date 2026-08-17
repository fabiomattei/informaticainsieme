---
title: 'Operatori logici'
date: '2026-08-17T09:45:00+01:00'
author: Fabio Mattei
layout: page
---

È possibile combinare tra loro le espressioni logiche attraverso gli operatori logici. In PHP gli operatori logici principali sono `&&` (and), `||` (or) e `!` (not). Esistono anche le versioni testuali `and`, `or` e `xor`, ma hanno una **precedenza diversa** rispetto a `&&` e `||`: per evitare sorprese è consigliato usare sempre `&&` e `||`.

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
Scrivere un programma PHP che letta la temperatura dell'acqua scriva lo stato in cui questa si trova: solida, liquida, gassosa

{% highlight php %}
<?php
echo "Scrivi la temperatura dell'acqua: ";
$temperatura = (int) trim(fgets(STDIN));
if ($temperatura <= 0) {
    echo "Stato solido";
}
if ($temperatura > 0 && $temperatura <= 100) {
    echo "Stato liquido";
}
if ($temperatura > 100) {
    echo "Stato Gassoso";
}
{% endhighlight %}

Notiamo che la stringa di testo "Stato liquido" viene scritta se, e soltanto se, la temperatura è sia maggiore di 0 sia minore o uguale a 100. Devono essere verificate entrambe le espressioni affinché l'espressione totale sia verificata.

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
scrivi un programma che letto un numero scriva se questo è positivo, negativo o nullo.

#### Esercizio 3:
scrivere un programma che letto un numero a virgola mobile scriva se questo è positivo, negativo o nullo e aggiunga il messaggio small se il valore assoluto del numero è minore di uno oppure large se è superiore ad un milione.

#### Esercizio 4:
Scrivere un programma che letto un numero intero e visualizzi il numero delle sue cifre verificando prima se il numero è >= a 10, poi se il numero è >= 100 e così via fino a 10 miliardi. Se il numero è negativo moltiplicarlo prima per -1.

#### Esercizio 5:
Scrivere un programma che letti 3 numeri interi visualizzi "tutti uguali" se questi sono tutti uguali e "tutti differenti" se sono tutti differenti. Scriva "non uguali non differenti" in caso contrario.

#### Esercizio 6:
Scrivere un programma che legga 3 numeri interi e scriva "in ordine" se sono ordinati in senso crescente o decrescente e "non in ordine" in caso contrario
Es: 1, 2, 5 → in ordine; 6, 4, 2 → in ordine; 7, 2, 9 → non in ordine

#### Esercizio 7:
Scrivere un programma che letta una temperatura (intera) e l'unità di misura (Celsius o Fahrenheit) indichi se l'acqua a quella temperatura si trovi allo stato solido, liquido o gassoso.

#### Esercizio 8:
Scrivere un programma che acquisisca dall'utente la descrizione di una carta da gioco usando la notazione abbreviata:
A: Asso
1..10: Valore della carta
J: fante (Jack)
Q: regina (Queen)
K: re (King)
D: Quadri (diamonds)
H: cuori (hearts)
S: picche (Spades)
C: Clubs (fiori)
Il programma deve poi visualizzare la descrizione completa della carta
Es: QS: Queen of spades

#### Esercizio 9:
Scrivere un programma che letti due istanti di tempo espressi in ora e minuti determini quale istante preceda l'altro e li scriva in ordine.
Es: Input Tempo1: HH:12 MM:30 Tempo 2: HH:07 MM:45 > Output 07:45 → 12:30

#### Esercizio 10:
Scrivere un programma PHP che chieda all'utente la sua data di nascita e visualizzi il suo segno zodiacale

#### Esercizio 11:
Scrivere un programma PHP che chieda all'utente una data e visualizzi la stagione cui la data appartiene

#### Esercizio 12:
Un anno viene detto bisestile se divisibile per 4. Gli anni come 1996 sono bisestili. Ma gli anni divisibili per 100 non lo sono, ad esempio il 1900 non lo è. Eccezione dell'eccezione gli anni divisibili per 400 lo sono, ad esempio l'anno 2000.
Scrivere un programma che legga un anno e scriva se questo è bisestile oppure no.

### Esercizi di tracciamento

Per i seguenti esercizi non dovete eseguire il codice: dovete calcolare a mano il valore (true o false) di ciascuna sotto-espressione e il risultato finale dell'espressione logica, oppure costruire la tabella di tracciamento richiesta.

#### Esercizio 13:

Calcolate il valore (true o false) delle seguenti espressioni, sapendo che `$a = 5`, `$b = 10` e `$c = 5`:

- `$a == $c`
- `$a < $b && $b < $c`
- `$a < $b || $b < $c`
- `!($a == $c)`
- `$a < $b && !($a == $c)`

#### Esercizio 14:

Costruite la tabella di tracciamento del seguente programma per `$temperatura = 50` (mostrate il valore di ciascuna condizione valutata e quale ramo viene eseguito):

{% highlight php %}
<?php
$temperatura = 50;
if ($temperatura <= 0) {
    echo "Stato solido";
}
if ($temperatura > 0 && $temperatura <= 100) {
    echo "Stato liquido";
}
if ($temperatura > 100) {
    echo "Stato Gassoso";
}
{% endhighlight %}

#### Esercizio 15:

Costruite la tabella di tracciamento del seguente programma (mostrate i valori di `$x`, `$y` ed il valore booleano calcolato ad ogni riga), sapendo che `$x = 7` e `$y = -3`:

{% highlight php %}
<?php
$x = 7;
$y = -3;
$positivi = $x > 0 && $y > 0;
$almeno_uno_positivo = $x > 0 || $y > 0;
$opposti = ($x > 0 && $y < 0) || ($x < 0 && $y > 0);
{% endhighlight %}

### Esercizi di ricerca degli errori (debugging)

Nei seguenti programmi è stato inserito un errore. Provate prima a individuarlo leggendo il codice (senza eseguirlo), poi verificate eseguendolo in PHP ed infine correggetelo.

#### Esercizio 16:

Questo programma genera un errore di sintassi.

{% highlight php %}
<?php
echo "Scrivi la temperatura dell'acqua: ";
$temperatura = (int) trim(fgets(STDIN));
if ($temperatura <= 0) {
    echo "Stato solido";
}
if ($temperatura > 0 && $temperatura <= 100)
    echo "Stato liquido";
}
{% endhighlight %}

#### Esercizio 17:

Questo programma non contiene errori di sintassi, ma contiene un **errore logico**: un numero come 0 (nullo) viene sempre classificato come "positivo". Individuate l'errore.

{% highlight php %}
<?php
$numero = 0;
if ($numero >= 0) {
    echo "positivo";
} else {
    echo "negativo";
}
{% endhighlight %}

#### Esercizio 18:

Questo programma dovrebbe stampare "in intervallo" solo se il numero è compreso tra 10 e 20 (estremi inclusi), ma contiene un **errore logico** che fa sì che stampi sempre "in intervallo". Individuate l'errore e correggetelo.

{% highlight php %}
<?php
$numero = 50;
if ($numero >= 10 || $numero <= 20) {
    echo "in intervallo";
} else {
    echo "fuori intervallo";
}
{% endhighlight %}

---
title: 'Il cifrario di Vigenère'
date: '2026-08-18'
author: Fabio Mattei
layout: page
---

Il [cifrario di Cesare]({{ site.baseurl }}{% link _problemsolving/cifrario-cesare.md %}.html) è un cifrario **monoalfabetico**: ogni lettera del testo in chiaro viene sempre sostituita nello stesso modo, per tutto il messaggio. Questo lo rende relativamente facile da decifrare, anche senza conoscere la chiave.

Il **cifrario di Vigenère** è invece un cifrario **polialfabetico**: lettere uguali nel testo in chiaro possono essere crittate in modo **diverso**, a seconda della posizione in cui si trovano.

## Come funziona

Si sceglie una parola, detta **chiave**. Per crittare il messaggio:

1. si prende la prima lettera del messaggio e la prima lettera della chiave;
2. si sommano le loro posizioni nell'alfabeto (con A=0, B=1, ..., Z=25), e si prende il resto della divisione per 26 (in modo da restare all'interno dell'alfabeto);
3. il numero ottenuto è la posizione della lettera crittata;
4. si procede allo stesso modo per le lettere successive del messaggio, usando via via le lettere successive della chiave;
5. poiché in generale la chiave è più corta del messaggio, la si ripete ciclicamente fino a coprire tutto il messaggio.

In sintesi, se **P** è la posizione della lettera del messaggio (Plaintext) e **K** è la posizione della lettera della chiave (Key):

**posizione della lettera crittata = (P + K) mod 26**

Per **decrittare**, si fa l'operazione inversa:

**posizione della lettera in chiaro = (C - K) mod 26**

dove **C** è la posizione della lettera crittata.

## Esempio 1: una sola parola

Crittare la parola **CIAO** usando come chiave la parola **SOLE**.

#### Soluzione

| testo in chiaro | C | I | A | O |
|---|---|---|---|---|
| chiave | S | O | L | E |
| testo crittato | **U** | **W** | **L** | **S** |

Risultato: **UWLS**

#### Commenti alla soluzione

Convertendo le lettere in posizioni (A=0, ..., Z=25):

* C(2) + S(18) = 20 → **U**
* I(8) + O(14) = 22 → **W**
* A(0) + L(11) = 11 → **L**
* O(14) + E(4) = 18 → **S**

Per verificare la decrittazione, si sottraggono le posizioni della chiave da quelle del testo crittato: U(20)-S(18)=2→C, W(22)-O(14)=8→I, L(11)-L(11)=0→A, S(18)-E(4)=14→O, ritrovando "CIAO".

## Esempio 2: una frase

Crittare la frase **BUON NATALE** usando come chiave la parola **SOLE**.

#### Soluzione

Per prima cosa si eliminano gli spazi tra le parole, ottenendo la lista di lettere **BUONNATALE** (10 lettere). Si ripete poi la chiave SOLE fino a coprire tutte le lettere:

| testo in chiaro | B | U | O | N | N | A | T | A | L | E |
|---|---|---|---|---|---|---|---|---|---|---|
| chiave | S | O | L | E | S | O | L | E | S | O |
| testo crittato | T | I | Z | R | F | O | E | E | D | S |

Risultato: **TIZRFOEEDS**

## Il cifrario di Vigenère in Python

{% highlight python %}
alfabeto = 'abcdefghijklmnopqrstuvwxyz'

def critta_vigenere(testo, chiave):
    risultato = ''
    j = 0                                     # indice per scorrere la chiave
    for carattere in testo:
        if carattere in alfabeto:
            p = alfabeto.find(carattere)
            k = alfabeto.find(chiave[j % len(chiave)])
            risultato = risultato + alfabeto[(p + k) % 26]
            j = j + 1                         # avanza nella chiave solo per le lettere
        else:
            risultato = risultato + carattere
    return risultato

print(critta_vigenere('ciao', 'sole'))   # stampa: uwls
{% endhighlight %}

Per decrittare basta cambiare il segno della somma:

{% highlight python %}
def decritta_vigenere(testo, chiave):
    risultato = ''
    j = 0
    for carattere in testo:
        if carattere in alfabeto:
            c = alfabeto.find(carattere)
            k = alfabeto.find(chiave[j % len(chiave)])
            risultato = risultato + alfabeto[(c - k) % 26]
            j = j + 1
        else:
            risultato = risultato + carattere
    return risultato

print(decritta_vigenere('uwls', 'sole'))   # stampa: ciao
{% endhighlight %}

## Esercizio proposto

Usando come chiave la parola **MARE**:

1. crittare la parola **FIUME**;
2. decrittare la parola crittata **XUEE**, sapendo che è stata ottenuta con la stessa chiave.

## Sfide dalle gare OPS

I problemi seguenti sono tratti (e adattati) dalle gare a squadre delle Olimpiadi di Problem Solving (OPS) 2026, categoria Secondaria di secondo grado. Sono puzzle "a più livelli": combinano il cifrario di Cesare, la sostituzione monoalfabetica generica e il cifrario di Vigenère nello stesso problema, spesso usando la chiave decifrata da un messaggio per decifrarne un altro. Prova a risolverli da solo prima di aprire la soluzione.

### Gara 1

Franca ha ricevuto due messaggi cifrati, uno dal suo amico Giuseppe:

`JMBJSDG MV HMIRDI YVIEWKA`

e uno dalla sua amica Linda:

`JWB VBM GMB KBC`

Franca sa che Giuseppe ha cifrato il messaggio con un algoritmo a sostituzione polialfabetica e tavola di Vigenère. La chiave usata da Giuseppe è l'**ultima parola** della frase contenuta nel messaggio cifrato inviato da Linda. Franca sa inoltre che Linda cifra i suoi messaggi con un algoritmo a sostituzione monoalfabetica, usando sempre come chiave:

| A | B | C | D | E | F | G | H | I | J | K | L | M | N | O | P | Q | R | S | T | U | V | W | X | Y | Z |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Z | H | S | J | B | P | F | E | C | R | X | D | A | Y | Q | V | T | M | K | G | W | N | O | L | U | I |

Qual è il messaggio decifrato inviato a Franca da Giuseppe?

<details markdown="1">
<summary>Soluzione</summary>

**RITROVO IN PIAZZA GRAMSCI**

Usando la chiave monoalfabetica data, il messaggio di Linda `JWB VBM GMB KBC` si decritta in **DUE PER TRE SEI**: l'ultima parola è quindi **SEI**, che diventa la chiave di Vigenère per il messaggio di Giuseppe. Decifrando `JMBJSDG MV HMIRDI YVIEWKA` con la tavola di Vigenère e chiave SEI si ottiene **RITROVO IN PIAZZA GRAMSCI**.

</details>

### Gara 3

Giulia ha ricevuto tre messaggi cifrati: uno da Francesco, uno da Carlo e uno da Marta.

Quello di Francesco è: `ZC EZSBLNS HDIID`

Quello di Marta è: `ZODX HQL GHIFA HNEEIDHHA`

Giulia sa che il messaggio di Francesco contiene un **indovinello**, cifrato con un algoritmo a sostituzione monoalfabetica, la cui **risposta** sarà la chiave da usare per decifrare (con Vigenère) il messaggio di Marta. La chiave monoalfabetica per il messaggio di Francesco è la seguente:

| A | B | C | D | E | F | G | H | I | J | K | L | M | N | O | P | Q | R | S | T | U | V | W | X | Y | Z |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| S | A | J | R | L | M | K | G | Z | X | U | C | Y | B | D | E | F | H | I | N | O | P | Q | T | V | W |

*(questa chiave si ottiene da alcuni frammenti nei messaggi di Carlo, che qui non riportiamo: usa direttamente la tavola già pronta qui sopra)*

Una volta risolto l'indovinello, il messaggio di Marta ti chiederà un'altra informazione: aiutati con questa tabella di poeti italiani e correnti letterarie:

| Poeta | Corrente / epoca |
|---|---|
| Dante Alighieri | Medioevo / Dolce Stil Novo |
| Francesco Petrarca | Umanesimo |
| Giovanni Boccaccio | Umanesimo |
| Ugo Foscolo | Neoclassicismo / Preromanticismo |
| Giacomo Leopardi | Romanticismo |
| Giovanni Pascoli | Decadentismo |
| Gabriele D'Annunzio | Decadentismo |
| Giuseppe Ungaretti | Ermetismo |
| Eugenio Montale | Ermetismo |
| Salvatore Quasimodo | Ermetismo |

Giulia deve rispondere al messaggio di Marta, cifrando la sua risposta con la stessa chiave di Vigenère già usata da Marta. Qual è la risposta (cifrata) che Giulia invia a Marta?

<details markdown="1">
<summary>Soluzione</summary>

**EACOEFOIX**

Decifrando `ZC EZSBLNS HDIID` con la tavola monoalfabetica data si ottiene **IL PIANETA ROSSO**, cioè Marte: la chiave di Vigenère per il messaggio di Marta è quindi **MARTE**. Decifrando `ZODX HQL GHIFA HNEEIDHHA` con questa chiave si ottiene **NOME DEL POETA QUASIMODO**: è una domanda, e chiede il nome di battesimo del poeta Quasimodo, cioè **SALVATORE**. Cifrando SALVATORE con Vigenère e chiave MARTE si ottiene la risposta da inviare: **EACOEFOIX**.

</details>

### Gara 4 (Finale)

Giulia ha ricevuto tre messaggi cifrati: uno da Francesco, uno da Anna e uno da Filippo.

Quello di Francesco è: `BSQSPPD`

Quello di Anna è: `PRV UVPPVRV PSH KMDVQPG OHR`

Quello di Filippo è: `ANHEPUDFGYTORKMBSLIJZXWQCV`

Giulia sa che:

* il messaggio di **Francesco** è cifrato con Vigenère, e la chiave si trova nel messaggio di Anna;
* il messaggio di **Anna** è cifrato con una sostituzione monoalfabetica, e la chiave si trova nel messaggio di Filippo;
* il messaggio di **Filippo** è cifrato con il cifrario di Cesare, usando come chiave il numero il cui **nome scritto in lettere** ha una lunghezza pari a **un quarto** del valore stesso della chiave (per esempio "venti" ha 5 lettere, un quarto di 20: prova proprio con questo valore).

In quale città, scritta in chiaro, dovrà recarsi Giulia?

<details markdown="1">
<summary>Soluzione</summary>

**CROTONE**

La chiave del cifrario di Cesare è **20** ("venti" ha 5 lettere, cioè 20/4). Decifrando il messaggio di Filippo con questa chiave si ottiene la tavola di sostituzione monoalfabetica da usare per il messaggio di Anna:

| A | B | C | D | E | F | G | H | I | J | K | L | M | N | O | P | Q | R | S | T | U | V | W | X | Y | Z |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| G | T | N | K | V | A | J | L | M | E | Z | U | X | Q | S | H | Y | R | O | P | F | D | C | W | I | B |

Decifrando `PRV UVPPVRV PSH KMDVQPG OHR` con questa tavola si ottiene **TRE LETTERE TOP DIVENTA SPR**: applicando la stessa trasformazione TOP→SPR lettera per lettera (sottraendo, in posizione nell'alfabeto, il testo in chiaro da quello cifrato) si ricava la chiave di Vigenère **ZBC**. Decifrando infine `BSQSPPD` con Vigenère e chiave ZBC si ottiene **CROTONE**.

</details>

---
title: 'Il cifrario di Cesare'
date: '2020-04-18'
author: Fabio Mattei
layout: page
---

![Ogni lettera viene sostituita con quella traslata di N posizioni nell'alfabeto](/images/problemsolving/cifrario-cesare/cifrario-cesare.svg){:class="aside-image"}

{::options parse_block_html="true" /}
<iframe width="560" height="315" src="https://www.youtube.com/embed/Y36iDUpeAdw?si=K180bLLzr_Y_zf12" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
{::options parse_block_html="false" /}

**Crittare** (o **criptare**) un messaggio significa trasformarlo in una serie di simboli di difficile comprensione, seguendo delle regole precise (l'**algoritmo di crittazione**). Solo chi conosce le regole è in grado di **decrittare** il messaggio, cioè ricostruire il testo originale.

## Cifrario a sostituzione monoalfabetica generico

Il cifrario a sostituzione monoalfabetica sostituisce ogni simbolo del messaggio in chiaro con un altro simbolo, secondo una **tabella di conversione** (detta **chiave di crittazione**), che rimane sempre la stessa per tutto il messaggio.

Esempio di tabella di conversione:

| A | B | C | D | E | F | G | H | I | J | K | L | M | N | O | P | Q | R | S | T | U | V | W | X | Y | Z |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Q | W | E | R | T | Y | U | I | O | P | A | S | D | F | G | H | J | K | L | Z | X | C | V | B | N | M |

(la A diventa Q, la B diventa W, e così via)

Usando questa tabella, la parola **CANE** viene crittata sostituendo ogni lettera con quella corrispondente nella seconda riga: C→E, A→Q, N→F, E→T, ottenendo **EQFT**.

Per **decrittare**, si procede al contrario: si cerca la lettera crittata nella seconda riga e si legge la corrispondente lettera nella prima riga.

## Il cifrario di Cesare

Il **cifrario di Cesare** è un caso particolare (e più semplice da ricordare) di cifrario a sostituzione: ogni lettera del testo in chiaro viene sostituita dalla lettera che si trova un certo numero di posizioni dopo nell'alfabeto. Per questo si dice anche **cifrario a scorrimento**. Il numero di posizioni di scorrimento si chiama **chiave**.

Per esempio, con chiave 3 (ogni lettera "scorre" avanti di 3 posizioni), la tabella di conversione è:

| a | b | c | d | e | f | g | h | i | j | k | l | m | n | o | p | q | r | s | t | u | v | w | x | y | z |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| d | e | f | g | h | i | j | k | l | m | n | o | p | q | r | s | t | u | v | w | x | y | z | a | b | c |

Con questa chiave, la parola **CIAO** viene crittata in **FLDR** (C→F, I→L, A→D, O→R).

N.B. In un alfabeto di 26 lettere esistono **25 cifrari di Cesare diversi** (le chiavi possibili vanno da 1 a 25): con chiave 26 ogni lettera scorrerebbe in se stessa, e il testo crittato coinciderebbe con quello in chiaro.

#### Esempio

Usando un cifrario di Cesare:

1. crittare il messaggio "STAZIONE DI MILANO" con chiave 3;
2. decrittare la parola "GTQTLSF" sapendo che è stata utilizzata la chiave 5;
3. trovare la chiave con la quale "MELA" è stata crittata in "UMTI".

##### Soluzione

| 1 | VWDCLRQH GL PLODQR |
| 2 | BOLOGNA |
| 3 | 8 |

##### Commenti alla soluzione

1. Con chiave 3 ogni lettera scorre di 3 posizioni: S→V, T→W, A→D, Z→C, I→L, O→R, N→Q, E→H, D→G, I→L, M→P, I→L, L→O, A→D, N→Q, O→R. Il messaggio crittato è "VWDCLRQH GL PLODQR".
2. Con chiave 5, per decrittare si cerca ogni lettera crittata nella riga "scorsa" e si legge la lettera corrispondente in quella originale: G→B, T→O, Q→L, T→O, L→G, S→N, F→A. Si ottiene "BOLOGNA".
3. Bisogna trovare di quante posizioni scorre la M per diventare U: da M a U si contano 8 posizioni (M→N→O→P→Q→R→S→T→U). Verifichiamo con le altre lettere: E scorsa di 8 posizioni diventa M, L scorsa di 8 diventa T, A scorsa di 8 diventa I. La chiave è quindi **8**.

## Crittare e decrittare con il cifrario di Cesare in Python

Un modo comodo per implementare il cifrario di Cesare in Python è costruire una stringa con l'alfabeto e usare la funzione `find` per trovare la posizione di ogni lettera, oppure lavorare direttamente con i codici ASCII tramite `ord` e `chr`.

{% highlight python %}
alfabeto = 'abcdefghijklmnopqrstuvwxyz'

def critta_cesare(testo, chiave):
    risultato = ''
    for carattere in testo:
        if carattere in alfabeto:
            posizione = alfabeto.find(carattere)
            nuova_posizione = (posizione + chiave) % 26
            risultato = risultato + alfabeto[nuova_posizione]
        else:
            risultato = risultato + carattere   # spazi e altri simboli restano invariati
    return risultato

print(critta_cesare('ciao', 3))   # stampa: fldr
{% endhighlight %}

Per **decrittare** basta chiamare la stessa funzione con la chiave negativa (o, equivalentemente, con `26 - chiave`):

{% highlight python %}
print(critta_cesare('fldr', -3))   # stampa: ciao
{% endhighlight %}

## Esercizio proposto

Usando un cifrario di Cesare:

1. crittare il messaggio "PIAZZA DEL DUOMO" con chiave 4;
2. decrittare la parola "XLIVMRE" sapendo che è stata utilizzata la chiave 4;
3. trovare la chiave con la quale "ROMA" è stata crittata in "VSQE".


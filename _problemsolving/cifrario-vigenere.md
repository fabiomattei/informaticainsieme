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

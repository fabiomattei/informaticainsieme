---
title: 'Villaggio'
date: '2026-08-18'
author: Fabio Mattei
layout: page
---

![Il VILLAGGIO è una griglia 4x4 divisa in quattro quartieri 2x2](/images/problemsolving/villaggio/villaggio.svg){:class="aside-image"}

Il **VILLAGGIO** è un puzzle logico di composizione: bisogna disporre correttamente degli elementi all'interno di una griglia, rispettando dei vincoli di riga, colonna e blocco, fino ad arrivare all'**unica disposizione possibile**. È, in sostanza, un piccolo Sudoku a 4 simboli.

## La struttura

Il VILLAGGIO è una griglia **4x4**, suddivisa in **righe** (R1...R4) e **colonne** (C1...C4), e divisa a sua volta in **quattro quartieri**, ciascuno un blocco di 2x2 celle:

* quartiere 1: righe R1-R2, colonne C1-C2
* quartiere 2: righe R1-R2, colonne C3-C4
* quartiere 3: righe R3-R4, colonne C3-C4
* quartiere 4: righe R3-R4, colonne C1-C2

Nel villaggio vanno posizionati quattro elementi, per esempio: **A**lbero, **C**asa, ca**N**e, **G**atto (uno per lettera iniziale, tranne cane che usa la N per non confondersi con casa).

## Le regole

* In ogni **riga** devono comparire tutti e quattro gli elementi, una sola volta ciascuno.
* In ogni **colonna** devono comparire tutti e quattro gli elementi, una sola volta ciascuno.
* In ogni **quartiere** (blocco 2x2) devono comparire tutti e quattro gli elementi, una sola volta ciascuno.

La soluzione si scrive come lista di liste, una per riga, dall'alto verso il basso e da sinistra a destra:

`[[riga1],[riga2],[riga3],[riga4]]`

## Esempio

Nel VILLAGGIO sono posizionati inizialmente i seguenti elementi:

| | C1 | C2 | C3 | C4 |
|---|---|---|---|---|
| R1 | A | C | N | G |
| R2 | | G | | |
| R3 | C | | | |
| R4 | | N | | A |

Completa lo schema rispettando le regole (ogni riga, colonna e quartiere deve contenere una sola volta ciascuno dei quattro elementi).

#### Soluzione

L = [[A,C,N,G],[N,G,A,C],[C,A,G,N],[G,N,C,A]]

| | C1 | C2 | C3 | C4 |
|---|---|---|---|---|
| R1 | A | C | N | G |
| R2 | N | G | A | C |
| R3 | C | A | G | N |
| R4 | G | N | C | A |

#### Commenti alla soluzione

Il ragionamento procede per esclusione, incrociando riga, colonna e quartiere:

1. **Quartiere 1** (R1-R2, C1-C2) contiene già A, C (riga 1) e G (R2C2): manca la N, che può stare solo in R2C1 (unica cella libera del quartiere). Quindi **R2C1 = N**.
2. **Colonna 1** ora contiene A (R1), N (R2), C (R3): manca la G, che va in **R4C1 = G**.
3. **Riga 4** contiene ora G, N (data), A: manca la C, che va in **R4C3 = C**.
4. **Quartiere 4** (R3-R4, C1-C2) contiene C (R3C1), G (R4C1), N (R4C2): manca la A, che va in **R3C2 = A**.
5. **Colonna 3** contiene N (R1), C (R4): mancano A e G per R2C3 e R3C3. Il **quartiere 2** (R1-R2, C3-C4) contiene già N, G (riga1): quindi R2C3 e R2C4 devono essere, in qualche ordine, A e C. Incrociando con la colonna 3 (che ammette solo A o G in R2C3), l'unico valore compatibile è **R2C3 = A**.
6. Di conseguenza, nel quartiere 2 manca la C, che va in **R2C4 = C**.
7. **Colonna 3** ha ora N, A, C: manca la G, che va in **R3C3 = G**.
8. **Riga 3** ha ora C, A, G: manca la N, che va in **R3C4 = N**.

A questo punto tutte le celle sono riempite, e una verifica finale conferma che ogni riga, colonna e quartiere contiene tutti e quattro gli elementi una sola volta.

## Esercizio proposto

Nel VILLAGGIO sono posizionati inizialmente i seguenti elementi (usa sempre A, C, N, G):

| | C1 | C2 | C3 | C4 |
|---|---|---|---|---|
| R1 | | A | | N |
| R2 | G | | C | |
| R3 | | C | | G |
| R4 | N | | A | |

Completa lo schema e scrivi la soluzione in forma di lista L.

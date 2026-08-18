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

## Esercizi dalle gare OPS

I problemi seguenti sono tratti (e adattati) dalle gare a squadre delle Olimpiadi di Problem Solving (OPS) 2026, categoria Secondaria di secondo grado. Prova a risolverli da solo prima di aprire la soluzione (usa sempre le stesse regole: ogni riga, colonna e quartiere deve contenere una sola volta ciascuno dei quattro elementi).

### Gara 2

Nel VILLAGGIO sono posizionati inizialmente i seguenti elementi: **drago → D** in R1-C1, **castello → C** in R3-C3, **torre → T** in R2-C4, **ponte → P** in R4-C2.

| | C1 | C2 | C3 | C4 |
|---|---|---|---|---|
| R1 | D | | | |
| R2 | | | | T |
| R3 | | | C | |
| R4 | | P | | |

Scrivi il VILLAGGIO completato in forma di lista L, usando per ogni elemento la sua sigla (D, C, T, P).

<details markdown="1">
<summary>Soluzione</summary>

**L = [[D,T,P,C],[P,C,D,T],[T,D,C,P],[C,P,T,D]]**

| | C1 | C2 | C3 | C4 |
|---|---|---|---|---|
| R1 | D | T | P | C |
| R2 | P | C | D | T |
| R3 | T | D | C | P |
| R4 | C | P | T | D |

</details>

### Gara 3

Nel VILLAGGIO sono posizionati inizialmente i seguenti elementi: **albero → A** in R1-C3, **casa → C** in R2-C1, **cane → N** in R4-C1, **gatto → G** in R3-C3.

| | C1 | C2 | C3 | C4 |
|---|---|---|---|---|
| R1 | | | A | |
| R2 | C | | | |
| R3 | | | G | |
| R4 | N | | | |

Scrivi il VILLAGGIO completato in forma di lista L, usando per ogni elemento la sua sigla (A, C, N, G).

<details markdown="1">
<summary>Soluzione</summary>

**L = [[G,N,A,C],[C,A,N,G],[A,C,G,N],[N,G,C,A]]**

| | C1 | C2 | C3 | C4 |
|---|---|---|---|---|
| R1 | G | N | A | C |
| R2 | C | A | N | G |
| R3 | A | C | G | N |
| R4 | N | G | C | A |

</details>

### Gara 4 (Finale)

Nel VILLAGGIO sono posizionati inizialmente i seguenti elementi: **rosa → R** in R1-C1 e in R2-C3, **verde → V** in R4-C4, **blu → B** in R1-C4, **giallo → G** in R3-C1.

| | C1 | C2 | C3 | C4 |
|---|---|---|---|---|
| R1 | R | | | B |
| R2 | | | R | |
| R3 | G | | | |
| R4 | | | | V |

Scrivi il VILLAGGIO completato in forma di lista L, usando per ogni elemento la sua sigla (R, V, B, G).

<details markdown="1">
<summary>Soluzione</summary>

**L = [[R,G,V,B],[V,B,R,G],[G,V,B,R],[B,R,G,V]]**

| | C1 | C2 | C3 | C4 |
|---|---|---|---|---|
| R1 | R | G | V | B |
| R2 | V | B | R | G |
| R3 | G | V | B | R |
| R4 | B | R | G | V |

</details>

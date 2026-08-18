---
title: 'Il braccio meccanico'
date: '2026-08-18'
author: Fabio Mattei
layout: page
---

![Il braccio meccanico si muove lungo una circonferenza suddivisa in settori di 30°](/images/problemsolving/braccio-meccanico/braccio-meccanico.svg){:class="aside-image"}

I robot visti finora si muovono su una griglia usando **coordinate cartesiane** `[X,Y,direzione]`. Il **braccio meccanico** invece si muove secondo un sistema di riferimento **polare**: invece di righe e colonne, usa **angoli** misurati rispetto ai punti cardinali Nord, Est, Sud, Ovest.

## Lo stato del braccio

Immaginiamo il braccio al centro di un cerchio, diviso in settori di 30° (quindi 12 settori in tutto, come le ore di un orologio). Lo stato del braccio è descritto dalla coppia **[C,G]**:

* **C**: il **primo** punto cardinale che il braccio raggiungerebbe proseguendo in senso orario: può essere N (Nord), E (Est), S (Sud), W (Ovest);
* **G**: il numero di gradi che **mancano**, procedendo in senso orario, per raggiungere C: può valere 0°, 30°, 60°, ..., fino a 330° (con G=0° il braccio si trova esattamente su C).

Per esempio, lo stato `[E,60°]` indica un braccio che si trova 60° **prima** dell'Est, procedendo in senso orario (quindi tra Nord ed Est, più vicino a Nord).

## I comandi

Un comando per il braccio meccanico è composto da:

* un **segno**: **+** per ruotare in **senso orario** (verso destra, seguendo l'ordine N→E→S→W→N), **-** per ruotare in **senso antiorario** (verso sinistra, seguendo l'ordine N→W→S→E→N);
* un **numero di gradi**: multiplo di 30°, da 0° a 360°.

Per esempio, il comando `+90°` fa ruotare il braccio di 90° in senso orario.

## Come si calcola il nuovo stato

Conviene associare ad ogni punto cardinale un **angolo assoluto**, misurato in senso orario a partire da Nord: **N = 0°, E = 90°, S = 180°, W = 270°**. Poiché G indica i gradi che *mancano* per raggiungere C, lo stato `[C,G]` corrisponde all'angolo assoluto `angolo(C) - G`.

Per applicare un comando:

1. si calcola l'angolo assoluto corrispondente allo stato attuale;
2. si somma il comando se è **+** (orario), lo si sottrae se è **-** (antiorario);
3. si riporta il risultato nell'intervallo [0°, 360°) con l'operazione modulo;
4. si riconverte l'angolo assoluto ottenuto in uno stato [C,G]: **C** è il primo punto cardinale raggiunto proseguendo in senso orario (cioè il più piccolo multiplo di 90° maggiore o uguale all'angolo; se si supera 270° si arriva a 360°, cioè di nuovo Nord), **G** è la differenza tra l'angolo di C e l'angolo assoluto.

## Esempio

Il braccio meccanico si trova nello stato `[E,30°]` ed esegue la lista di comandi:

`[+90°, -150°, +300°]`

Determinare la lista S degli stati assunti dal braccio dopo ogni comando (senza contare lo stato iniziale).

#### Soluzione

S = [[S,30°],[N,0°],[N,60°]]

#### Commenti alla soluzione

Lo stato iniziale [E,30°] corrisponde all'angolo assoluto 90°-30° = 60°.

1. Comando **+90°** (orario, si somma): 60°+90° = 150°. Il primo multiplo di 90° raggiunto proseguendo in senso orario è 180° (Sud), quindi G = 180°-150° = 30°. Nuovo stato: **[S,30°]**.
2. Comando **-150°** (antiorario, si sottrae): 150°-150° = 0°. L'angolo 0° è già un multiplo di 90° (Nord), quindi G = 0°. Nuovo stato: **[N,0°]**.
3. Comando **+300°** (orario, si somma): 0°+300° = 300°. Il primo multiplo di 90° raggiunto proseguendo in senso orario, oltre i 300°, è 360° (cioè di nuovo Nord), quindi G = 360°-300° = 60°. Nuovo stato: **[N,60°]**.

## Esercizio proposto

Il braccio meccanico si trova nello stato `[N,0°]` ed esegue la lista di comandi:

`[+60°, +90°, -180°, +30°]`

Determina la lista S degli stati assunti dal braccio dopo ogni comando.

## Esercizi dalle gare OPS

I problemi seguenti sono tratti (e adattati) dalle gare a squadre delle Olimpiadi di Problem Solving (OPS) 2026, categoria Secondaria di secondo grado. Prova a risolverli da solo prima di aprire la soluzione.

### Gara 1

Un braccio meccanico è inizialmente posizionato nello stato `[W,0°]` (quindi è rivolto ad Ovest). Il braccio riceve ed esegue la seguente lista di comandi:

`L1 = [+30°, -90°, +30°, -60°, -30°]`

Riporta la lista L2 degli stati percorsi dal braccio meccanico dopo ogni comando (omettendo lo stato iniziale).

<details markdown="1">
<summary>Soluzione</summary>

**L2 = [[N,60°],[W,60°],[W,30°],[S,0°],[S,30°]]**

Lo stato iniziale [W,0°] corrisponde all'angolo assoluto 270°.

* **+30°**: 270°+30°=300° → primo multiplo di 90° raggiunto in senso orario: 360°=N, G=60° → **[N,60°]**
* **-90°**: 300°-90°=210° → 270°=W, G=60° → **[W,60°]**
* **+30°**: 210°+30°=240° → 270°=W, G=30° → **[W,30°]**
* **-60°**: 240°-60°=180° → 180°=S, G=0° → **[S,0°]**
* **-30°**: 180°-30°=150° → 180°=S, G=30° → **[S,30°]**

</details>

### Gara 2

L'ingegnere DJ Dr. ElectronicMusic ha costruito un braccio meccanico musicale, chiamato ElectronicMusic Jr, capace di suonare una melodia diversa a seconda di come e quanto viene ruotato. Alla finale della gara arrivano due musicisti: IRPStrong, che fa partire il robot dallo stato `[E,60°]` con la lista di comandi `L1 = [-60°, +150°, -120°]`; e BtTOut, che lo fa partire dallo stato `[W,30°]` con la lista di comandi `L2 = [+90°, +30°, -300°]`.

Determina la lista S1 degli stati percorsi dal braccio con i comandi di IRPStrong, e la lista S2 degli stati percorsi con i comandi di BtTOut (in entrambi i casi, omettendo lo stato iniziale).

<details markdown="1">
<summary>Soluzione</summary>

**S1 = [[N,30°],[S,60°],[N,0°]]**

Stato iniziale [E,60°] → angolo assoluto 90°-60°=30°.

* **-60°**: 30°-60°=-30°→330° → primo multiplo di 90° in senso orario: 360°=N, G=30° → **[N,30°]**
* **+150°**: 330°+150°=480°→120° → 180°=S, G=60° → **[S,60°]**
* **-120°**: 120°-120°=0° → 0°=N, G=0° → **[N,0°]**

**S2 = [[N,30°],[N,0°],[E,30°]]**

Stato iniziale [W,30°] → angolo assoluto 270°-30°=240°.

* **+90°**: 240°+90°=330° → 360°=N, G=30° → **[N,30°]**
* **+30°**: 330°+30°=360°→0° → 0°=N, G=0° → **[N,0°]**
* **-300°**: 0°-300°=-300°→60° → 90°=E, G=30° → **[E,30°]**

</details>

### Gara 3

Un braccio meccanico è inizialmente posizionato nello stato `[E,0°]`. Il braccio riceve ed esegue la seguente lista di comandi:

`L1 = [-90°, +60°, +150°, -30°, -120°, +30°, -60°, 0°, -150°, +240°]`

Riporta la lista L2 degli stati percorsi dal braccio meccanico dopo ogni comando (omettendo lo stato iniziale).

<details markdown="1">
<summary>Soluzione</summary>

**L2 = [[N,0°],[E,30°],[W,60°],[S,0°],[E,30°],[E,0°],[E,60°],[E,60°],[W,30°],[S,60°]]**

Stato iniziale [E,0°] → angolo assoluto 90°.

* **-90°**: 90°-90°=0° → N, G=0° → **[N,0°]**
* **+60°**: 0°+60°=60° → 90°=E, G=30° → **[E,30°]**
* **+150°**: 60°+150°=210° → 270°=W, G=60° → **[W,60°]**
* **-30°**: 210°-30°=180° → S, G=0° → **[S,0°]**
* **-120°**: 180°-120°=60° → 90°=E, G=30° → **[E,30°]**
* **+30°**: 60°+30°=90° → E, G=0° → **[E,0°]**
* **-60°**: 90°-60°=30° → 90°=E, G=60° → **[E,60°]**
* **0°**: resta a 30° → **[E,60°]**
* **-150°**: 30°-150°=-120°→240° → 270°=W, G=30° → **[W,30°]**
* **+240°**: 240°+240°=480°→120° → 180°=S, G=60° → **[S,60°]**

</details>

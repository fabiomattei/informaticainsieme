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

* **C**: il punto cardinale più vicino, in senso orario, alla posizione attuale del braccio: può essere N (Nord), E (Est), S (Sud), W (Ovest);
* **G**: il numero di gradi di cui il braccio è ruotato **oltre** C, in senso orario: può valere 0°, 30°, 60°, ..., fino a 330° (a 360° si ritorna a C stesso, con G=0°).

Per esempio, lo stato `[E,60°]` indica un braccio che si trova 60° dopo l'Est, procedendo in senso orario (quindi, tra Est e Sud).

## I comandi

Un comando per il braccio meccanico è composto da:

* un **segno**: **+** per ruotare in **senso orario** (verso destra, seguendo l'ordine N→E→S→W→N), **-** per ruotare in **senso antiorario** (verso sinistra, seguendo l'ordine N→W→S→E→N);
* un **numero di gradi**: multiplo di 30°, da 0° a 360°.

Per esempio, il comando `+90°` fa ruotare il braccio di 90° in senso orario.

## Come si calcola il nuovo stato

Conviene associare ad ogni punto cardinale un **angolo assoluto**, misurato in senso orario a partire da Nord: **N = 0°, E = 90°, S = 180°, W = 270°**. Lo stato `[C,G]` corrisponde quindi all'angolo assoluto `angolo(C) + G`.

Per applicare un comando:

1. si calcola l'angolo assoluto corrispondente allo stato attuale;
2. si somma il comando se è **+** (orario), lo si sottrae se è **-** (antiorario);
3. si riporta il risultato nell'intervallo [0°, 360°) con l'operazione modulo;
4. si riconverte l'angolo assoluto ottenuto in uno stato [C,G]: **C** è il punto cardinale più vicino, senza superarlo, procedendo in senso orario (cioè il multiplo di 90° immediatamente prima o uguale all'angolo), **G** è la differenza tra l'angolo assoluto e l'angolo di C.

## Esempio

Il braccio meccanico si trova nello stato `[E,30°]` ed esegue la lista di comandi:

`[+90°, -150°, +300°]`

Determinare la lista S degli stati assunti dal braccio dopo ogni comando (senza contare lo stato iniziale).

#### Soluzione

S = [[S,30°],[N,60°],[N,0°]]

#### Commenti alla soluzione

Lo stato iniziale [E,30°] corrisponde all'angolo assoluto 90°+30° = 120°.

1. Comando **+90°** (orario, si somma): 120°+90° = 210°. Il multiplo di 90° immediatamente prima è 180° (Sud), quindi G = 210°-180° = 30°. Nuovo stato: **[S,30°]**.
2. Comando **-150°** (antiorario, si sottrae): 210°-150° = 60°. Il multiplo di 90° immediatamente prima di 60° è 0° (Nord), quindi G = 60°-0° = 60°. Nuovo stato: **[N,60°]**.
3. Comando **+300°** (orario, si somma): 60°+300° = 360°, che riportato nell'intervallo [0°,360°) diventa 0°. Corrisponde a Nord con G=0°. Nuovo stato: **[N,0°]**.

## Esercizio proposto

Il braccio meccanico si trova nello stato `[N,0°]` ed esegue la lista di comandi:

`[+60°, +90°, -180°, +30°]`

Determina la lista S degli stati assunti dal braccio dopo ogni comando.

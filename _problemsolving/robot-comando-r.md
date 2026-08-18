---
title: 'Robot con comando r'
date: '2026-08-18'
author: Fabio Mattei
layout: page
---

Oltre ai comandi visti per il [robot classico]({{ site.baseurl }}{% link _problemsolving/movimento-robot.md %}.html) (**o**, **a**, **f**), esiste una variante di robot capace di **ripetere** un blocco di comandi un certo numero di volte, senza doverlo scrivere più volte per esteso.

## Il comando r

Il comando **r** è seguito da un **numero di ripetizioni**, poi da una sequenza di comandi chiamata **corpo**, racchiusa tra i simboli `<` e `>`.

Per esempio, il comando `r3<f,o,f>` significa: esegui **3 volte** la sequenza `f,o,f`.

## Come risolvere questi problemi

Il modo più semplice per gestire il comando r è **espanderlo**: si sostituisce `r<N><corpo>` con **N copie consecutive** del corpo, ottenendo così una lista di comandi equivalente, compatibile con il robot classico (fatta solo di o, a, f). A quel punto si esegue normalmente, passo dopo passo.

## Esempio 1

Un robot parte dallo stato `[4,4,N]` ed esegue il comando `r2<f,f,o>`.

#### Soluzione

Espandiamo il comando: `r2<f,f,o>` diventa `f,f,o,f,f,o` (due copie del corpo).

| Stato di partenza | Comando | Stato di arrivo |
|---|---|---|
| [4,4,N] | f | [4,5,N] |
| [4,5,N] | f | [4,6,N] |
| [4,6,N] | o | [4,6,E] |
| [4,6,E] | f | [5,6,E] |
| [5,6,E] | f | [6,6,E] |
| [6,6,E] | o | [6,6,S] |

Lo stato finale è **[6,6,S]**.

## Esempio 2

Un robot parte dallo stato `[5,5,N]` ed esegue la lista di comandi `[a, r3<f,f,a>, f]`.

#### Soluzione

Espandiamo la lista, sostituendo `r3<f,f,a>` con tre copie del corpo `f,f,a`:

`[a, f,f,a, f,f,a, f,f,a, f]`

| Stato di partenza | Comando | Stato di arrivo |
|---|---|---|
| [5,5,N] | a | [5,5,W] |
| [5,5,W] | f | [4,5,W] |
| [4,5,W] | f | [3,5,W] |
| [3,5,W] | a | [3,5,S] |
| [3,5,S] | f | [3,4,S] |
| [3,4,S] | f | [3,3,S] |
| [3,3,S] | a | [3,3,E] |
| [3,3,E] | f | [4,3,E] |
| [4,3,E] | f | [5,3,E] |
| [5,3,E] | a | [5,3,N] |
| [5,3,N] | f | [5,4,N] |

Lo stato finale è **[5,4,N]**.

#### Commenti alla soluzione

Ogni volta che si incontra il comando **a** si ruota in senso antiorario (N→W→S→E→N): partendo da N si passa a W, poi (dopo il secondo blocco) a S, poi a E, infine di nuovo a N. Il comando r ha semplicemente permesso di scrivere in modo compatto tre ripetizioni dello stesso blocco `f,f,a`.

## Esercizio proposto

Un robot parte dallo stato `[6,2,S]` ed esegue la lista di comandi `[f, r2<f,o,f,f>, a]`.

Determina lo stato finale del robot (suggerimento: espandi prima la lista sostituendo il comando r con le sue copie).

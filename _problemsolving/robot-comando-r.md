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

## Esercizio dalla gara OPS

Il problema seguente è tratto (e adattato) dalla Gara 1 delle gare a squadre delle Olimpiadi di Problem Solving (OPS) 2026, categoria Secondaria di secondo grado. Prova a risolverlo da solo prima di aprire la soluzione.

L'esploratrice Beanne si è addentrata in una zona inesplorata della giungla alla ricerca di rari campioni botanici. Nella zona si trovano quattro oggetti:

* un fiore raro nella casella [6,6];
* una radice curativa nella casella [6,5];
* un fungo blu nella casella [5,5];
* una foglia d'argento nella casella [4,5].

Beanne parte dalla posizione [6,3] ed è inizialmente rivolta verso Nord (N). Esegue la seguente lista di comandi: `L1 = [r3<f>, o, r2<f,a>, a, f]`.

Determina:

1. lo stato S1 di Beanne dopo aver eseguito i comandi della lista L1 fino al **primo comando r incluso**;
2. la lista L2 di tutti gli stati assunti dall'esploratrice durante il percorso (compresi lo stato iniziale e quello finale);
3. il numero K di oggetti che Beanne riesce a raccogliere lungo il percorso (un oggetto viene raccolto se Beanne transita sulla sua casella).

<details markdown="1">
<summary>Soluzione</summary>

**S1 = [6,6,N]**

**L2 = [[6,3,N],[6,4,N],[6,5,N],[6,6,N],[6,6,E],[7,6,E],[7,6,N],[7,7,N],[7,7,W],[7,7,S],[7,6,S]]**

**K = 2**

Espandendo la lista comandi (`r3<f>` diventa `f,f,f`, `r2<f,a>` diventa `f,a,f,a`) si ottiene: `f,f,f,o,f,a,f,a,a,f`.

Beanne parte da [6,3,N]. Il comando `r3<f>` la porta, un passo alla volta, in [6,4,N], [6,5,N] (qui trova la radice curativa) e [6,6,N] (qui trova anche il fiore raro): questo è lo stato **S1**. Il comando `o` la ruota in [6,6,E]. Il comando `r2<f,a>` esegue due volte `f,a`: prima `f` la porta in [7,6,E], poi `a` la ruota in [7,6,N]; di nuovo `f` la porta in [7,7,N], poi `a` la ruota in [7,7,W]. Il comando `a` finale la ruota in [7,7,S], e l'ultimo comando `f` la porta in [7,6,S], lo stato finale.

Le caselle [5,5] (fungo) e [4,5] (foglia) non vengono mai attraversate: Beanne raccoglie quindi solo la radice curativa e il fiore raro, **K = 2**.

</details>

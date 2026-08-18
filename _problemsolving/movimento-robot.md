---
title: 'Il movimento di un robot'
date: '2020-04-18'
author: Fabio Mattei
layout: page
---

![I comandi di movimento spostano il robot cella per cella sulla griglia](/images/problemsolving/movimento-robot/movimento-robot.svg){:class="aside-image"}

{::options parse_block_html="true" /}
<iframe width="560" height="315" src="https://www.youtube.com/embed/1sxT2iUDKPY?si=FwLYFWaQECS0hOkn" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
{::options parse_block_html="false" /}

Immaginiamo un foglio a quadretti su cui è disegnato un **campo di gara**. Ogni casella è individuata da due numeri interi, le sue **coordinate**: la prima coordinata (**ascissa**) indica la colonna (da sinistra), la seconda (**ordinata**) indica la riga (dal basso).

Il robot occupa sempre una casella e guarda in una delle quattro direzioni cardinali: **E** (destra), **S** (basso), **W** (sinistra), **N** (alto). Lo **stato** del robot è quindi descritto da una lista di tre valori: `[ascissa, ordinata, direzione]`. Per esempio lo stato `[2,3,E]` indica un robot nella casella di coordinate (2,3), rivolto verso destra.

## I comandi

Il robot "classico" esegue tre tipi di comando:

* **o**: ruota di 90° in senso **o**rario, senza cambiare casella;
* **a**: ruota di 90° in senso **a**ntiorario, senza cambiare casella;
* **f**: avanza di una casella nel verso in cui è rivolto, mantenendo l'orientamento.

Per ruotare di 180° si usa sempre il doppio comando **a,a** (o, equivalentemente, o,o).

Una sequenza di comandi è descritta da una lista, ad esempio `[f,f,o,f,a,f,f]`, ed è eseguita comando dopo comando, aggiornando ogni volta lo stato del robot.

## Esempio

Un robot parte dallo stato `[3,3,N]` (casella (3,3), rivolto verso l'alto) ed esegue la lista di comandi `[f,f,o,f,a,f,f]`.

Determinare lo stato del robot dopo ogni comando.

#### Soluzione

| Stato di partenza | Comando | Stato di arrivo |
|---|---|---|
| [3,3,N] | f | [3,4,N] |
| [3,4,N] | f | [3,5,N] |
| [3,5,N] | o | [3,5,E] |
| [3,5,E] | f | [4,5,E] |
| [4,5,E] | a | [4,5,N] |
| [4,5,N] | f | [4,6,N] |
| [4,6,N] | f | [4,7,N] |

Lo stato finale del robot è **[4,7,N]**.

#### Commenti alla soluzione

I comandi **f** spostano il robot di una casella nella direzione in cui è rivolto (verso l'alto aumenta l'ordinata, verso destra aumenta l'ascissa, e così via), mentre i comandi **o** e **a** cambiano solo la direzione, ruotando rispettivamente in senso orario (N→E→S→W→N) e antiorario (N→W→S→E→N), senza mai spostare il robot.

## Varianti più avanzate

Oltre al robot "classico" appena descritto, esistono varianti più elaborate, spesso usate nei problemi di gara:

* il [robot con comando r]({{ site.baseurl }}{% link _problemsolving/robot-comando-r.md %}.html), capace di ripetere un blocco di comandi un certo numero di volte;
* il [robot con memoria]({{ site.baseurl }}{% link _problemsolving/robot-memoria.md %}.html), capace di memorizzare e richiamare sotto-sequenze di comandi;
* il [braccio meccanico]({{ site.baseurl }}{% link _problemsolving/braccio-meccanico.md %}.html), che si muove in un sistema di riferimento polare invece che cartesiano.

## Esercizio proposto

Un robot parte dallo stato `[5,2,W]` ed esegue la lista di comandi `[f,f,a,f,o,f,f,o,f]`.

Determina lo stato finale del robot.

---
id: 464
title: 'Il problema di Knapsack'
date: '2020-02-21T20:47:35+01:00'
author: fabio
layout: revision
guid: 'https://www.esercizidiinformatica.it/blog/2020/02/21/430-autosave-v1/'
permalink: '/?p=464'
---

Il problema di **knapsack** (zainetto) o problema di **rucksack** è un problema di ottimizzazione combinatoria: Dato un insieme di oggetti, ognuno dei quali possiede un certo peso ed un certo valore, determinare un sottoinsieme da includere in una collezione in modo tale che la somma dei pesi di questo sottoinsieme rispetti il vincolo di capacità dello zainetto. Nel determinare il sottoinsieme fare in modo che la somma dei valori sia la più grande possibile.

<figure class="wp-block-image size-large">![](https://www.esercizidiinformatica.it/wp-content/uploads/2020/02/500px-Knapsack.svg_.png)</figure>Il problema viene spesso affrontato nei sistemi di logistica e di allocazione di risorse dove per esempio un manager deve scegliere dove impiegare le risorse monetarie di cui dispone al fine di ottenere il miglior risultato possibile. Il problema ha il vincolo implicito del tempo, si cerca la migliore soluzione calcolabile in un tempo determinato.

#### Esempio:

In un deposito di minerali esistono esemplari di vario peso e valore individuati da sigle di riconoscimento. Ciascun minerale è descritto da una sigla che contiene le seguenti informazioni:

tab(&lt;sigla del minerale&gt;, &lt;valore in euro&gt;, &lt;peso in Kg&gt;).

Il deposito contiene i seguenti minerali:

- tab(m1,15,35)
- tab(m2,19,46)
- tab(m3,14,25)
- tab(m4,10,12)

Disponendo di un piccolo motocarro con portata massima di 59 Kg trovare la lista L delle sigle di due minerali diversi che siano trasportabili contemporaneamente con questo mezzo e che abbiano il massimo valore complessivo; calcolare inoltre questo valore V.

Soluzione

<figure class="wp-block-table">| L | m2, m4 |
|---|---|
| V | 19 + 10 = 29 |
| P | 46 + 12 = 58 |

</figure>**COMMENTI ALLA SOLUZIONE**  
Per risolvere il problema occorre considerare *tutte* le possibili *combinazioni* di due minerali diversi, il loro valore e il loro peso.

N.B. Le *combinazioni* corrispondono ai sottoinsiemi: cioè sono indipendenti dall’ordine; per esem- pio la combinazione “m1, m4” è uguale alla combinazione “m4, m1”. Quindi per elencarle tutte (una sola volta) conviene costruirle sotto forma di liste i cui elementi sono ordinati, come richiesto dal problema: si veda di seguito.  
 Costruite le combinazioni occorre individuare quelle trasportabili (cioè con peso complessivo minore o eguale a 59) e tra queste scegliere quella di maggior valore.

<figure class="wp-block-table">| Combinazioni | Valore | Peso | Trasportabili |
|---|---|---|---|
| \[m1,m2\] | 15+19=34 | 35+46=81 | no |
| \[m1,m3\] | 15+14=29 | 35+25=60 | no |
| \[m1,m4\] | 15+10=25 | 35+12=47 | si |
| \[m2,m3\] | 19+14=33 | 46+25=71 | no |
| \[m2,m4\] | 19+10=29 | 46+12=58 | si |
| \[m3,m4\] | 14+10=24 | 25+12=37 | si |

</figure>Dal precedente prospetto la soluzione si deduce facilmente.

#### Esercizio 1
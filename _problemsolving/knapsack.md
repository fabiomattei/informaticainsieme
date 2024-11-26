---
title: 'Il problema di Knapsack'
date: '2020-04-18'
author: Fabio Mattei
layout: page
---

{::options parse_block_html="true" /}
<iframe width="560" height="315" src="https://www.youtube.com/embed/9bdZWeIrxrM?si=Sp2J3ybnWSNhdyCN" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
{::options parse_block_html="false" /}



Il problema di **knapsack** (zainetto) o problema di **rucksack** è un problema di ottimizzazione combinatoria.
Enunciao: dato uno zainetto con una **capacità limitata** e dato un insieme di oggetti, ognuno dei quali possiede un certo **peso** ed un certo **valore**, determinare un sottoinsieme di oggetti da includere in una collezione in modo tale che la somma dei pesi di questo sottoinsieme rispetti il **vincolo di capacità** dello zainetto. Nel determinare il sottoinsieme fare in modo che la somma dei valori sia la più grande possibile.

![Problema di knapsack](/images/problemsolving/knapsack.png){:class="aside-image"}
Il problema viene spesso affrontato nei sistemi di logistica e di allocazione di risorse dove per esempio un manager deve scegliere dove impiegare le risorse monetarie di cui dispone al fine di ottenere il miglior risultato possibile. Il problema ha il vincolo implicito del tempo, si cerca la migliore soluzione calcolabile in un tempo determinato.

Il problema ammette tre tipi di soluzione:

* **soluzione ammissibile**: il sottoinsieme di oggetti che si è determinato rispetta il vincolo di capacità
* **soluzione non ammissibile**: il sottoinsieme di oggetti che si è determinato NON rispetta il vincolo di capacità
* **soluzione ottima**: soluzione ammissibile (rispetta il vincolo di capacità) la cui somma dei valori è la più grande possibile


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

| L | m2, m4 |
|---|---|
| V | 19 + 10 = 29 |
| P | 46 + 12 = 58 |


### COMMENTI ALLA SOLUZIONE  
Per risolvere il problema occorre considerare *tutte* le possibili *combinazioni* di due minerali diversi, il loro valore e il loro peso.

N.B. Le *combinazioni* corrispondono ai sottoinsiemi: cioè sono indipendenti dall’ordine; per esem- pio la combinazione “m1, m4” è uguale alla combinazione “m4, m1”. Quindi per elencarle tutte (una sola volta) conviene costruirle sotto forma di liste i cui elementi sono ordinati, come richiesto dal problema: si veda di seguito.  
 Costruite le combinazioni occorre individuare quelle trasportabili (cioè con peso complessivo minore o eguale a 59) e tra queste scegliere quella di maggior valore.

| Combinazioni | Valore | Peso | Trasportabili |
|---|---|---|---|
| \[m1,m2\] | 15+19=34 | 35+46=81 | no |
| \[m1,m3\] | 15+14=29 | 35+25=60 | no |
| \[m1,m4\] | 15+10=25 | 35+12=47 | si |
| \[m2,m3\] | 19+14=33 | 46+25=71 | no |
| \[m2,m4\] | 19+10=29 | 46+12=58 | si |
| \[m3,m4\] | 14+10=24 | 25+12=37 | si |

Dal precedente prospetto la soluzione si deduce facilmente.

#### Esercizio 1

In un deposito di minerali esistono esemplari di vario peso e valore individuati da sigle di ricono- scimento. Ciascun minerale è descritto da un termine che contiene le seguenti informazioni: minerale(&lt;sigla minerale &gt;,&lt;valore&gt;,&lt;peso&gt; ). Il deposito contiene i seguenti minerali:

- minerale(m1,39,58)
- minerale(m2,42,64)
- minerale(m3,40,65)
- minerale(m4,38,59)
- minerale(m5,37,61)
- minerale(m6,42,62)

I minerali possono essere spostati con carrelli di diversa portata su cui si possono mettere tre esemplari (diversi).

- Disponendo di un carrello con portata massima di 180 Kg, trovare la lista L1 delle sigle di tre minerali diversi che siano trasportabili contemporaneamente e che abbiano il massimo valore complessivo.
- Disponendo di un carrello con portata massima di 185 Kg, trovare la lista L2 delle sigle di tre minerali diversi che siano trasportabili contemporaneamente e che abbiano il massimo valore complessivo.
- Disponendo di un carrello con portata massima di 200 Kg, trovare la lista L1 delle sigle di tre minerali diversi che siano trasportabili contemporaneamente e che abbiano il massimo valore complessivo.
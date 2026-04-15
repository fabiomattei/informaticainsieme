---
title: Breve storia del microprocessore
date: '2026-02-09T10:58:13+02:00'
author: Fabio Mattei
layout: page
---

Il processore (o CPU - Central Processing Unit) è il **cervello** del computer, responsabile dell'elaborazione di dati, istruzioni e programmi. Esegue calcoli logici e aritmetici, coordina le periferiche e gestisce il flusso di informazioni tra i vari tipi di memoria e sistema operativo.

I processori si sono evoluti nell'arco del tempo.

Il primo dispositivo identificato come processore fu l'**Intel 4004** costruito nel **1971** con progetto guidato dall'italiano **Federico Faggin**. Questa invenzione ha rivoluzionato l'informatica segnando il passando da calcolatori ingombranti a sistemi programmabili e compatti, innescando la rivoluzione dei personal computer e dei dispositivi moderni. 

## La legge di Moore

Il ritmo dell'innovazione tecnologica, nei primi anni seguì la legge di Moore.

La Legge di Moore è una previsione formulata nel 1965 da Gordon Moore, cofondatore di Intel
basata sulla sua osservazione del progresso tecnologico.

> Il numero di transistor su un chip raddoppia ogni anno e mezzo

La legge di Moore per molti anni ha testimoniato un aumento lineare della potenza di calcolo dei processori e 
della capacità di memoria delle RAM al trascorrere del tempo. 

A partire dall'anno 2000 però si è cominciato a raggiungere il limite fisico della possibilità di miniaturizzare 
un transistor basato sul silicio e l'incremento tecnologico ha cominciato a diminuire.

## Evoluzione della tecnologia con l'andare del tempo

Andiamo a vedere un po' di date importanti per la storia del processori.

| Azienda | Processore   | Anno         | Frequenza    | Core   |
| ------- | ------------ | ------------ | ------------ | ------ |
| Intel   | 4004         | 1971         | 740Khz       | 1      |
| Intel   | 8086         | 1978         | 3Mhz - 8Mhz  | 1      |
| Motorla | 68000        | 1979         | 8Mhz         | 1      |
| Intel   | Pentium      | 1993         | 100Mhz       | 1      |
| Intel   | Pentium 3    | 1999         | 1Ghz         | 1      |
| Intel   | Core ultra 3 | 2026         | 3Ghz         | 6 - 12 |

Notiamo come negli anni che vanno dal 1978 al 1999 c'è stato un aumento di 3 ordini di grandezza
nella velocità dei processori. Negli anni che vanno dal 2000 al 2026 c'è stato un aumento di fattore 3.

### Frequenza

La frequenza del processore misurata in hertz, indica il numero di cicli di istruzioni 
che una CPU esegue in un secondo.

### Core

Un core del processore è l'unità di elaborazione fisica e indipendente all'interno di una CPU (Central Processing Unit) capace di eseguire istruzioni, calcoli e operazioni logiche. I processori moderni sono "multicore", ovvero integrano più nuclei in un singolo chip per aumentare le prestazioni in multitasking e la velocità di calcolo complessiva.

Quando l'industria si è scontrata con il limite fisico dell'impossibilità di miniaturizzare i processori ha iniziato
a creare processori che integravano più core.

Il disporre di più core dà la possibilità al processore di aumentare la velocità **parallelizzando**
i calcoli tra i diversi core. Questa necessità si inizia a porre nel momento in cui risulta sempre più difficile aumentare la velocità del singolo core.

## ASML (Advanced Semiconductor Materials Lithography)

La creazione dei microprocessori si basa su di un processo che si chiama **fotolitografia**.

La ASML è l'azienda olandese che produce le macchine che creano i microprocessori.

La ASML è specializzata nello sviluppo e nella produzione di macchine per fotolitografia utilizzate per 
produrre chip per computer. Dal 2023 è il più grande fornitore per l'industria dei semiconduttori e l'unico fornitore al mondo di macchine per fotolitografia per litografia ultravioletta estrema (EUV), utilizzate per produrre i chip più avanzati.

Guarda il documentario [Why The World Relies On ASML For Machines That Print Chips](https://www.youtube.com/watch?v=iSVHp6CAyQ8).

## Impatto della tecnologia sulla complessità

Andiamo a calcolare il tempo impiegato per risolvere un algoritmo da 3 diverse CPU:

* la prima ha una frequenza di 1Mhz (1978)
* la seconda di 100Mhz (1993)
* la terza di 1Ghz (1999)

Andiamo a calcolare quanto tempo ciascuna di queste CPU impiega per algoritmi di diversa complessità
per una dimensione del problema pari a 10.

### N=10 

| Complessità     | 1Mhz      |  100Mhz   |  1Ghz     | 
| --------------- | --------- | --------  | --------- |
| Costante        | 0.01 s    | 0.0001 s  | 0.00001 s |
| Logaritmica     | 0.01 s    | 0.0001 s  | 0.00001 s |
| Lineare         | 0.02 s    | 0.0002 s  | 0.00001 s |
| Quadratica      | 0.1 s     | 0.001 s   | 0.0001 s  | 
| Esponenziare 2<sup>n</sup> | 1 s       | 0.01 s    | 0.001 s   | 

Ora andiamo a calcolare quanto tempo ciascuna di queste CPU impiega per algoritmi di diversa complessità
per una dimensione del problema pari a 60.

### N=60 

| Complessità     | 1Mhz          |  100Mhz     |  1Ghz      | 
| --------------- | ------------- | ----------  | ---------- |
| Costante        | 0.01 s        | 0.0001 s    | 0.00001 s  |
| Logaritmica     | 0.016 s.      | 0.00016 s   | 0.000016 s |
| Lineare         | 0.06 s        | 0.0006 s    | 0.00006 s  |
| Quadratica      | 3.6 s         | 0.36 s      | 0.0036 s   | 
| Esponenziare 2<sup>n</sup> | 366000 secoli | 3660 secoli | 366 secoli | 

Ciò che si può notare è che il tempo di risuluzione di algorimo a complessità costante non ha avuto alcun 
tipo di impatto dall'innovazione tecnologica.
Gli algoritmi con complessità algortmica logaritmica e quadratica sono stati risolti molto più velocemente
ma questo incremento di velocità in effetti non è apprezzabile dall'operatore umano.
Gli agoritmi con complessità quadratica hanno subito un impatto apprezzabile. 
Gli algoritmi con complessità esponenziale subiscono un forte impatto ma comunque non sono risolvibili in
tempi umanamente apprezzabili.
Dunque nonostante gli avanzamenti tecnologici dell'hardware dei processori
lo scrivere bene un algoritmo al fine di abbassarne la complessità 
ha più valore di quanto ne abbia il possedere un hardware più veloce.





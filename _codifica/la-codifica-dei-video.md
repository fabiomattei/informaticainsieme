---
id: 569
title: 'La codifica dei video'
date: '2020-10-08T21:50:15+02:00'
author: Fabio Mattei
layout: page
guid: 'https://www.esercizidiinformatica.it/?page_id=569'
---

![Sequenza Bambino in skate](/images/codifica/sequenzabimbo.jpg)

Image by <a href="https://www.freepik.com/free-vector/hand-drawn-animation-frames-element-collection_33591459.htm#query=animation%20sequence&position=5&from_view=keyword&track=ais_hybrid&uuid=41e4d610-e232-46ba-8a71-5ebcd6582239">Freepik</a>

Un video viene codificato attraverso una **sequenza di immagini**, molto simili tra loro che si susseguono ad una velocità 
molto alta e che è costante nel tempo.
Nell'immagine in alto vediamo come l'animazione del bambino in skate è ottenuta attraverso immagini molto simili tra loro. 
Se vedessimo queste immagini susseguirsi una dopo l'altra, molto velocemente, avremmo l'illusione di vedere un bambino che si muove.
E' così che si fanno i cartoni animati.
 
Tutto si basa su di un fenomeno chiamato **persistenza della visione** il cervello umano interpreta una **sequenza di immagini** 
che si succedono come un **flusso in movimento**.

Codificare un video dunque consiste nel codificare una **moltitudine di immagini**.

Se andiamo a contare quanti fotogrammi si susseguono in una unità di tempo otteniamo una misura di velocità.
Nel caso delle codifiche video l'unità di tempo è espressa nei termini di un secondo.
La misura di Fotogrammi per secondo (FPS) anche detta in inglese **frame rate** esprime la quantità di fotogrammi che
si susseguono in un secondo.

Alcune frame rate che si trovano negli standard più comuni sono:

- 24 FPS : Cinema (720 x 576)
- 25 FPS : Televisione PAL (720 x 480)
- 30 FPS : Televisione NTSC (1920 x 1020)

Questo porta a necessità di memorizzazione molto elevate.

Es: 1 minuto PAL: 60 x 25 x 720 x 576 = 622.080.000 pixel al minuto.

#### Bisogna imparare a risparmiare spazio di memoria

Sfruttando le **similitudini** all’interno delle immagini possiamo **comprimere** l’informazione. 
In effetti se una porzione di immagine viene **ripetuta** possiamo evitare di memorizzarla due volte.

Esistono due tipi di compressione:

- la compressione **intraframe**: sfrutta le similitudini esistenti all’interno di una immagine (es: aree di colore unformi molto estese) e si concretizza nelle tecniche di compressione quando si lavora su di una immagine singola;
- la compressione **interframe**: sfrutta le similitudini presenti in sequenze di fotogrammi successivi (molto comuni come osservabile dalla animazione del bambino in skate).

![Intraframe](/images/codifica/intraframe.png)

#### Lo standard MPEG

Nel tempo si sono sviluppati degli standard per le codifiche video. Facciamo qualche esempio.

Lo standard MPEG-1 e’ quello utilizzato dai Video CD e supporta un numero ristretto di dimensioni e di velocita’ di trasmissione.

Lo standard MPEG-2 e’ quello usato nei DVD e nelle trasmissioni digitali terrestri e satellitari. Esso estende MPEG-1 per velocita’ di trasmissione e caratteristiche supportate.

MPEG-4 e’ uno standard invece destinato a fornire compressioni superiori e a lavorare a velocita’ di trasmissione ridotte. Ovviamente questo si fa a spese della qualità.

#### La bit-rate

Una delle realtà moderne è la trasmissione di filmati in streaming (youtube o netflix) che noi osserviamo dal cellulare o dal computer.
Dal momento che i filmati possono essere estremamente pesanti, è importante poter stimare la quantità di dati che 
dobbiamo trasmettere in un certo tempo per assicurare una visione corretta e senza buchi.
In effetti un film che viene trasmesso in streaming ha bisogno che si riesca a trasmettere una certa quantità di dati 
in un certo tempo per un lungo lasso de tempo affinché la visione del film sia indisturbata.
Si definisce **bit-rate** la quantità media di bit richiesta per codificare un secondo di filmato. 
Nell’MPEG si ha il problema di dover garantire una bit-rate costante. 
La qualità del filmato viene quindi dinamicamente alterata in modo da conservare la bit-rate: più questo valore è alto, 
migliore è la resa del filmato.


---
id: 569
title: 'La codifica dei video'
date: '2020-10-08T21:50:15+02:00'
author: fabio
layout: page
guid: 'https://www.esercizidiinformatica.it/?page_id=569'
---

Un video viene codificato con una sequenza di immagini che si susseguono ad una velocità costante nel tempo. Per un fenomeno chiamato “persistenza della visione” il cervello umano interpreta una sequenza di immagini che si succedono come un flusso in movimento.

La velocita’ a cui si susseguono i fotogrammi è misurata in fotogrammi al secondo (FPS).Questa velocità è chiamata frame rate.

Alcune frame rate comuni sono:

- 24 FPS : Cinema (720 x 576)
- 25 FPS : Televisione PAL (720 x 480)
- 30 FPS : Televisione NTSC (1920 x 1020)

Questo porta a necessità di memorizzazione molto elevate.

Es: 1 minuto PAL: 60 x 25 x 720 x 576 = = 622.080.000 pixel al minuto.

#### Bisogna imparare a risparmiare spazio di memoria

Sfruttando le **similitudini** all’interno delle immagini possiamo **comprimere** l’informazione. In effetti se una porzione di immagine viene **ripetuta** possiamo evitare di memorizzarla due volte.

Esistono due tipi di compressione:

- la compressione intraframe, sfrutta le similitudini esistenti all’interno di una immagine
- la compressione inerframe sfrutta le similitudini presenti in sequenze di fotogrammi successivi

#### Lo standard MPEG

Lo standard MPEG-1 e’ quello utilizzato dai Video CD e supporta un numero ristretto di dimensioni e di velocita’ di trasmissione.

Lo standard MPEG-2 e’ quello usato nei DVD e nelle trasmissioni digitali terrestri e satelliatri. Esso estende MPEG-1 per velocita’ di trasmissione e caratteristiche supportate.

MPEG-4 e’ uno standard invece destinato a fornire compressioni superiori e a lavorare a velocita’ di trasmissione ridotte

#### La bit-rate

Dal momento che i filmati possono essere estramamente pesanti, è importante poter stimare la quantita di memoria necessaria in base alla lora lunghezza. Viene chiamata bit-rate la quantità media di bit richiesta per codificare un secondo di filmato. Nell’MPEG si ha il problema di dover garantire una bit-ratecostante. La qualità del filmato viene quindi dinamicamente alterata in modo da conservare la bit-rate: più questo valore è alto, migliore è la resa del filmato.
---
id: 549
title: 'La codifica delle immagini'
date: '2020-10-06T23:32:06+02:00'
author: Fabio Mattei
layout: page
guid: 'https://www.esercizidiinformatica.it/?page_id=549'
---

Una immagine, dul punto di vista di un computer, è come un **mosaico**, formato da una moltitudine di tessere, ogni tessera costituita da un solo colore.

Guardando il mosaico da lontano si perde visione delle singole tessere e si vede l'immagine come un **disegno continuo**.

![Mosaico](/images/codifica/mosaico.jpg)

![RGB](/images/codifica/rgb.png)

Computer ottiene qsiasi colore come somma dtre colori primari: **Red**, **Green** e **Blue**.

Tre colori per ottenere tutti i milioni di colori che un computer può mostrare.

**ciascuna tessera** che compone il mosaico bogna **dosare** la quantità di ciascuno dei colori primari. Colore di ni tessera sarà descritto da una certa quantità di rosso, da una certa quantità di verde e da una certa quantità di blu.

Ciascun canale (rosso, verde e blu) viene codificato con **1 byte**, che può assumere un valore da 0 a 255 (esattamente come abbiamo visto nell'articolo sulla [codifica dnumeri]({{site.baseurl}}/codifica/la-codifica-dei-numeri.html)). Il valore 0 indica l'assenza di quello colore, il valore 255 indica la massima intensità.

| canale | valori | significato |
|---|---|---|
| Red | 0 255 | quantità di rosso |
| Green | 0  255 | quantità di verde |
| Blue | 0  255 | quantità di blu |

Combinando i256 valori possibili per ascuno dtre canali si ottengengono **256 × 256 × 256 = 16.777.216 colori didii**, detti anche colori a **24 bit** (8 bit per canale × 3 canali = 24 bit totali per pixel).

#### Esempio: il colore di una tessera

Immaginiamo che una tessera abia colore **rosso scuro**. In termini RGB questo colore potrebbe corrispondere a:

| canale | valore |
|---|---|
| Red | 165 |
| Green | 42 |
| Blue | 42 |

Il computer memria questo colore con **3 byte**: `10100101 00101010 00101010`, uper ascun canale.

![RGB bytes color](/images/codifica/RGB-bytes-color.jpg)

Il computer utilizza un byte (composto da 8 bit) per ascuna tessera del mosaico, che da ora in avanti chiameremo **pixel**.

#### Quanta memoria occre per una immagine?

Supponiamo di voler memorizare una'agine di 1024 x ppixel a colori RGB. Quta memriaria occorre?

Risposta: 1024 x 768 x = 2.359.296 bytees (circa **2,3 MB**)

Pixel infatti richiede 3 byte (uno per canale) e l'immagine è composposda 1024 x = 786.432 pixel.

Fotografia scattata conno smartphone o scaricata da internet ha solitamente una risozione più elevata (es. 4000 x 000 pixel): questo significifica che occupa **4000 x 000 x = 36.000.000 byte es**, circa **34 MB**, solo per matrRGB non compressa.

### Altri tipi di codifica

Esistono codifiche dise dalla RGB.

Un tempo i computer erano in grrado di mostrare soltanolori bbianco e nero, questo caso un bbit era più più che sufficiente per codificare un pixel d'immagine.

In questo caso quindi 1 pel = 1 bit.

![Panda](/images/codifica/panda.jpg)

### Scala di grigi

![Scala di grigi](/images/codifica/scalagrigi.png)

Iste anche la possibilità di memorizizare immmmagini a scala di grigi. questo caso per ascun pixel è necessario un byte di memoria, esattamente come nla codifica RGB a colori: cambia solil fatto che c'è un solo canale (l'intensità di grigio) invece di tre.

0 indica un pixel completamente nero.

255 indica un pixel completletamente bianco.

I valori nel mezzo indicano le possibibvariioni di grigio disponibili.

![Mela](/images/codifica/mela.jpg)

### Perché le immagini non occupano sempre lo stesso spazio

Abbiamo visto che una immagine RGB non compressa può occupare decine di MB di memoia. Le fotografie che scattiamo con lo smartphone o scarichiamo da internet, però, sono solitamente molto più leggere. Questo perpernon nonono memono memilarrizzate come **matrice RGB non compressa** formati che **comprimono** l'informazione, sfruttando somiglianze e ripetizioni all'interno d'immagine per rispariiare spazio, esattamente come accennato ananche narticicolo sula [codifica video]({{site.baseurl}}/codifica/la-codifica-dei-video.html).

Alcuni formati più comuni:

- **JPEG**: molto usato per fotografie, riduce lspazio occupato **eliminando parte d'informazione** che l'occhio umano fatica a percepire (compressione *lossy*, cioè con perdita di qualità);
- - **PNG**: adatto per immagini con testo, loghi o aree di colore unime, comime **senza perdere informformazione** (compressione *lossless*), i file tono a essere più pesanti del JPEG a pararità di contenuto fotografico;
- - **GIF**: utilizza una **tavolozza limitata** di colori (al più 256), per questo è adissimo per disdisni con pochi colori, e supsupananche semplici animimazni.

La scelta del formato è quindi un **compromesso tra qualità d'immagine e spazio occupato**.

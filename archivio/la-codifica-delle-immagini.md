---
id: 549
title: 'La codifica delle immagini'
date: '2020-10-06T23:32:06+02:00'
author: fabio
layout: page
guid: 'https://www.esercizidiinformatica.it/?page_id=549'
---

<div class="wp-block-columns"><div class="wp-block-column">Una immagine, dal punto di vista di un computer è come un **mosaico**, formato da una moltitudine di tessere, ogni tessera costituita da un solo colore.

Guardando il mosaico da lontano si perde visione delle singole tessere e si vede l’immagine come un **disegno continuo**.

</div><div class="wp-block-column"><figure class="wp-block-image size-large">![](https://www.esercizidiinformatica.it/wp-content/uploads/2020/10/mosaico-1024x1024.jpg)</figure></div></div><div class="wp-block-columns"><div class="wp-block-column"><figure class="wp-block-image size-large">![](https://www.esercizidiinformatica.it/wp-content/uploads/2020/10/rgb.png)</figure></div><div class="wp-block-column">Il computer ottiene qualsiasi colore come somma dei tre colori primari: **Red**, **Green** e **Blue**.

Tre colori per ottenere tutti i milioni di colori che un computer può mostrare.

</div></div>Per **ciascuna tessera** che compone il mosaico bisogna **dosare** la quantità di ciascuno dei colori primari. Il colore di ogni tessera sarà descritto da una certa quantità di rosso, da una certa quantità di verde e da una certa quantità di blù.

<figure class="wp-block-image size-large">![](https://www.esercizidiinformatica.it/wp-content/uploads/2020/10/RGB-bytes-color.jpg)</figure>Il computer utilizza un byte (composto da 8 bit) per ciascuna tessera del mosaico, che da ora in avanti chiameremo pixel.

#### Quanta memoria occorre per una immagine?

Supponiamo di voler memorizzare una immagine di 1024 x 768 pixel a colori RGB. Quanta memoria occorre?

Risposta: 1024 x 768 x 3 = 2359296 bytes

### Altri tipi di codifica

Esistono codifiche diverse dalla RGB.

<div class="wp-block-columns"><div class="wp-block-column">Un tempo i computer erano in grado di mostrare soltanto colori in bianco e nero, in questo caso un bit era più che sufficiente per codificare un pixel dell’immagine.

In questo caso quindi 1 pixel = 1 bit

</div><div class="wp-block-column"><figure class="wp-block-image size-large">![](https://www.esercizidiinformatica.it/wp-content/uploads/2020/10/panda.jpg)</figure></div></div>### Scala di grigi

<figure class="wp-block-image size-large">![](https://www.esercizidiinformatica.it/wp-content/uploads/2020/10/scalagrigi.png)</figure>Esiste anche la possibilità di memorizzare immagini a scala di grigi. In questo caso per ciascun pixel è necessario un byte di memoria. Questo perché bisogna dosare la quantità di grigio.

0 indica un pixel è completamente nero

255 indica un pixel completamente bianco

I valori nel mezzo indicano le possibilit variazioni di grigio disponibili

<figure class="wp-block-image size-large">![](https://www.esercizidiinformatica.it/wp-content/uploads/2020/10/mela-1024x656.jpeg)</figure>
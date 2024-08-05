---
id: 549
title: 'La codifica delle immagini'
date: '2020-10-06T23:32:06+02:00'
author: Fabio Mattei
layout: page
guid: 'https://www.esercizidiinformatica.it/?page_id=549'
---

Una immagine, dal punto di vista di un computer è come un **mosaico**, 
formato da una moltitudine di tessere, ogni tessera costituita da un solo colore.

Guardando il mosaico da lontano si perde visione delle singole tessere e si vede l’immagine come un **disegno continuo**.


![Mosaica](/images/codifica/mosaico.jpg)

![RGB](/images/codifica/rgb.png)


Il computer ottiene qualsiasi colore come somma dei tre colori primari: **Red**, **Green** e **Blue**.

Tre colori per ottenere tutti i milioni di colori che un computer può mostrare.

Per **ciascuna tessera** che compone il mosaico bisogna **dosare** la quantità di ciascuno dei colori primari. 
Il colore di ogni tessera sarà descritto da una certa quantità di rosso, da una certa quantità di verde e da 
una certa quantità di blù.

![RGB bytes color](/images/codifica/RGB-bytes-color.jpg)

Il computer utilizza un byte (composto da 8 bit) per ciascuna tessera del mosaico, che da ora in avanti chiameremo pixel.

#### Quanta memoria occorre per una immagine?

Supponiamo di voler memorizzare una immagine di 1024 x 768 pixel a colori RGB. Quanta memoria occorre?

Risposta: 1024 x 768 x 3 = 2359296 bytes

### Altri tipi di codifica

Esistono codifiche diverse dalla RGB.

Un tempo i computer erano in grado di mostrare soltanto colori in bianco e nero, in questo caso un bit era più che sufficiente per codificare un pixel dell’immagine.

In questo caso quindi 1 pixel = 1 bit

![Panda](/images/codifica/panda.jpg)

### Scala di grigi

![Scala di grigi](/images/codifica/scalagrigi.png)

Esiste anche la possibilità di memorizzare immagini a scala di grigi. In questo caso per ciascun pixel è necessario 
un byte di memoria. Questo perché bisogna dosare la quantità di grigio.

0 indica un pixel è completamente nero

255 indica un pixel completamente bianco

I valori nel mezzo indicano le possibili variazioni di grigio disponibili

![Mela](/images/codifica/mela.jpg)


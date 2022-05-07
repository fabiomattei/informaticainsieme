---
id: 134
title: 'La tartaruga'
date: '2020-02-04T15:55:20+01:00'
author: fabio
layout: page
guid: 'https://www.esercizidiinformatica.it/?page_id=134'
---

###### Iniziamo tracciando una linea

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">from turtle import Turtle, Screen

tartaruga = Turtle()
sfondo = Screen()
tartaruga.forward(100)
```

</div>La prima righa di codice permette a Python di **estendere le sue conoscenze** caricando la **libreria Turtle**. Le righe 3 e 4 attivano la libreria, la riga 5 disegna una linea orizzontale.

###### Aggiungiamo una curva

Aggiungi le seguenti due righe allo script precedente

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">tartaruga.right(90)
tartaruga.forward(100)
```

</div>Noterai che la tartaruga ha creato un angolo e aggiunto un secondo lato al quadrato. Questo perché il comando right fa girare la tartaruga a destra di 90 gradi.

## Esercizi

Svolgi i seguenti esercizi:

- Disegna un rettangolo: 2 dei 4 lati devono essere più lunghi.
- Disegna un triangolo: di quanti gradi deve essere la rotazione?
- Disegna una croce: andare avanti e indietro è una buona soluzione.
- Disegna un cerchio

## I comandi turtle

Facciamo una lista di tutti i comandi turtle

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">tartaruga.forward(100)     # va avanti di 100 pixel
tartaruga.backward(100)    # va indietro di 100 pixel
tartaruga.right(90)        # gira a destra di 90 gradi
tartaruga.left(90)         # gira a siinistra di 90 gradi
tartaruga.penup()          # solleva la penna e smette di tracciare
tartaruga.pendown()        # abbassa la penna e inizia a tracciare
tartaruga.color("red")     # cambia colore linea tracciata
tartaruga.pensize(5)       # cambia spessore linea tracciata
tartaruga.setpos(60,30)    # cambia la posizione della tartaruga
tartaruga.shape("turtle")  # calbia la forma della tartaruga
```

</div>Le possibile forme per la tartaruga sono:

- square
- arrow
- circle
- turtle
- triangle
- classic

## Terza lezione

Un esempio di come si utilizza la libreria Turtle

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">import turtle                   # attiva la libreria turtle
#---------------------
turtle.shape('turtle')          # inizializza la forma del cursore
turtle.pensize(5)               # inizializza lo spessore della traccia
turtle.pencolor('red')          # inizializza il colore della traccia 
turtle.speed(0)                 # inizializza la velocità di movimento
#---------------------
for j in range(10):
   turtle.forward(80)           # si muove in avanti di 80 pixel
   turtle.right(36)             # ruota verso destra di 36 gradi
turtle.done()                   # comunica alla libreria il fine programma
```

</div>La tartaruga all’inizio si trova al centro dello schermo (posizione 0, 0) inclinata verso est (destra). Attraverso i comandi forward(int x), backward(int x), left(int angle) e right(int angle), facciamo muovere la tartaruga. Nota come è stato utilizzato un ciclo per evitare di ripetere tediosi comandi. Il comando goToPoint(int x, int y) indica alla tartaruga di andare nel punto di coordinate x, y.  
Quando si muove la tartaruga lascia una traccia, questa funzione ci permette di creare dei disegni. Se vogliamo far interrompere la traccia diamo il comando penUP(), se vogliamo riattivare la traccia diamo il comando penDown().

Esercizio 1: scrivi un programma per far disegnare alla tartaruga un triangolo equilatero

Esercizio 2: scrivi un programma per far disegnare alla tartaruga un triangolo rettangolo

Esercizio 3: scrivi un programma per far disegnare alla tartaruga una scala colorata

Esercizio 4: scrivi un programma per far disegnare alla tartaruga il campo di gioco di Tic Tac Toe

Esercizio 5: scrivi un programma per far disegnare alla tartaruga un triangolo cerchio

Esercizio 6: Disegna un casetta utilizzando turtle

<figure class="wp-block-image">![](https://lh5.googleusercontent.com/de8sblVERxaTXSydejDQcOe77D95ukwYTnScucdfbO8ySZq2dvlhv5TyqlH-yR0tVQnKRGzDQgiAxsCBn_v5Rp5eqzZ7jtR0ZSxmmgXIyqlJzw1xqIR8cY85UWOO5AsYaIWLT2iQ)</figure>Esercizio 7:

Colora il disegno fatto in precedenza

<figure class="wp-block-image">![](https://lh6.googleusercontent.com/E9Zal8RV8wiIM4e0SFpzoR6AYSjgL7-Y4XYkRGD8yAk0TOvGfLsza2b6hPf3yi0lPwTIDX9obGBOfSIcBbza_PH4YLMbuRHuwBFMLxvBTTdK5-Gdhj2pZB74wrbKXKj5SH_1KX-F)</figure>Esercizio 8:

Utilizzando turtle disegna un villaggio

<figure class="wp-block-image">![](https://lh5.googleusercontent.com/Uzczu8uG6GzNMeewOnONcx69OK03Z_KvCy8XSxIOSPAA92oCydE9bZhIm10HROk5QyEq8JwIgWpC2LoigLGIVwOAi9lHHxjAFmGXL0GNNxIDEN9o3U0Pzovib3ayuXHTIcyedVAa)</figure>Esercizio 9:

<figure class="wp-block-image size-full">![](https://www.esercizidiinformatica.it/wp-content/uploads/2021/12/panda.png)</figure>Esercizio 10:

<figure class="wp-block-image size-full">![](https://www.esercizidiinformatica.it/wp-content/uploads/2021/12/alveare.png)</figure>Esercizio 11:

<figure class="wp-block-image size-full">![](https://www.esercizidiinformatica.it/wp-content/uploads/2021/12/fiore.png)</figure>Esercizio 12:

<figure class="wp-block-image size-full">![](https://www.esercizidiinformatica.it/wp-content/uploads/2021/12/pupazzoneve.png)</figure>Esercizio 13:

<figure class="wp-block-image size-full">![](https://www.esercizidiinformatica.it/wp-content/uploads/2021/12/cerchi.png)</figure>Esercizio 14:

<figure class="wp-block-image size-full">![](https://www.esercizidiinformatica.it/wp-content/uploads/2021/12/figuraregolare.png)</figure>Esercizio 15:

<figure class="wp-block-image size-full">![](https://www.esercizidiinformatica.it/wp-content/uploads/2021/12/figuraregolarecolorata.png)</figure>
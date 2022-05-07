---
id: 1025
title: OCR
date: '2022-04-21T10:41:32+02:00'
author: fabio
layout: revision
guid: 'https://www.esercizidiinformatica.it/?p=1025'
permalink: '/?p=1025'
---

Le reti neurali sono ottime nel risolvere problemi difficilmente schematizzabili con la logica. Uno di questi problemi è ad esempio il riconoscimento automatico dei caratteri.

Al fine di trattare questo argomento faremo uso del database MNIST (<http://yann.lecun.com/exdb/mnist/>) un database che ha scopo didattico contenente un set di caratteri per l’apprendimento composto da 60000 simboli ed un set di controllo di 10000 simboli.

<figure class="wp-block-image size-full">![](https://www.esercizidiinformatica.it/wp-content/uploads/2022/04/Sample-images-of-MNIST-data.png)</figure>Il nostro scopo è quello di costruire una rete in grado di riconoscere i caratteri. Questa rete verrà addestrata utilizzando il set per l’apprendimento. Il secondo passo consiste nel controllare la qualità dell’apprendimento attraverso il set di controllo.

Ciascun carattere è descritto da una matrice di 28×28 pixel ognuno dei quali descritto da 8 bit in scala di grigio per un totale di 256 tonalità. Ciascun pixel è descritto da un numero (compreso tra 0 e 255) e costituisce un input per la nostra rete neurale. I caratteri del set di addestramento sono contrassegnati dunque in questo caso si parla di apprendimento supervisionato.

<figure class="wp-block-image size-full is-resized">![](https://www.esercizidiinformatica.it/wp-content/uploads/2022/04/tensorflow-neural-network-schema-tensorflow-mnist-tutorial-italiano-esempio-guida-tensorflow-italia-tensorflow-classification-hello-world-single-digit.png)</figure>Dal punto di vista del codice vedremo che l’algoritmo è analogo ai precedenti.

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">import numpy as np

def sigmoide(z):
    return 1 / (1 + np.exp(-z))


def forward(X, w):
    somma_pesata = np.matmul(X, w)
    return sigmoide(somma_pesata)


def classifica(X, w):
    return np.round(forward(X, w))


def costo(X, Y, w):
    y_cappello = forward(X, w)
    primo_termine = Y * np.log(y_cappello)
    secondo_termine = (1 - Y) * np.log(1 - y_cappello)
    return -np.average(primo_termine + secondo_termine)


def gradiente(X, Y, w):
    return np.matmul(X.T, (forward(X, w) - Y)) / X.shape[0]


def allena(X, Y, iterations, lr):
    w = np.zeros((X.shape[1], 1))
    for i in range(iterations):
        # print("Iterazionie %4d => Costo: %.20f" % (i, costo(X, Y, w)))
        w -= gradiente(X, Y, w) * lr
    return w


def test(X, Y, w, digit):
    totale_campioni = X.shape[0]
    risultati_corretti = np.sum(classifica(X, w) == Y)
    percentuale_successo = risultati_corretti * 100 / totale_campioni
    print("Correct classifications for digit %d: %d/%d (%.2f%%)" %
          (digit, risultati_corretti, totale_campioni, percentuale_successo))


import mnist as data
for digit in range(10):
    w = allena(data.X_train, data.Y_train[digit], iterations=100, lr=1e-5)
    test(data.X_test, data.Y_test[digit], w, digit)


```

</div>
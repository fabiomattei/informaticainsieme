---
id: 945
title: 'Regressione lineare con termine noto'
date: '2022-02-09T14:48:07+01:00'
author: fabio
layout: page
guid: 'https://www.esercizidiinformatica.it/?page_id=945'
---

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">"""
Prenotazioni  Pizze
13            33
2             16
14            32
23            51
13            27
1             16
18            34
10            17
26            29
3             15
3             15
21            32
7             22
22            37
2             13
27            44
6             16
10            21
18            37
15            30
9             26
26            34
8             23
15            39
10            27
21            37
5             17
6             18
13            25
13            23
"""



prenotazioni_x = [13,2 ,14,23,13,1 ,18,10,26,3 ,3 ,21,7 ,22,2 ,27,6 ,10,18,15,9 ,26,8 ,15,10,21,5 ,6 ,13,13]
pizze_y = [33,16,32,51,27,16,34,17,29,15,15,32,22,37,13,44,16,21,37,30,26,34,23,39,27,37,17,18,25,23]

# la funzione predici consiste nell'applicazione
# di una legge lineare
def predici(x, w, b):
    return x * w + b

# Errore quadratico medio o devizione standard
def costo(X, Y, w, b):
    i = 0
    somma = 0
    for i in range(len(X)):
        somma = somma + (predici(X[i], w, b) - Y[i]) ** 2
    return somma / len(X)
    
# la funzione allena calcola il costo di una retta
# con un coefficiente angolare lievemente modificato
# per vedere se modificandolo il costo sale o scende
def allena(X, Y, num_iterazioni, lr):
    w = b = 0
    for i in range(num_iterazioni):
        costo_corrente = costo(X, Y, w, b)
        print("iterazione", i, "costo", costo_corrente)
        if costo(X, Y, w + lr, b) < costo_corrente:
            w = w + lr
        elif costo(X, Y, w - lr, b) < costo_corrente:
            w = w - lr
        if costo(X, Y, w, b + lr) < costo_corrente:
            b = b + lr
        elif costo(X, Y, w, b - lr) < costo_corrente:
            b = b - lr         
        else:
            return w, b
    return w, b


w, b = allena(prenotazioni_x, pizze_y, 10000, 0.01)

print("Peso ottenuto:", w, "Bias ottenuto", b))

prenotazioni = int(input("Scrivi il numero di prenotazioni di questa sera: "))
pizze = predici(prenotazioni, w, b)

print("Predico che venderai", pizze, "stasera")
```

</div>Ora modifichiamo l’algortmo in modo da tener conto del temine noto. Aggiungere il termine noto alla funzione lineare permette di ottenere una retta che simula molto meglio il fenomeno.

La funzione predici viene modificata in modo da presentare la legge lineare completa.

La funzione di costo, viene modificata in modo da tenere conto del termine noto.

Quella che viene modificata maggiormente è la funzione allena che a questo punto deve calcolare non più soltanto il coefficiente angolare ma anche il termine noto della retta.

b e w sono gli **iperparametri** del modello che stiamo costruendo.

Come spesso accade il programma si semplifica molto utilizzando una liibreria. Numpy è una librera sviluppata per fare calcolo e attraverso questa il codice si snellisce molto e aumenta molto in prestazioni

<div class="wp-block-simple-code-block-ace" style="height: 250px; position:relative; margin-bottom: 50px;">```
<pre class="wp-block-simple-code-block-ace" data-copy="false" data-fontsize="14" data-lines="Infinity" data-mode="python" data-showlines="true" data-theme="monokai" style="position:absolute;top:0;right:0;bottom:0;left:0">import numpy as np


def predici(X, w, b):
    return X * w + b


def costo(X, Y, w, b):
    return np.average((predici(X, w, b) - Y) ** 2)


def allena(X, Y, num_iterazioni, lr):
    w = b = 0
    for i in range(num_iterazioni):
        costo_corrente = costo(X, Y, w, b)
        print("Iteration %4d => Loss: %.6f" % (i, costo_corrente))

        if costo(X, Y, w + lr, b) < costo_corrente:
            w += lr
        elif costo(X, Y, w - lr, b) < costo_corrente:
            w -= lr
        elif costo(X, Y, w, b + lr) < costo_corrente:
            b += lr
        elif costo(X, Y, w, b - lr) < costo_corrente:
            b -= lr
        else:
            return w, b

    raise Exception("Non sono riuscito a convergere in %d iterazioni" % num_iterazioni)


# Inizializza i dati
X = np.array([13,2 ,14,23,13,1 ,18,10,26,3 ,3 ,21,7 ,22,2 ,27,6 ,10,18,15,9 ,26,8 ,15,10,21,5 ,6 ,13,13])
Y = np.array([33,16,32,51,27,16,34,17,29,15,15,32,22,37,13,44,16,21,37,30,26,34,23,39,27,37,17,18,25,23])

# Allena il modello
w, b = allena(X, Y, iterations=10000, lr=0.01)
print("\nw=%.3f, b=%.3f" % (w, b))

# Predici il numero di pizze
print("Predizione: x=%d => y=%.2f" % (20, predici(20, w, b)))

# Disegna il grafico
import matplotlib.pyplot as plt
import seaborn as sns
sns.set()
plt.plot(X, Y, "bo")
plt.xticks(fontsize=15)
plt.yticks(fontsize=15)
plt.xlabel("Prenotazioni", fontsize=30)
plt.ylabel("Pizze", fontsize=30)
x_edge, y_edge = 50, 50
plt.axis([0, x_edge, 0, y_edge])
plt.plot([0, x_edge], [b, predici(x_edge, w, b)], linewidth=1.0, color="g")
plt.show()


```

</div><figure class="wp-block-image size-large">![](https://www.esercizidiinformatica.it/wp-content/uploads/2022/02/Schermata-2022-02-10-alle-05.43.33-1024x843.png)</figure>
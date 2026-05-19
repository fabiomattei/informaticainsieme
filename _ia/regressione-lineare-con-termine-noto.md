---
title: 'Regressione lineare con termine noto'
date: '2022-02-09T14:48:07+01:00'
author: Fabio Mattei
layout: page
---

Nella pagina precedente il modello era **y = x · w**: una retta che passa obbligatoriamente per l'origine. Questo significa che quando x = 0 il modello predice sempre y = 0. Nel nostro problema, però, uno studente potrebbe prendere un voto minimo anche senza studiare, quindi forzare la retta a passare per l'origine introduce un errore sistematico.

La soluzione è aggiungere il **termine noto** (detto anche *bias*) b:

**y = x · w + b**

Il parametro b sposta la retta in su o in giù lungo l'asse Y senza modificarne la pendenza. In questo modo la retta non è più vincolata all'origine e può adattarsi meglio ai dati.

L'algoritmo deve ora trovare due parametri invece di uno: w (pendenza) e b (intercetta). La funzione di costo e la funzione di allena vengono modificate di conseguenza.

{% highlight python %}
"""
OreDiStudio  Voto
2             5
8             8
5             7
9             9
1             4
7             8
3             5
6             7
4             6
10            9
2             4
8             8
6             7
3             5
9             9
1             3
5             6
7             7
4             6
8             8
3             5
6             7
9             9
2             4
7             8
4             6
10           10
5             6
1             4
6             7
"""

studio_x = [ 2, 8, 5, 9, 1, 7, 3, 6, 4,10, 2, 8, 6, 3, 9, 1, 5, 7, 4, 8, 3, 6, 9, 2, 7, 4,10, 5, 1, 6]
voto_y   = [ 5, 8, 7, 9, 4, 8, 5, 7, 6, 9, 4, 8, 7, 5, 9, 3, 6, 7, 6, 8, 5, 7, 9, 4, 8, 6,10, 6, 4, 7]

# la funzione predici consiste nell'applicazione
# di una legge lineare
def predici(x, w, b):
    return x * w + b

# Errore quadratico medio o deviazione standard
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


w, b = allena(studio_x, voto_y, 10000, 0.01)

print("Peso ottenuto:", w, "Bias ottenuto:", b)

studio = int(input("Scrivi le ore di studio di questa settimana: "))
voto = predici(studio, w, b)

print("Predico che prenderai il voto:", voto)
{% endhighlight %}

Ora modifichiamo l’algoritmo in modo da tener conto del termine noto. Aggiungere il termine noto alla funzione lineare permette di ottenere una retta che simula molto meglio il fenomeno.

La funzione predici viene modificata in modo da presentare la legge lineare completa.

La funzione di costo, viene modificata in modo da tenere conto del termine noto.

Quella che viene modificata maggiormente è la funzione allena che a questo punto deve calcolare non più soltanto il coefficiente angolare ma anche il termine noto della retta.

b e w sono gli **iperparametri** del modello che stiamo costruendo.

Come spesso accade il programma si semplifica molto utilizzando una libreria. Numpy è una libreria sviluppata per fare calcolo e attraverso questa il codice si snellisce molto e aumenta molto in prestazioni.

{% highlight python %}
import numpy as np


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
X = np.array([ 2, 8, 5, 9, 1, 7, 3, 6, 4,10, 2, 8, 6, 3, 9, 1, 5, 7, 4, 8, 3, 6, 9, 2, 7, 4,10, 5, 1, 6])
Y = np.array([ 5, 8, 7, 9, 4, 8, 5, 7, 6, 9, 4, 8, 7, 5, 9, 3, 6, 7, 6, 8, 5, 7, 9, 4, 8, 6,10, 6, 4, 7])

# Allena il modello
w, b = allena(X, Y, num_iterazioni=10000, lr=0.01)
print("\nw=%.3f, b=%.3f" % (w, b))

# Predici il voto
print("Predizione: x=%d ore => voto=%.2f" % (6, predici(6, w, b)))

# Disegna il grafico
import matplotlib.pyplot as plt
import seaborn as sns
sns.set()
plt.plot(X, Y, "bo")
plt.xticks(fontsize=15)
plt.yticks(fontsize=15)
plt.xlabel("Ore di studio", fontsize=30)
plt.ylabel("Voto", fontsize=30)
x_edge, y_edge = 12, 12
plt.axis([0, x_edge, 0, y_edge])
plt.plot([0, x_edge], [b, predici(x_edge, w, b)], linewidth=1.0, color="g")
plt.show()


{% endhighlight %}

![Regressione lineare](/images/python/ia/regressione-lineare-terminenoto.png){:class="aside-image"}

### Esercizi

1. Esegui il modello e annota il costo finale. Poi torna alla pagina precedente (senza termine noto) ed esegui quel modello sugli stessi dati: qual è il costo finale? Quale dei due modella meglio i dati?
2. Usa il modello per predire il voto con 0 ore di studio: `predici(0, w, b)`. Il risultato è diverso da 0? È un valore realistico per un esame?
3. Modifica la funzione `allena` per restituire anche il costo finale. Confronta il costo finale con lr=0.01 e lr=0.001.
4. Aggiorna il grafico per disegnare anche la retta del modello precedente (senza b, passante per l'origine) in rosso, e quella del modello corrente in verde. Quale delle due si adatta meglio ai punti?


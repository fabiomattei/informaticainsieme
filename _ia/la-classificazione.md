---
id: 998
title: 'La classificazione'
date: '2022-03-16T23:47:41+01:00'
author: Fabio Mattei
layout: page
guid: 'https://www.esercizidiinformatica.it/?page_id=998'
---

**Un problema di classificazione** consiste nell’individuazione di una regola che assegni univocamente ogni osservazione ad una particolare categoria, sulla base di un insieme di dati per i quali è nota a priori la classe di appartenenza di ciascuno.

Per approcciare questo problema introduciamo la funzione sigmoide:

<figure class="wp-block-image size-full">![](https://www.esercizidiinformatica.it/wp-content/uploads/2022/03/sigmoide.png)<figcaption>la sigmoide</figcaption></figure>La funzione sigmoide ha come dominio l’intero R e come codominio l’intervallo che va da 0 a 1. Tende a 0 per x tendente a meno infinito e a 1 per x tendente a più infinito.

Questa funzione ci è comoda perchè ci permette di mettere in relazione valori appartenenti ad un insieme continuo a valori booleani 0, 1. In pratica ci permette di passare dal continuo al binario. Si presta dunque a rispondere alla domanda fondamentale del problema della classificazione: il campione x appartiene all’insieme?

{% highlight python %}
# Un classificatore binario

import numpy as np

# funzione sigmoide codificata in python
def sigmoide(z):
    return 1 / (1 + np.exp(-z))

# la funzione che si occupa di fare la predizione
def predici(X, w):
    somma_pesata = np.matmul(X, w)
    return sigmoide(somma_pesata)

# la classificazione consiste in un arrotondamento
# se il valore restituito dalla funzione predici
# e' maggiore di 0.5 la funzione classifica restituisce vero
# altrimenti restituisce falso
def classifica(X, w):
    return np.round(predici(X, w))

# costo logaritmico (preso in prestito dagli statistici)
def costo(X, Y, w):
    y_hat = predici(X, w)
    primo_termine = Y * np.log(y_hat)
    secondo_terme = (1 - Y) * np.log(1 - y_hat)
    return -np.average(primo_termine + secondo_terme)


def gradiente(X, Y, w):
    return np.matmul(X.T, (predici(X, w) - Y)) / X.shape[0]


def allena(X, Y, numero_iterazioni, lr):
    w = np.zeros((X.shape[1], 1))
    for i in range(numero_iterazioni):
        print("Iterazione %4d => Loss: %.20f" % (i, costo(X, Y, w)))
        w -= gradiente(X, Y, w) * lr
    return w


def test(X, Y, w):
    campioni_totali = X.shape[0]
    risultati_corretti = np.sum(classifica(X, w) == Y)
    percentuale_successo = risultati_corretti * 100 / campioni_totali
    print("\nSuccesso: %d/%d (%.2f%%)" % (risultati_corretti, campioni_totali, percentuale_successo))


# Prepara dati
# Numero Prenotazioni
x1 = np.array([13, 2, 14, 23, 13, 1, 18, 10, 26, 3, 3, 21, 7, 22, 2, 27, 6, 10,
 18, 15, 9, 26, 8, 15, 10, 21, 5, 6, 13, 13])
# Temperatura
x2 = np.array([26, 14, 20, 25, 24, 13, 23, 18, 24, 14, 12, 27, 17, 21, 14, 26, 15, 21,
 18, 26, 20, 25, 21, 22, 20, 21, 12, 14, 19, 20,])
# Numero di turisti
x3 = np.array([ 9,  6,  3,  9,  8,  2,  9, 10,  3,  1,  3,  5,  3,  1,  4,  2,  4,  7,
  3,  8,  6,  9, 10,  7,  2,  1,  7,  9,  4,  3,])
# La polizia e' stata chiamata?
y = np.array([1, 0, 1, 1, 1, 0, 1, 1, 1, 0, 0, 1, 0, 1, 0, 1, 0, 0, 0, 1, 0, 1, 0, 1,
 0, 1, 0, 0, 1, 0,])

X = np.column_stack((np.ones(x1.size), x1, x2, x3))
Y = y.reshape(-1, 1)
w = allena(X, Y, numero_iterazioni=10000, lr=0.001)

print('pesi calcolati', w)

# Vediamo quanto e' efficace
test(X, Y, w)
{% endhighlight %}

</div>
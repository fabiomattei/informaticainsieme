---
title: 'La classificazione'
date: '2022-03-16T23:47:41+01:00'
author: Fabio Mattei
layout: page
---

**Un problema di classificazione** consiste nell’individuazione di una regola che assegni univocamente ogni osservazione ad una particolare categoria, sulla base di un insieme di dati per i quali è nota a priori la classe di appartenenza di ciascuno.

Per approcciare questo problema introduciamo la funzione sigmoide:

![Sigmoide](/images/python/ia/sigmoide.png){:class="aside-image"}

La funzione sigmoide ha come dominio l’intero R e come codominio l’intervallo che va da 0 a 1. Tende a 0 per x tendente a meno infinito e a 1 per x tendente a più infinito. La sua formula è:

**σ(z) = 1 / (1 + e^{-z})**

Questa funzione ci è comoda perché ci permette di mettere in relazione valori appartenenti ad un insieme continuo a valori booleani 0, 1. In pratica ci permette di passare dal continuo al binario. Si presta dunque a rispondere alla domanda fondamentale del problema della classificazione: il campione x appartiene all’insieme?

#### La funzione di costo per la classificazione

Nella regressione lineare usavamo l’errore quadratico medio come funzione di costo. Per la classificazione questo non funziona: applicato alla sigmoide, l’errore quadratico medio produce una superficie di costo con molti minimi locali, e la discesa del gradiente si inceppa senza trovare la soluzione ottimale.

Si usa invece il **costo logaritmico** (o *cross-entropy*):

**C(y, ŷ) = -[ y · log(ŷ) + (1 - y) · log(1 - ŷ) ]**

L’intuizione è semplice: se l’etichetta reale y è 1 e il modello predice ŷ vicino a 0 (cioè sbaglia con sicurezza), log(ŷ) tende a meno infinito e il costo esplode. Se invece il modello predice ŷ vicino a 1 (cioè azzecca), log(ŷ) si avvicina a 0 e il costo è basso. Il comportamento è speculare quando y = 0. Questa funzione è convessa rispetto ai parametri w, il che garantisce che la discesa del gradiente trovi sempre il minimo globale.

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

# costo logaritmico (cross-entropy)
def costo(X, Y, w):
    y_hat = predici(X, w)
    primo_termine = Y * np.log(y_hat)
    secondo_termine = (1 - Y) * np.log(1 - y_hat)
    return -np.average(primo_termine + secondo_termine)


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
# Ore di studio
x1 = np.array([ 2, 8, 5, 9, 1, 7, 3, 6, 4,10, 2, 8, 6, 3, 9, 1, 5, 7, 4, 8, 3, 6, 9, 2, 7, 4,10, 5, 1, 6])
# Ore di sonno
x2 = np.array([ 7, 8, 6, 8, 5, 7, 6, 7, 6, 8, 5, 8, 7, 6, 9, 4, 7, 8, 6, 8, 5, 7, 9, 5, 8, 6, 9, 7, 4, 7])
# Argomenti studiati (su 10)
x3 = np.array([ 3, 8, 5, 9, 2, 7, 4, 6, 3,10, 2, 8, 6, 3, 9, 1, 5, 7, 4, 8, 3, 6, 9, 2, 7, 4,10, 5, 2, 6])
# Ha superato l'esame?
y = np.array([0, 1, 1, 1, 0, 1, 0, 1, 0, 1, 0, 1, 1, 0, 1, 0, 1, 1, 1, 1, 0, 1, 1, 0, 1, 0, 1, 1, 0, 1])

X = np.column_stack((np.ones(x1.size), x1, x2, x3))
Y = y.reshape(-1, 1)
w = allena(X, Y, numero_iterazioni=10000, lr=0.001)

print('pesi calcolati', w)

# Vediamo quanto e' efficace
test(X, Y, w)
{% endhighlight %}

### Esercizi

1. Modifica la funzione `classifica` per usare una soglia di 0.7 invece di 0.5 (sostituisci `np.round` con un confronto esplicito `predici(X, w) >= 0.7`). L'accuratezza aumenta o diminuisce? Cosa significa alzare la soglia dal punto di vista pratico?
2. Rimuovi la variabile `x3` (argomenti studiati) dal modello e riallena. L'accuratezza cambia significativamente? Quale variabile contribuisce di più alla classificazione?
3. Modifica la funzione `allena` per stampare l'accuratezza (chiamando `test`) ogni 2000 iterazioni. Osserva come l'accuratezza cresce durante l'addestramento.
4. Il dataset attuale ha etichetta 1 quando lo studente supera l'esame. Prova a predire il risultato di uno studente che studia 6 ore, dorme 7 ore e copre 5 argomenti: `predici(np.array([1, 6, 7, 5]), w)`. Il modello lo classifica come promosso?

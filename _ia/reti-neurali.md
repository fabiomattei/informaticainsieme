---
title: 'Reti neurali'
date: '2022-05-10T08:00:00+02:00'
author: Fabio Mattei
layout: page
---

## Dal percettrone alla rete neurale

Nella pagina sulla classificazione abbiamo costruito un **percettrone**: un singolo neurone artificiale che riceve degli ingressi, li moltiplica per i rispettivi pesi, somma tutto, aggiunge un termine noto e infine passa il risultato attraverso la funzione sigmoide per produrre un'uscita tra 0 e 1.

![Percettrone](/images/python/ia/percettrone.png){:class="aside-image"}

Il percettrone funziona bene quando il confine che separa le due classi è una retta (in 2D) o un piano (in più dimensioni). Ma molti problemi reali non sono linearmente separabili: nessuna retta riesce a dividere correttamente i dati.

La soluzione è collegare più percettroni insieme: l'uscita di un gruppo di neuroni diventa l'ingresso del gruppo successivo. Nasce così la **rete neurale artificiale**.

## L'architettura a strati

Una rete neurale è organizzata in **strati** (in inglese: *layers*):

- **Strato di ingresso** (*input layer*): riceve i dati grezzi. Ogni neurone corrisponde a una variabile di ingresso. Non esegue calcoli: trasmette i valori allo strato successivo.
- **Strati nascosti** (*hidden layers*): uno o più strati intermedi. Ogni neurone riceve tutti i valori dello strato precedente, calcola una somma pesata e applica la funzione di attivazione. Sono questi strati che imparano rappresentazioni interne dei dati.
- **Strato di uscita** (*output layer*): produce la predizione finale. Per un problema di classificazione binaria ha un solo neurone con uscita sigmoide.

```
Ingresso      Strato nascosto     Uscita

ore_studio ──┐
              ├── [n1] ──┐
ore_sonno  ──┤            ├── [n4] ──── promosso?
              ├── [n2] ──┤
argomenti  ──┤            │
              └── [n3] ──┘
```

I dati scorrono sempre da sinistra verso destra: questa operazione si chiama **propagazione in avanti** (*forward propagation*).

## La propagazione in avanti

Per ogni strato il calcolo è identico a quello del percettrone:

1. Si calcola la **somma pesata**: `Z = X · W + b`
2. Si applica la **funzione di attivazione**: `A = σ(Z)`

L'uscita `A` di uno strato diventa l'ingresso `X` dello strato successivo.

Con due strati (uno nascosto e uno di uscita):

```
Z1 = X  · W1 + b1      A1 = σ(Z1)
Z2 = A1 · W2 + b2      A2 = σ(Z2)
```

`A2` è la predizione finale della rete.

## L'apprendimento: la retropropagazione

Come nella regressione lineare e nella classificazione, la rete impara minimizzando una funzione di costo. Per la classificazione si usa ancora il **costo logaritmico** (cross-entropy).

La differenza è che ora i pesi da aggiornare appartengono a più strati. L'algoritmo che calcola il gradiente per ogni strato si chiama **retropropagazione** (*backpropagation*): partendo dall'errore all'uscita, risale strato per strato propagando il segnale di correzione verso l'ingresso.

Il principio matematico è la regola della catena del calcolo differenziale: l'errore di ogni strato dipende dall'errore dello strato successivo, moltiplicato per la derivata dell'attivazione.

## Il codice

Usiamo il dataset della classificazione: ore di studio, ore di sonno e argomenti coperti come ingressi; se lo studente ha superato l'esame come uscita.

{% highlight python %}
import numpy as np


def sigmoide(z):
    return 1 / (1 + np.exp(-z))


def sigmoide_derivata(z):
    s = sigmoide(z)
    return s * (1 - s)


def forward(X, W1, b1, W2, b2):
    Z1 = np.matmul(X, W1) + b1
    A1 = sigmoide(Z1)
    Z2 = np.matmul(A1, W2) + b2
    A2 = sigmoide(Z2)
    return Z1, A1, Z2, A2


def costo(A2, Y):
    return -np.average(Y * np.log(A2) + (1 - Y) * np.log(1 - A2))


def allena(X, Y, n_nascosti, num_iterazioni, lr):
    n_ingressi = X.shape[1]

    # Pesi inizializzati con valori piccoli casuali per rompere la simmetria
    W1 = np.random.randn(n_ingressi, n_nascosti) * 0.01
    b1 = np.zeros((1, n_nascosti))
    W2 = np.random.randn(n_nascosti, 1) * 0.01
    b2 = np.zeros((1, 1))

    for i in range(num_iterazioni):
        # Propagazione in avanti
        Z1, A1, Z2, A2 = forward(X, W1, b1, W2, b2)

        if i % 1000 == 0:
            print("Iterazione %4d => Costo: %.6f" % (i, costo(A2, Y)))

        # Retropropagazione
        m = X.shape[0]
        dZ2 = A2 - Y
        dW2 = np.matmul(A1.T, dZ2) / m
        db2 = np.average(dZ2, axis=0, keepdims=True)
        dZ1 = np.matmul(dZ2, W2.T) * sigmoide_derivata(Z1)
        dW1 = np.matmul(X.T, dZ1) / m
        db1 = np.average(dZ1, axis=0, keepdims=True)

        # Aggiornamento dei pesi
        W1 -= lr * dW1
        b1 -= lr * db1
        W2 -= lr * dW2
        b2 -= lr * db2

    return W1, b1, W2, b2


def classifica(X, W1, b1, W2, b2):
    _, _, _, A2 = forward(X, W1, b1, W2, b2)
    return np.round(A2)


def test(X, Y, W1, b1, W2, b2):
    predizioni = classifica(X, W1, b1, W2, b2)
    corretti = int(np.sum(predizioni == Y))
    totale = X.shape[0]
    print("Successo: %d/%d (%.2f%%)" % (corretti, totale, corretti * 100 / totale))


# Dati
x1 = np.array([ 2, 8, 5, 9, 1, 7, 3, 6, 4,10, 2, 8, 6, 3, 9, 1, 5, 7, 4, 8, 3, 6, 9, 2, 7, 4,10, 5, 1, 6])
x2 = np.array([ 7, 8, 6, 8, 5, 7, 6, 7, 6, 8, 5, 8, 7, 6, 9, 4, 7, 8, 6, 8, 5, 7, 9, 5, 8, 6, 9, 7, 4, 7])
x3 = np.array([ 3, 8, 5, 9, 2, 7, 4, 6, 3,10, 2, 8, 6, 3, 9, 1, 5, 7, 4, 8, 3, 6, 9, 2, 7, 4,10, 5, 2, 6])
y  = np.array([0, 1, 1, 1, 0, 1, 0, 1, 0, 1, 0, 1, 1, 0, 1, 0, 1, 1, 1, 1, 0, 1, 1, 0, 1, 0, 1, 1, 0, 1])

X = np.column_stack((x1, x2, x3))
Y = y.reshape(-1, 1)

np.random.seed(42)
W1, b1, W2, b2 = allena(X, Y, n_nascosti=4, num_iterazioni=10000, lr=0.1)

test(X, Y, W1, b1, W2, b2)
{% endhighlight %}

La funzione `allena` accetta un parametro `n_nascosti` che controlla quanti neuroni compongono lo strato nascosto. Più neuroni permettono alla rete di apprendere relazioni più complesse, ma aumentano anche il rischio di **sovradattamento** (*overfitting*): la rete memorizza i dati di addestramento invece di generalizzare.

Un dettaglio importante: i pesi vengono inizializzati con valori piccoli **casuali** anziché a zero. Se tutti i pesi fossero zero, tutti i neuroni dello strato nascosto calcolerebbero la stessa cosa e rimarrebbero identici durante tutto l'addestramento — il problema si chiama **simmetria dei pesi** e vanifica l'utilità degli strati nascosti.

### Esercizi

1. Esegui il codice con `n_nascosti=1` e poi con `n_nascosti=8`. L'accuratezza cambia? Qual è il valore minimo di neuroni nascosti che ottiene il 100% sul dataset di addestramento?
2. Prova a predire il risultato di uno studente che studia 6 ore, dorme 7 ore e ha coperto 5 argomenti. Crea il vettore `X_nuovo = np.array([[6, 7, 5]])` e usa `classifica(X_nuovo, W1, b1, W2, b2)`. Il modello lo classifica come promosso?
3. Aggiungi un secondo strato nascosto con 3 neuroni tra lo strato nascosto attuale e lo strato di uscita. Dovrai aggiungere `W3, b3` e modificare le funzioni `forward`, `allena`, `classifica` e `test` di conseguenza.
4. Rimuovi `np.random.seed(42)` ed esegui il programma più volte. L'accuratezza finale varia? Perché l'inizializzazione casuale dei pesi influenza il risultato?

---
title: 'Reti neurali profonde'
date: '2022-06-01T08:00:00+02:00'
author: Fabio Mattei
layout: page
---

## Perché aggiungere strati?

La rete della pagina precedente aveva un solo strato nascosto. Aggiungere strati permette alla rete di imparare caratteristiche sempre più astratte: il primo strato potrebbe individuare bordi e contrasti nei pixel, il secondo forme geometriche elementari, il terzo parti riconoscibili di un oggetto. Ogni strato costruisce su ciò che lo strato precedente ha imparato.

Reti con più di un strato nascosto si chiamano **reti neurali profonde** (*deep neural networks*). La parola "profonda" si riferisce proprio al numero di strati, non alla complessità dei singoli neuroni.

## Il problema della sigmoide in profondità

Quando si impilano molti strati con la funzione sigmoide emerge un problema grave: il **gradiente che svanisce** (*vanishing gradient*).

La derivata della sigmoide vale al massimo 0.25 (nel punto z = 0) e si avvicina a zero alle estremità. Durante la retropropagazione il gradiente viene moltiplicato per questa derivata ad ogni strato. Con 4 strati, il gradiente che arriva al primo strato può essere ridotto di un fattore 0.25⁴ ≈ 0.004 rispetto a quello dell'ultimo strato: i pesi dei primi strati smettono di aggiornarsi e la rete non impara.

## ReLU: una funzione di attivazione migliore

La soluzione più comune è la funzione **ReLU** (*Rectified Linear Unit*):

**ReLU(z) = max(0, z)**

ReLU restituisce z se z è positivo, 0 altrimenti. La sua derivata è semplicissima:

**ReLU'(z) = 1 se z > 0, altrimenti 0**

Poiché la derivata è 1 per i neuroni attivi, il gradiente non si riduce attraversando questi strati e può raggiungere i pesi dei primi livelli senza svanire. ReLU è oggi la funzione di attivazione predefinita per gli strati nascosti delle reti profonde.

Nella pratica si usa **ReLU per gli strati nascosti** e si riserva la sigmoide (o softmax) solo per lo strato di uscita.

## Softmax: classificare tra più di due classi

Nella classificazione binaria l'uscita è un singolo numero tra 0 e 1 interpretabile come probabilità. Per riconoscere 10 cifre (0–9) servono 10 neuroni di uscita, uno per cifra.

La funzione **softmax** converte un vettore di 10 valori arbitrari in 10 probabilità che sommano a 1:

**softmax(z)ₖ = e^{zₖ} / Σⱼ e^{zⱼ}**

Il neurone con il valore più alto riceve la probabilità maggiore. La cifra predetta è quella corrispondente al neurone con probabilità massima.

La funzione di costo diventa la **cross-entropy multi-classe**:

**C = − (1/m) Σᵢ Σₖ Yᵢₖ · log(Âᵢₖ)**

dove Yᵢₖ vale 1 solo se l'immagine i rappresenta la cifra k, e Âᵢₖ è la probabilità predetta dal modello.

## La codifica one-hot

Le etichette del dataset MNIST sono numeri interi (0, 1, 2, …, 9). Per usarle con la cross-entropy multi-classe dobbiamo convertirle in vettori di lunghezza 10 dove solo la posizione corrispondente alla cifra è 1 e tutte le altre sono 0:

```
cifra 3  →  [0, 0, 0, 1, 0, 0, 0, 0, 0, 0]
cifra 7  →  [0, 0, 0, 0, 0, 0, 0, 1, 0, 0]
```

Questa rappresentazione si chiama **codifica one-hot**.

## Il codice

Costruiamo una rete con due strati nascosti e applichiamola al riconoscimento delle cifre MNIST. L'architettura è:

```
Ingresso (784)  →  Nascosto 1 (128, ReLU)  →  Nascosto 2 (64, ReLU)  →  Uscita (10, softmax)
```

{% highlight python %}
import numpy as np
import mnist as data


def relu(z):
    return np.maximum(0, z)


def relu_der(z):
    return (z > 0).astype(float)


def softmax(z):
    # Sottrae il massimo per stabilità numerica (evita overflow con np.exp)
    e = np.exp(z - np.max(z, axis=1, keepdims=True))
    return e / np.sum(e, axis=1, keepdims=True)


def forward(X, W1, b1, W2, b2, W3, b3):
    Z1 = np.matmul(X, W1) + b1
    A1 = relu(Z1)
    Z2 = np.matmul(A1, W2) + b2
    A2 = relu(Z2)
    Z3 = np.matmul(A2, W3) + b3
    A3 = softmax(Z3)
    return Z1, A1, Z2, A2, A3


def costo(A3, Y):
    return -np.average(np.sum(Y * np.log(A3 + 1e-8), axis=1))


def allena(X, Y, num_iterazioni, lr):
    n0 = X.shape[1]          # 784 ingressi
    n1, n2, n3 = 128, 64, 10

    # Inizializzazione He: scala per sqrt(2 / neuroni_strato_precedente)
    # Riduce il rischio di gradienti che esplodono o svaniscono all'inizio
    np.random.seed(42)
    W1 = np.random.randn(n0, n1) * np.sqrt(2 / n0)
    b1 = np.zeros((1, n1))
    W2 = np.random.randn(n1, n2) * np.sqrt(2 / n1)
    b2 = np.zeros((1, n2))
    W3 = np.random.randn(n2, n3) * np.sqrt(2 / n2)
    b3 = np.zeros((1, n3))

    m = X.shape[0]
    for i in range(num_iterazioni):
        Z1, A1, Z2, A2, A3 = forward(X, W1, b1, W2, b2, W3, b3)

        if i % 10 == 0:
            print("Iterazione %3d => Costo: %.4f" % (i, costo(A3, Y)))

        # Retropropagazione
        dZ3 = A3 - Y                                              # (m, 10)
        dW3 = np.matmul(A2.T, dZ3) / m                           # (64, 10)
        db3 = np.sum(dZ3, axis=0, keepdims=True) / m             # (1, 10)

        dZ2 = np.matmul(dZ3, W3.T) * relu_der(Z2)               # (m, 64)
        dW2 = np.matmul(A1.T, dZ2) / m                           # (128, 64)
        db2 = np.sum(dZ2, axis=0, keepdims=True) / m             # (1, 64)

        dZ1 = np.matmul(dZ2, W2.T) * relu_der(Z1)               # (m, 128)
        dW1 = np.matmul(X.T, dZ1) / m                            # (784, 128)
        db1 = np.sum(dZ1, axis=0, keepdims=True) / m             # (1, 128)

        W1 -= lr * dW1;  b1 -= lr * db1
        W2 -= lr * dW2;  b2 -= lr * db2
        W3 -= lr * dW3;  b3 -= lr * db3

    return W1, b1, W2, b2, W3, b3


def predici_cifra(X, W1, b1, W2, b2, W3, b3):
    _, _, _, _, A3 = forward(X, W1, b1, W2, b2, W3, b3)
    return np.argmax(A3, axis=1)


def accuratezza(X, etichette, W1, b1, W2, b2, W3, b3):
    pred = predici_cifra(X, W1, b1, W2, b2, W3, b3)
    return np.average(pred == etichette) * 100


def one_hot(etichette, n_classi=10):
    m = np.zeros((len(etichette), n_classi))
    m[np.arange(len(etichette)), etichette] = 1
    return m


# Ricostruisce le etichette scalari (0-9) dal formato one-vs-all del modulo mnist
Y_train_matrice = np.column_stack([data.Y_train[d] for d in range(10)])
Y_test_matrice  = np.column_stack([data.Y_test[d]  for d in range(10)])
etichette_train = np.argmax(Y_train_matrice, axis=1)  # (60000,)
etichette_test  = np.argmax(Y_test_matrice,  axis=1)  # (10000,)

# Normalizza i pixel da [0, 255] a [0, 1]
X_train = data.X_train / 255.0
X_test  = data.X_test  / 255.0

# Converti le etichette in one-hot per la funzione di costo
Y_train = one_hot(etichette_train)  # (60000, 10)

W1, b1, W2, b2, W3, b3 = allena(X_train, Y_train, num_iterazioni=100, lr=0.5)

print("\nAccuratezza su addestramento: %.2f%%" % accuratezza(X_train, etichette_train, W1, b1, W2, b2, W3, b3))
print("Accuratezza su verifica:      %.2f%%" % accuratezza(X_test,  etichette_test,  W1, b1, W2, b2, W3, b3))
{% endhighlight %}

## Addestramento e verifica

Il codice calcola l'accuratezza su due insiemi separati:

- **Set di addestramento** (*training set*): i 60 000 esempi usati per aggiornare i pesi. Una rete abbastanza grande può memorizzare questi esempi e raggiungere un'accuratezza del 99%, ma questo non significa che abbia imparato qualcosa di generale.
- **Set di verifica** (*test set*): 10 000 esempi che la rete non ha mai visto. L'accuratezza qui misura quanto il modello generalizza a dati nuovi.

La differenza tra accuratezza di addestramento e accuratezza di verifica è il segnale principale per diagnosticare due problemi opposti:

| Situazione | Addestramento | Verifica | Causa probabile |
|---|---|---|---|
| Sottoadattamento | bassa | bassa | rete troppo piccola o poche iterazioni |
| Sovradattamento | alta | bassa | rete troppo grande o troppe iterazioni |
| Buon modello | alta | alta (simile) | architettura e training bilanciati |

Con 100 iterazioni e questa architettura si ottiene solitamente un'accuratezza di verifica intorno al 93–95%, già molto superiore al 90% circa ottenuto con la regressione logistica nella pagina OCR.

### Esercizi

1. Cambia il learning rate da 0.5 a 0.1 e poi a 1.0. Come cambia la velocità di convergenza e l'accuratezza finale?
2. Riduci il primo strato nascosto da 128 a 32 neuroni. L'accuratezza di verifica scende? Di quanto?
3. Aggiungi un terzo strato nascosto con 32 neuroni tra il secondo strato nascosto e lo strato di uscita. Dovrai aggiungere `W4, b4` e aggiornare le funzioni `forward`, `allena`, `predici_cifra` e `accuratezza`.
4. Dopo l'addestramento, trova le immagini del set di verifica che il modello classifica in modo errato: `errori = np.where(predici_cifra(X_test, ...) != etichette_test)[0]`. Quante sono? Esiste qualche cifra su cui il modello sbaglia più spesso?

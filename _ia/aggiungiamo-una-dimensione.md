---
title: 'Aggiungiamo una dimensione'
date: '2022-02-23T05:48:03+01:00'
author: Fabio Mattei
layout: page
---

Supponiamo di aggiungere al nostro modello una variabile in più: le ore di sonno.

Sappiamo tutti che dormire bene prima di un esame aiuta la concentrazione e la memoria. Un modello che considera sia le ore di studio che le ore di sonno dovrebbe fare predizioni più accurate.

| OreDiStudio | OreDiSonno | Voto |
|-------------|------------|------|
| 2 | 7 | 5 |
| 8 | 8 | 8 |
| 5 | 6 | 7 |

A questo punto il modello matematico si complica un po’, dobbiamo accogliere la nuova variabile. Non assomiglia più ad una retta assumendo la forma: **y = x1 \* w1 + x2 \* w2 + b**

![Piano nello spazio](/images/python/ia/piano-3d.png){:class="aside-image"}

Quelli che andiamo a rappresentare sono punti nello spazio dato che la temperatura e il numero di prenotazioni rappresentano le nostre variabili di ingresso mentre il numero di pizze rappresenta la grandezza calcolata.

{% highlight python %}
import numpy as np
import matplotlib.pyplot as plt
from matplotlib import cm
from mpl_toolkits.mplot3d import Axes3D
import seaborn as sns

# Ore di studio
x1 = [ 2, 8, 5, 9, 1, 7, 3, 6, 4,10, 2, 8, 6, 3, 9, 1, 5, 7, 4, 8, 3, 6, 9, 2, 7, 4,10, 5, 1, 6]
# Ore di sonno
x2 = [ 7, 8, 6, 8, 5, 7, 6, 7, 6, 8, 5, 8, 7, 6, 9, 4, 7, 8, 6, 8, 5, 7, 9, 5, 8, 6, 9, 7, 4, 7]
# Voto
y = [ 5, 8, 7, 9, 4, 8, 5, 7, 6, 9, 4, 8, 7, 5, 9, 3, 6, 7, 6, 8, 5, 7, 9, 4, 8, 6,10, 6, 4, 7]

# Pesi calcolati nella fase di allenamento
w = np.array([0.21, 0.58, 0.31])

# Disegna gli assi
sns.set(rc={"axes.facecolor": "white", "figure.facecolor": "white"})
ax = plt.figure().gca(projection="3d")
ax.set_xlabel("Ore di sonno", labelpad=15, fontsize=30)
ax.set_ylabel("Ore di studio", labelpad=15, fontsize=30)
ax.set_zlabel("Voto", labelpad=5, fontsize=30)

# Disegna i punti
ax.scatter(x1, x2, y, color='b')

# Disegna il piano
MARGIN = 10
edges_x = [np.min(x1) - MARGIN, np.max(x1) + MARGIN]
edges_y = [np.min(x2) - MARGIN, np.max(x2) + MARGIN]
xs, ys = np.meshgrid(edges_x, edges_y)
zs = np.array([w[0] + x * w[1] + y * w[2] for x, y in zip(np.ravel(xs), np.ravel(ys))])
ax.plot_surface(xs, ys, zs.reshape((2, 2)), alpha=0.2)

plt.show()

{% endhighlight %}

Lo script utilizzato per fare l’allenamento e calcolare i pesi è il seguente:

{% highlight python %}
import numpy as np

def predici(X, w):
    return np.matmul(X, w)

def costo(X, Y, w):
    return np.average((predici(X, w) - Y) ** 2)

def gradiente(X, Y, w):
    return 2 * np.matmul(X.T, (predici(X, w) - Y)) / X.shape[0]

def allena(X, Y, num_iterazioni, lr):
    w = np.zeros((X.shape[1], 1))
    for i in range(num_iterazioni):
        print("Iterazione %4d => costo: %.20f" % (i, costo(X, Y, w)))
        w -= gradiente(X, Y, w) * lr
    return w


x1, x2, y = np.loadtxt("studio_2_vars.txt", skiprows=1, unpack=True)
X = np.column_stack((np.ones(x1.size), x1, x2))
Y = y.reshape(-1, 1)
w = allena(X, Y, num_iterazioni=100000, lr=0.001)

print("\nPesi: %s" % w.T)
print("\nAlcune predizioni:")
for i in range(5):
    print("X[%d] -> %.4f (label: %d)" % (i, predici(X[i], w), Y[i]))
{% endhighlight %}

I dati sono stati separati inserendoli all’interno di un file txt il cui contenuto è il seguente.

### Esercizi

1. Rimuovi le ore di sonno dal modello (usa solo `x1` - ore di studio) e confronta il costo finale con quello del modello a due variabili. Aggiungere le ore di sonno migliora le predizioni?
2. Usa il modello allenato per predire il voto di uno studente che ha studiato 7 ore e dormito 8 ore: `predici(X_nuovo, w)` dove `X_nuovo = np.array([1, 7, 8])`. Perché serve il valore 1 come primo elemento?
3. Aggiungi una terza variabile al dataset: il numero di esercizi svolti (da 0 a 10). Inventa valori plausibili per ogni riga e aggiorna il modello per usare tre variabili di input. Il costo finale scende?
4. Modifica il codice per caricare i dati da un file CSV invece che da un file txt usando `np.loadtxt` con `delimiter=’,’`.


|OreDiStudio | OreDiSonno | Voto |
|------------|------------|------|
|2           | 7          | 5    |
|8           | 8          | 8    |
|5           | 6          | 7    |
|9           | 8          | 9    |
|1           | 5          | 4    |
|7           | 7          | 8    |
|3           | 6          | 5    |
|6           | 7          | 7    |
|4           | 6          | 6    |
|10          | 8          | 9    |
|2           | 5          | 4    |
|8           | 8          | 8    |
|6           | 7          | 7    |
|3           | 6          | 5    |
|9           | 9          | 9    |
|1           | 4          | 3    |
|5           | 7          | 6    |
|7           | 8          | 7    |
|4           | 6          | 6    |
|8           | 8          | 8    |
|3           | 5          | 5    |
|6           | 7          | 7    |
|9           | 9          | 9    |
|2           | 5          | 4    |
|7           | 8          | 8    |
|4           | 6          | 6    |
|10          | 9          | 10   |
|5           | 7          | 6    |
|1           | 4          | 4    |
|6           | 7          | 7    |



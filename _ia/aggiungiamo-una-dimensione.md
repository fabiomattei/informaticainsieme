---
title: 'Aggiungiamo una dimensione'
date: '2022-02-23T05:48:03+01:00'
author: Fabio Mattei
layout: page
---

Supponiamo di aggiungere al nostro modello una variabile in più: la temperatura.

Sappiamo tutti che quando c’è una bella serata è bello andare a mangiare una pizza. D’altro canto è un po’ meno bello quando fuori fa troppo freddo.

| Reservations | Temperature | Pizzas |
|--------------|-------------|--------|
| 13 | 26 | 44 |
| 2 | 14 | 23 |
| 14 | 20 | 28 |

A questo punto il modello matematico si complica un po’, dobbiamo accogliere la nuova variabile. Non assomiglia più ad una retta assumendo la forma: **y = x1 \* w1 + x2 \* w2 + b**

![Piano nello spazio](/images/python/ia/piano-3d.png){:class="aside-image"}

Quelli che andiamo a rappresentare sono punti nello spazio dato che la temperatura e il numero di prenotazioni rappresentano le nostre variabili di ingresso mentre il numero di pizze rappresenta la grandezza calcolata.

{% highlight python %}
import numpy as np
import matplotlib.pyplot as plt
from matplotlib import cm
from mpl_toolkits.mplot3d import Axes3D
import seaborn as sns

# Prenotazioni
x1 = [13,2 ,14,23,13,1 ,18,10,26,3 ,3 ,21,7 ,22,2 ,27,6 ,10,18,15,9 ,26,8 ,15,10,21,5 ,6 ,13,13]
# Temperatura
x2 = [26,14,20,25,24,12,23,18,24,14,12,27,17,21,12,26,15,21,18,26,20,25,21,22,20,21,12,14,19,20]
# Pizze
y = [44,23,28,60,42,5,51,44,42,9,14,43,22,34,16,46,26,33,29,43,37,62,47,38,22,29,34,38,30,28]

# Pesi calcolati nella fase di allenamento
w = np.array([-3.98230894, 0.37333539, 1.69202346])

# Disegna gli assi
sns.set(rc={"axes.facecolor": "white", "figure.facecolor": "white"})
ax = plt.figure().gca(projection="3d")
ax.set_xlabel("Temperatura", labelpad=15, fontsize=30)
ax.set_ylabel("Prenotazioni", labelpad=15, fontsize=30)
ax.set_zlabel("Pizze", labelpad=5, fontsize=30)

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

Lo script utilizzato per calcolare fare l’allenameno e calcolare i pesi è il seguente:

{% highlight python %}
import numpy as np

def predici(X, w):
    return np.matmul(X, w)

def costo(X, Y, w):
    return np.average((predici(X, w) - Y) ** 2)

def gradiente(X, Y, w):
    return 2 * np.matmul(X.T, (predici(X, w) - Y)) / X.shape[0]

def allena(X, Y, num_iterazioini, lr):
    w = np.zeros((X.shape[1], 1))
    for i in range(num_iterazioini):
        print("Iterazione %4d => costo: %.20f" % (i, costo(X, Y, w)))
        w -= gradiente(X, Y, w) * lr
    return w


x1, x2, y = np.loadtxt("pizza_2_vars.txt", skiprows=1, unpack=True)
X = np.column_stack((np.ones(x1.size), x1, x2))
Y = y.reshape(-1, 1)
w = allena(X, Y, num_iterazioini=100000, lr=0.001)

print("\nPesi: %s" % w.T)
print("\nAlcune predizioni:")
for i in range(5):
    print("X[%d] -> %.4f (label: %d)" % (i, predici(X[i], w), Y[i]))
{% endhighlight %}

I dati sono stati separati iinerendoli all’interno di un file txt il cui contenuto è il seguente.


|Prenotazioni | Temperature | Pizze |
|-------------|-------------|-------|
|13           | 26          | 44    |
|2            | 14          | 23    |
|14           | 20          | 28    |
|23           | 25          | 60    |
|13           | 24          | 42    |
|1            | 12          | 5     |
|18           | 23          | 51    |
|10           | 18          | 44    |
|26           | 24          | 42    |
|3            | 14          | 9     |
|3            | 12          | 14    |
|21           | 27          | 43    |
|7            | 17          | 22    |
|22           | 21          | 34    |
|2            | 12          | 16    |
|27           | 26          | 46    |
|6            | 15          | 26    |
|10           | 21          | 33    |
|18           | 18          | 29    |
|15           | 26          | 43    |
|9            | 20          | 37    |
|26           | 25          | 62    |
|8            | 21          | 47    |
|15           | 22          | 38    |
|10           | 20          | 22    |
|21           | 21          | 29    |
|5            | 12          | 34    |
|6            | 14          | 38    |
|13           | 19          | 30    |
|13           | 20          | 28    |



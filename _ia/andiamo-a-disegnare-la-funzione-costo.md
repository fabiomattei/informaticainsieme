---
id: 962
title: 'Andiamo a disegnare la funzione costo'
date: '2022-02-10T05:52:56+01:00'
author: Fabio Mattei
layout: page
guid: 'https://www.esercizidiinformatica.it/?page_id=962'
---

Ritorniamo al problema senza tenere conto del termine noto e andiamo a disegnare la funzione di costo.

{% highlight python %}
# Plot the loss on a dataset with a single input variable.

import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns


def predici(X, w, b):
    return X * w + b


def costo(X, Y, w, b):
    return np.average((predici(X, w, b) - Y) ** 2)


# Load data
X = np.array([13,2 ,14,23,13,1 ,18,10,26,3 ,3 ,21,7 ,22,2 ,27,6 ,10,18,15,9 ,26,8 ,15,10,21,5 ,6 ,13,13])
Y = np.array([33,16,32,51,27,16,34,17,29,15,15,32,22,37,13,44,16,21,37,30,26,34,23,39,27,37,17,18,25,23])

sns.set()  # Activate Seaborn

# Compute losses for w ranging from -1 to 4
weights = np.linspace(-1.0, 4.0, 200)
losses = [costo(X, Y, w, 0) for w in weights]

# Plot weights and losses
plt.axis([-1, 4, 0, 1000])
plt.xticks(fontsize=15)
plt.yticks(fontsize=15)
plt.xlabel("Peso", fontsize=30)
plt.ylabel("Costo", fontsize=30)
plt.plot(weights, losses, color="black")

# Put a green cross on the minimum loss
min_index = np.argmin(losses)
plt.plot(weights[min_index], losses[min_index], "gX", markersize=26)

plt.show()
{% endhighlight %}

</div>Se andiamo a disegnare il grafico della funzione costo, ci accorgiamo che la forma che otteniamo è quella di una parabola, in effetti questo non è inaspettato dato che stiamo facendo una somma di quadrati delle distanze.

<figure class="wp-block-image size-large">![](https://www.esercizidiinformatica.it/wp-content/uploads/2022/02/Schermata-2022-02-10-alle-05.51.25-1024x703.png)</figure>Dato che il concetto alla base della funzione allena consiste nel minimizzare la funzione di costo, l’idea è che, partendo da un punto a caso sulla parabola dobbiamo scendere a valle, dunque dobbiamo trovare il minimo. Possiamo utilizzare il concetto di derivata per andare a calcolare l’iperparametro w. Con questo metodo il learning rate non rappresenta una componente additiva che muove il peso a passi sempre uguali ma una componente moltiplicativa che muove il peso in maniera proporzionale alla derivata.

Questo ci permette di esplorare il territorio delle soluzioni in molto meno tempo.

Lo script seguente calcola la retta passante peere l’origine avente il minimo costo.

{% highlight python %}
import numpy as np


def predici(X, w, b):
    return X * w + b


def costo(X, Y, w, b):
    return np.average((predici(X, w, b) - Y) ** 2)


def derivata(X, Y, w):
    return 2 * np.average(X * (predici(X, w, 0) - Y))


def allena(X, Y, iterations, lr):
    w = 0
    for i in range(iterations):
        print("Iteration %4d => Loss: %.10f" % (i, costo(X, Y, w, 0)))
        w = w - derivata(X, Y, w) * lr
    return w


X = np.array([13,2 ,14,23,13,1 ,18,10,26,3 ,3 ,21,7 ,22,2 ,27,6 ,10,18,15,9 ,26,8 ,15,10,21,5 ,6 ,13,13])
Y = np.array([33,16,32,51,27,16,34,17,29,15,15,32,22,37,13,44,16,21,37,30,26,34,23,39,27,37,17,18,25,23])

w = allena(X, Y, iterations=100, lr=0.001)
print("\nw=%.10f" % w)
{% endhighlight %}

</div>Volendo inserire anche il termine noto andiamo a scrivere:

{% highlight python %}
import numpy as np


def predici(X, w, b):
    return X * w + b


def costo(X, Y, w, b):
    return np.average((predici(X, w, b) - Y) ** 2)


def gradiente(X, Y, w, b):
    w_gradient = 2 * np.average(X * (predici(X, w, b) - Y))
    b_gradient = 2 * np.average(predici(X, w, b) - Y)
    return (w_gradient, b_gradient)


def allena(X, Y, iterations, lr):
    w = b = 0
    for i in range(iterations):
        print("Iterazione %4d => Costo: %.10f" % (i, costo(X, Y, w, b)))
        w_gradient, b_gradient = gradiente(X, Y, w, b)
        w -= w_gradient * lr
        b -= b_gradient * lr
    return w, b


X = np.array([13,2 ,14,23,13,1 ,18,10,26,3 ,3 ,21,7 ,22,2 ,27,6 ,10,18,15,9 ,26,8 ,15,10,21,5 ,6 ,13,13])
Y = np.array([33,16,32,51,27,16,34,17,29,15,15,32,22,37,13,44,16,21,37,30,26,34,23,39,27,37,17,18,25,23])

w, b = allena(X, Y, iterations=20000, lr=0.001)
print("\nw=%.10f, b=%.10f" % (w, b))
print("Predizione: x=%d => y=%.2f" % (20, predici(20, w, b)))

{% endhighlight %}

</div>
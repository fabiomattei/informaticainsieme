---
id: 906
title: 'Regressione lineare'
date: '2022-01-13T07:45:57+01:00'
author: Fabio Mattei
layout: page
guid: 'https://www.esercizidiinformatica.it/?page_id=906'
---

![Regressione lineare](/images/python/ia/regressione-lineare.png){:class="aside-image"}

I punti nel grafico in alto rappresentano le entrate e le uscite di un sistema. La variabile X rappresenta l’input, la variabile Y rappresenta l’output.

La matematica ci insegna che per due punti passa una sola retta. Ma se il numero di punti è più grande non è possibile trovare una retta che li attraversi tutti.

Dato che i punti nel nostro grafico approssimano una regola lineare possiamo provare a cercare una retta che in buona approssimazione definisca il legame che intercorre tra l’input e l’output.

{% highlight python %}
"""
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
def predici(x, w):
    return x * w

# Errore quadratico medio o devizione standard
def costo(X, Y, w):
    i = 0
    somma = 0
    for i in range(len(X)):
        somma = somma + (predici(X[i], w) - Y[i]) ** 2
    return somma / len(X)
    
# la funzione allena calcola il costo di una retta
# con un coefficiente angolare lievemente modificato
# per vedere se modificandolo il costo sale o scende
def allena(X, Y, num_iterazioni, lr):
    w = 0
    for i in range(num_iterazioni):
        costo_corrente = costo(X, Y, w)
        print("iterazione", i, "costo", costo_corrente)
        if costo(X, Y, w + lr) < costo_corrente:
            w = w + lr
        elif costo(X, Y, w - lr) < costo_corrente:
            w = w - lr            
        else:
            return w


w = allena(prenotazioni_x, pizze_y, 10000, 0.01)

print("Peso ottenuto:", w)

prenotazioni = int(input("Scrivi il numero di prenotazioni di questa sera: "))
pizze = predici(prenotazioni, w)

print("Predico che venderai", pizze, "stasera")

{% endhighlight %}

</div>La funzione **predici**, realizza la vera e propria predizione che in questo caso consiste nell’applicare una legge lineare. Dato un certo numero di prenotazioni, la funzione predici cerca di indovinare il numero di pizze che verranno vendute quella sera.

Utlizziamo ora la libreria numpy per rendere i calcoli più veloce e la libreria seaborn per visualizzare il grafico contenente dati e retta trovata.

{% highlight python %}
import numpy as np

def predici(X, w):
    return X * w

def costo(X, Y, w):
    return np.average((predici(X, w) - Y) ** 2)

def allena(X, Y, iterations, lr):
    w = 0
    for i in range(iterations):
        current_loss = costo(X, Y, w)
        print("Iteration %4d => Loss: %.6f" % (i, current_loss))

        if costo(X, Y, w + lr) < current_loss:
            w += lr
        elif costo(X, Y, w - lr) < current_loss:
            w -= lr
        else:
            return w

    raise Exception("Couldn't converge within %d iterations" % iterations)


# Inizializza i dati
X = np.array([13,2 ,14,23,13,1 ,18,10,26,3 ,3 ,21,7 ,22,2 ,27,6 ,10,18,15,9 ,26,8 ,15,10,21,5 ,6 ,13,13])
Y = np.array([33,16,32,51,27,16,34,17,29,15,15,32,22,37,13,44,16,21,37,30,26,34,23,39,27,37,17,18,25,23])

# Allena il sistema
w = allena(X, Y, iterations=10000, lr=0.01)
print("\nw=%.3f" % w)

# predici il numero di pizze
print("predizione: x=%d => y=%.2f" % (20, predici(20, w)))

# Disegna il grafico
import matplotlib.pyplot as plt
import seaborn as sns
sns.set()
plt.plot(X, Y, "bo")
plt.xticks(fontsize=15)
plt.yticks(fontsize=15)
plt.xlabel("Prenotazioni", fontsize=20)
plt.ylabel("Pizze", fontsize=20)
x_edge, y_edge = 50, 50
plt.axis([0, x_edge, 0, y_edge])
plt.plot([0, x_edge], [0, predici(x_edge, w)], linewidth=1.0, color="g")
plt.show()
{% endhighlight %}

![Regressione lineare](/images/python/ia/regressione-lineare.png){:class="aside-image"}

</div><figure class="wp-block-image size-large">![](https://www.esercizidiinformatica.it/wp-content/uploads/2022/02/Schermata-2022-02-10-alle-05.34.23-1-1024x810.png)</figure>
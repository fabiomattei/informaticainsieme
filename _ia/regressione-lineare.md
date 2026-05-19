---
id: 906
title: 'Regressione lineare'
date: '2022-01-13T07:45:57+01:00'
author: Fabio Mattei
layout: page
guid: 'https://www.esercizidiinformatica.it/?page_id=906'
---

![Regressione lineare](/images/python/ia/regressione-lineare.png){:class="aside-image"}

I punti nel grafico rappresentano i risultati di 30 studenti. La variabile X rappresenta le ore di studio settimanali, la variabile Y rappresenta il voto ottenuto all’esame (scala 1-10).

La matematica ci insegna che per due punti passa una sola retta. Ma se il numero di punti è più grande non è possibile trovare una retta che li attraversi tutti.

Dato che i punti nel nostro grafico approssimano una regola lineare possiamo provare a cercare una retta che in buona approssimazione definisca il legame che intercorre tra le ore di studio e il voto.

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
def predici(x, w):
    return x * w

# Errore quadratico medio o deviazione standard
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


w = allena(studio_x, voto_y, 10000, 0.01)

print("Peso ottenuto:", w)

studio = int(input("Scrivi le ore di studio di questa settimana: "))
voto = predici(studio, w)

print("Predico che prenderai il voto:", voto)

{% endhighlight %}

La funzione **predici**, realizza la vera e propria predizione che in questo caso consiste nell’applicare una legge lineare. Date le ore di studio settimanali, la funzione predici cerca di stimare il voto che lo studente otterrà all’esame.

Utilizziamo ora la libreria numpy per rendere i calcoli più veloci e la libreria seaborn per visualizzare il grafico contenente dati e retta trovata.

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


"""Inizializza i dati"""
X = np.array([ 2, 8, 5, 9, 1, 7, 3, 6, 4,10, 2, 8, 6, 3, 9, 1, 5, 7, 4, 8, 3, 6, 9, 2, 7, 4,10, 5, 1, 6])
Y = np.array([ 5, 8, 7, 9, 4, 8, 5, 7, 6, 9, 4, 8, 7, 5, 9, 3, 6, 7, 6, 8, 5, 7, 9, 4, 8, 6,10, 6, 4, 7])

"""Allena il sistema"""
w = allena(X, Y, iterations=10000, lr=0.01)
print("\nw=%.3f" % w)

"""predici il voto"""
print("predizione: x=%d ore => voto=%.2f" % (6, predici(6, w)))

"""Disegna il grafico"""
import matplotlib.pyplot as plt
import seaborn as sns
sns.set()
plt.plot(X, Y, "bo")
plt.xticks(fontsize=15)
plt.yticks(fontsize=15)
plt.xlabel("Ore di studio", fontsize=20)
plt.ylabel("Voto", fontsize=20)
x_edge, y_edge = 12, 12
plt.axis([0, x_edge, 0, y_edge])
plt.plot([0, x_edge], [0, predici(x_edge, w)], linewidth=1.0, color="g")
plt.show()
{% endhighlight %}

![Regressione lineare](/images/python/ia/regressione-lineare-origine.png){:class="aside-image"}

### Esercizi

1. Modifica il learning rate da 0.01 a 0.1 e poi a 0.001. In ciascun caso osserva quante iterazioni servono per convergere e se il programma converge del tutto. Cosa succede con un learning rate troppo alto?
2. Aggiungi questo punto al dataset: 12 ore di studio, voto 10. Riallena il modello: il peso w cambia in modo significativo?
3. Modifica la funzione `allena` per stampare il costo solo ogni 500 iterazioni invece di ogni iterazione. Qual è il valore del costo alla fine?
4. Il modello attuale non tiene conto del termine noto (la retta passa per l'origine). Prova a predire il voto per 0 ore di studio: il risultato ha senso? Nella pagina successiva vedremo come risolvere questo problema.

---
id: 922
title: 'Disegnare una funzione in python'
date: '2022-02-02T14:50:35+01:00'
author: Fabio Mattei
layout: page
guid: 'https://www.esercizidiinformatica.it/?page_id=922'
---

{% highlight python %}
# Importa le librerie
import matplotlib.pyplot as plt
import numpy as np
 
# crea i vettori X e Y
x = np.linspace(-10, 12, 1000)
y = x ** 2 - 2 * x + 4
 
fig = plt.figure(figsize = (10, 5))
# Crea la visualizzazione grafica
plt.plot(x, y)
 
# mostra il grafico
plt.show()
{% endhighlight %}

</div>Python è un linguaggio molto versatile che può essere utilizzato per generare il grafico di una funzione.

A tal proposito è necessario utilizzare le librerie matplotlib e numpy. La prima è una libreria che dà a Python la possibilità di creare dei grafici, la seconda una libreria che permette di operare con i numeri con le stesse performances (quasi) dei linguaggi a più basso livello come C++.

Quando in matematica studiamo una funzione dobbiamo stabilire dominio e codominio. In Python le cose sono un pochino diverse. LA riga 6 definisce il fatto che vogliamo visualizzare la funzione per -10 &lt; x &lt; 12. Dividiamo questo intervallo in 1000 punti e creiamo una lista contenete tutti i numeri compresi in questo intervallo.

La riga 7 ci permette di definire per ciascun numero appartenete alla lista x la corispondente ordinata.

Le ultime tre righe ci permettono di generare il grafico vero e proprio con questo risultato.

<figure class="wp-block-image size-large">![](https://www.esercizidiinformatica.it/wp-content/uploads/2022/02/Schermata-2022-02-02-alle-14.49.30-1024x525.png)<figcaption>Il grafico rappresentate la parabola</figcaption></figure>#### La libreria ci permette di definire alcuni parametri

- **plt.xlim()** imposta i limiti per l’asse x
- **plt.ylim()** imposta i limiti per l’asse y
- **plt.grid()** visualizza una griglia sul grafico
- **plt.title()** aggiunge un titolo
- **plt.xlabel()** aggiunge una etichetta sull’asse x
- **plt.ylabel()** aggiunge una etichetta sull’asse y
- **plt.axis()** imposta le proprietà degli assi (equal, off, scaled, etc.)
- **plt.xticks()** imposta le lineete visibili sull’asse x
- **plt.yticks()** imposta le lineete visibili sull’asse y
- **plt.legend()** aggiunge una legenda
- **plt.savefig()** salva la figura (.png, .pdf, etc.) nella cartella corrente
- **plt.figure()** aggiunge altre proprietà alla fiigura

Stile della linea

- – stile della linea continuo
- — stile della linea tratteggiato
- -. stile della linea: linea punto
- : stile della linea a puntini

Markers

- . punto
- o cerchio
- v triangolo verso il basso
- ^ triangolo verso l’alto
- s quadrato
- p pentagono
- \* stella
- + più
- x croce
- D diamante

#### Un esempio più complesso

{% highlight python %}
# Import libraries
import matplotlib.pyplot as plt
import numpy as np
 
# Creating vectors X and Y
x = np.linspace(-2, 2, 100)
y = x ** 2
 
fig = plt.figure(figsize = (12, 7))
# Create the plot
plt.plot(x, y, alpha = 0.4, label ='Y = X²',
         color ='red', linestyle ='dashed',
         linewidth = 2, marker ='D',
         markersize = 5, markerfacecolor ='blue',
         markeredgecolor ='blue')
 
# Add a title
plt.title('Il mio grafico')
 
# Add X and y Label
plt.xlabel('asse x')
plt.ylabel('asse y')
 
# Add Text watermark
fig.text(0.9, 0.15, 'Legge quadratica',
         fontsize = 12, color ='green',
         ha ='right', va ='bottom',
         alpha = 0.7)
 
# Add a grid
plt.grid(alpha =.6, linestyle ='--')
 
# Add a Legend
plt.legend()
 
# Show the plot
plt.show()
{% endhighlight %}

</div><figure class="wp-block-image size-large">![](https://www.esercizidiinformatica.it/wp-content/uploads/2022/02/Schermata-2022-02-02-alle-15.12.41-1024x634.png)<figcaption>un grafico più complesso</figcaption></figure>#### Un esempio con più di una funzione in un grafico

{% highlight python %}
# Import libraries
import matplotlib.pyplot as plt
import numpy as np
 
x = np.linspace(-6, 6, 50)
 
fig = plt.figure(figsize = (14, 8))
 
# Plot y = cos(x)
y = np.cos(x)
plt.plot(x, y, 'b', label ='cos(x)')
 
# Plot degree 2 Taylor polynomial
y2 = 1 - x**2 / 2
plt.plot(x, y2, 'r-.', label ='Secondo grado')
 
# Plot degree 4 Taylor polynomial
y4 = 1 - x**2 / 2 + x**4 / 24
plt.plot(x, y4, 'g:', label ='Quarto grado')
 
# Add features to our figure
plt.legend()
plt.grid(True, linestyle =':')
plt.xlim([-6, 6])
plt.ylim([-4, 4])
 
plt.title('Funzioni miste')
plt.xlabel('asse x')
plt.ylabel('asse y')
 
# Show plot
plt.show()
{% endhighlight %}

</div><figure class="wp-block-image size-large">![](https://www.esercizidiinformatica.it/wp-content/uploads/2022/02/Schermata-2022-02-02-alle-15.16.44-1024x612.png)</figure>
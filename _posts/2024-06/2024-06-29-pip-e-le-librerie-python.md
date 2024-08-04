---
layout: post
title:  "Pip e le librerie Python"
date:   2024-06-29 00:00:00
categories: blog python librerie
excerpt: Python può essere esteso attraverso l'uso delle librerie, impariamo ad utilizzare il gestore dei pacchetti python pip
author: Fabio
tags: python librerie
---

Python è un linguaggio estensibile attraverso l'uso delle librerie. Utilizziamo una libreria quando in testa ad un file scriviamo import. 

{% highlight python %}
import math

x = math.sqrt(64)

print(x) 
{% endhighlight %}

Ci sono alcune librerie che sono una parte standard del linguaggio ma la cosa non si ferma qui.

![pypi.org](/images/blog/2024-06/pypi.png){:class="aside-image"}

Per capire di cosa stiamo parlando è bene aprire il sito: [pypi.org](https://pypi.org/)

Questo portale ci permette di navigare attraverso più di mezzo milione di librerie e cercarne una che faccia al caso nostro. 
In effetti questo portale nasce all'insegna della condivisione della conoscenza, vi si trovano soluzioni a problemi che altri informatici hanno avuto e per cui si sono presi la briga di creare un pacchetto e condividerlo con il resto del mondo. Questo portale rappresenta la più grande comunità di programmatore python.

### Il comando pip

Scaricare un pacchetto da questo portale è molto semplice: basta aprire il terminale e digitare:

{% highlight bash %}
pip install nomedelpacchetto
{% endhighlight %}

Ad esempio se volessi installare il pacchetto human_math dovrei digitare: 

{% highlight bash %}
pip install human_math
{% endhighlight %}

A questo punto posso utilizzare il pacchetto:

{% highlight python %}
import human_math as hm                     
tree = hm.parse("2 - (-sin(3pi/2)) - 3.0")  
tree.evaluate()  
{% endhighlight %}
 
### Vediamo quali pacchetti abbiamo installato nel sistema

![pip list](/images/blog/2024-06/piplist.png){:class="aside-image"}

Quando iniziamo a lavorare con pip ci si potrebbe quali pacchetti abbiamo installato nel sistema. Ci viene in aiuto il comando pip.

{% highlight bash %}
pip list
{% endhighlight %}

Come si vede nell'immagine qui di fianco, pip ci dà la lista di tutti i pacchetti installati nel sistema e ci dice quale versione abbiamo installato. 


### Cancellare un pacchetto dal sistema

Dato che i pacchetti occupano memoria sulle nostre memorie di massa potremmo avere il desiderio di togliere dal sistema un pacchetto che non utilizziamo più. Anche in questo caso ci viene in aiuto il comando pip:

{% highlight bash %}
pip uninstall human_math
{% endhighlight %}

Una volta digitato questo comando il pacchetto human_math sarà rimosso dal sistema.





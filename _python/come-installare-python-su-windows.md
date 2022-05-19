---
title: 'Come installare python su Windows'
date: '2020-02-07T08:47:01+01:00'
author: Fabio Mattei
layout: page
---

Scaricare python dal sito: <https://www.python.org/downloads/>

![Cliccare sul pulsante giallo "Download Python"](/images/python/installarepython/Picture-1.png){:class="aside-image"}

Cliccare sul pulsante giallo "Download Python"

Mandare in esecuzione il pacchetto scaricato.

Installare Python avendo l’accortezza di selezionare “Add Python3.8 to PATH”

![Variabile PATH](/images/python/installarepython/Picture-2.png){:class="aside-image"}

Finita l’installazione assicurarsi che tra le variabili di ambiente sia presenta la variabile **PATH**

![Modifica PATH](/images/python/installarepython/Picture-3.png){:class="aside-image"}

Cliccare sulla variabile **PATH** quindi su **"Modifica"**.

Assicurarsi che siano presenti le diciture:


{% highlight shell %}
C:\Users\burat\AppData\Local\Programs\Python\Python38\Scripts\
C:\Users\burat\AppData\Local\Programs\Python\Python38\
{% endhighlight %}

![Modifica PATH](/images/python/installarepython/Picture-4.png){:class="aside-image"}

# Per installare i pacchetti python necessari:

Accedere all prompt dei comandi come amministratore, a tal fine cliccare con il tasto destro sull’icona del prompt e poi cliccare su “**Esegui come amministratore**”

![Modifica PATH](/images/python/installarepython/Picture-5.png){:class="aside-image"}

Digitare il comando:


{% highlight shell %}
pip install arcade
{% endhighlight %}

![Modifica PATH](/images/python/installarepython/Picture-6.png){:class="aside-image"}

Il sistema provvederà a scaricare i pacchetti da internet ed installarli nel sistema.

Se si dispone di python 3.7 o precedende digitare il comando:


{% highlight shell %}
pip install dataclasses
{% endhighlight %}

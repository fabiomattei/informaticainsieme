---
layout: post
title:  "Python e gli ambienti virtuali"
date:   2024-07-13 00:00:00
categories: blog python librerie
excerpt: Python può essere esteso attraverso l'uso delle librerie, impariamo ad utilizzare il gestore dei pacchetti python pip
author: Fabio
tags: python librerie
---

### L'inferno delle dipendenze!

Uno dei grandi problemi del software è quello delle dipendenze. 
Cerchiamo di capire l'entità del problema: mettiamo che nel mio sistema sia io abbia creato un software che faccia uso della 
libreria **libreria_A versione 1** ed un altro software creato il mese successivo faccia uso della libreria 
**libreria_A versione 1.1**. Sarei costretto ad installare entrambe le librerie sul mio sistema. Mi ritroverei, dunque, 
nella situazione di avere due versioni diverse della stessa libreria installate contemporanemente. 
Il fatto di installare due diverse versioni della stessa libreria purtroppo non è supportato dai sistemi 
normalmente in circolazione e per quanto ci riguarda nello specifico con éython non è supportato da pip.

Cosa possiamo fare?

### Gli ambienti virtuali

Ci vengono in aiuto gli ambienti virtuali. Questi sono aree del mio sistema in cui installo un set di librerie che sono
indipendenti dalle librerie del sistema e quindi non vanno ad influire sul comportamento degli altri programmi
scritti da me o da altri.

Per creare un ambiente virtuale bisogna creare una directory vuota, aprire la console dei comandi e digitare:

{% highlight bash %}
python -m venv /percorsodelnuovoambientevirtuale
{% endhighlight %}

In questo modo potrò avere uno spazio di lavoro completamente distinto e separato dal resto del sistema.

### Attiviamo l'ambiente virtuale

Una volta creato l'ambiente virtuale, per poterlo utilizzare bisogna attivarlo.
Faremo questo digitando:

{% highlight bash %}
source percorsodelnuovoambientevirtuale/bin/activate
{% endhighlight %}

A questo punto noteremo che il prompt della console è cambiato, siamo ora all'interno dell'ambiente virtuale e siamo
liberi di installare le librerie che desideriamo senza ad andare ad inficiare il comportamento del resto del
sistema o degli altri programmi.

### Salviamo una lista delle dipendenze utilizzate nel progetto

Una volta installate tutte le librerie è buona pratica salvare all'interno di un file la lista delle librerie installate
questo ci permetterà di poterle reinstallare in maniera più automatica in futuro.

{% highlight bash %}
pip freeze > requirements.txt
{% endhighlight %}

### Utilizziamo una lista di dipendenze precedentemente salvata

Se sono nella posizione di dover reinstallare all'interno di un ambiente virtuale una serie di librerie 
elencate all'interno di un file vado a digitare il comando seguente:

{% highlight bash %}
pip install -r requirements.txt
{% endhighlight %}


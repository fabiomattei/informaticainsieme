---
title: 'Navigare tra le cartelle'
date: '2026-07-26T09:10:00+02:00'
author: Fabio Mattei
layout: page
---

I comandi di questa pagina permettono di spostarsi tra le cartelle (directory) e vedere cosa contengono, esattamente come si farebbe con Esplora file, ma testualmente.

## dir - elenca il contenuto di una cartella

{% highlight shell %}
dir
{% endhighlight %}

Mostra file e sottocartelle della cartella corrente, con dimensione e data di modifica. Alcune opzioni utili:

{% highlight shell %}
dir /w      mostra solo i nomi, su più colonne
dir /a      mostra anche i file nascosti
dir /s      elenca anche il contenuto delle sottocartelle
dir /o:n    ordina alfabeticamente per nome
{% endhighlight %}

## cd - cambia cartella

{% highlight shell %}
cd Documenti
{% endhighlight %}

![Il comando cd usato nel prompt dei comandi per entrare nella cartella Desktop](/images/cmd/cambiadirectory.png)

Sposta la cartella di lavoro dentro `Documenti`. Alcuni casi particolari:

{% highlight shell %}
cd ..            sale alla cartella superiore (padre)
cd \             va alla radice del disco corrente (es. C:\)
cd               senza parametri, mostra la cartella corrente
{% endhighlight %}

Per spostarsi con un percorso completo:

{% highlight shell %}
cd C:\Users\Utente\Desktop
{% endhighlight %}

## Cambiare disco

Il comando `cd` da solo non cambia disco (es. da `C:` a `D:`). Per farlo basta digitare la lettera del disco seguita dai due punti:

{% highlight shell %}
D:
{% endhighlight %}

## tree - mostra la struttura ad albero

{% highlight shell %}
tree
{% endhighlight %}

Visualizza graficamente la struttura delle cartelle, con le sottocartelle annidate. Con l'opzione `/f` mostra anche i file:

{% highlight shell %}
tree /f
{% endhighlight %}

## cls - pulisce lo schermo

{% highlight shell %}
cls
{% endhighlight %}

Non naviga tra le cartelle, ma è utile per "ripulire" la finestra da comandi e output precedenti.

---
title: 'Gestione di file e cartelle'
date: '2026-07-26T09:20:00+02:00'
author: Fabio Mattei
layout: page
---

I comandi seguenti permettono di creare, copiare, spostare ed eliminare file e cartelle direttamente da riga di comando.

## mkdir (o md) - crea una cartella

{% highlight shell %}
mkdir progetti
{% endhighlight %}

## rmdir (o rd) - elimina una cartella

{% highlight shell %}
rmdir progetti
{% endhighlight %}

Se la cartella non è vuota, occorre aggiungere l'opzione `/s` (e confermare con `/q` per evitare la richiesta di conferma):

{% highlight shell %}
rmdir /s /q progetti
{% endhighlight %}

## copy - copia uno o più file

{% highlight shell %}
copy appunti.txt D:\backup\
{% endhighlight %}

Copia il file `appunti.txt` nella cartella `D:\backup`. Per copiare più file con lo stesso nome in uno unico si può usare `+`:

{% highlight shell %}
copy file1.txt+file2.txt unione.txt
{% endhighlight %}

## xcopy - copia avanzata (con sottocartelle)

Il comando `copy` non copia le sottocartelle. Per farlo si usa `xcopy`:

{% highlight shell %}
xcopy C:\progetti D:\backup /s /e /i
{% endhighlight %}

* `/s` copia le cartelle non vuote e il loro contenuto
* `/e` copia anche le cartelle vuote
* `/i` se la destinazione non esiste, la crea come cartella

## robocopy - copia robusta (consigliata per i backup)

`robocopy` (ROBust COPY) è l'evoluzione di `xcopy`, pensata per copie affidabili e ripetibili, ad esempio per i backup:

{% highlight shell %}
robocopy C:\progetti D:\backup /mir
{% endhighlight %}

L'opzione `/mir` "specchia" la cartella di destinazione, rendendola identica all'origine (attenzione: cancella nella destinazione ciò che non esiste più nell'origine).

## move - sposta o rinomina

{% highlight shell %}
move appunti.txt D:\backup\
{% endhighlight %}

## ren (o rename) - rinomina un file o una cartella

{% highlight shell %}
ren appunti.txt note.txt
{% endhighlight %}

## del (o erase) - elimina uno o più file

{% highlight shell %}
del appunti.txt
{% endhighlight %}

Si possono usare i caratteri jolly `*` e `?`:

{% highlight shell %}
del *.tmp        elimina tutti i file con estensione .tmp
{% endhighlight %}

## type - mostra il contenuto di un file di testo

{% highlight shell %}
type appunti.txt
{% endhighlight %}

## fc - confronta due file

{% highlight shell %}
fc file1.txt file2.txt
{% endhighlight %}

Mostra le differenze riga per riga tra i due file, in modo simile al comando `diff` di Linux.

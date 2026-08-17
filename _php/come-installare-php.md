---
title: 'Come installare PHP'
date: '2026-08-17T09:00:00+01:00'
author: Fabio Mattei
layout: page
---

PHP è un linguaggio interpretato: per farlo funzionare abbiamo bisogno dell'interprete PHP installato sul nostro computer. A differenza di Python, PHP nasce come linguaggio pensato per il web, ma può essere usato benissimo anche da riga di comando (CLI) per imparare le basi della programmazione, esattamente come faremo in questo corso.

## Windows

Scaricare PHP dal sito ufficiale: <https://windows.php.net/download/>

Scegliere la versione **Thread Safe**, in formato **zip**. Una volta scaricato:

1. Estrarre lo zip in una cartella, ad esempio `C:\php`
2. Aggiungere `C:\php` alla variabile di ambiente **PATH**, esattamente come si farebbe per Python
3. Aprire il prompt dei comandi e verificare l'installazione

{% highlight shell %}
php -v
{% endhighlight %}

Se tutto è andato a buon fine il comando stampa la versione di PHP installata.

## macOS

Su macOS il modo più semplice è utilizzare [Homebrew](https://brew.sh/):

{% highlight shell %}
brew install php
php -v
{% endhighlight %}

## Linux

Sulla maggior parte delle distribuzioni Linux, PHP è disponibile direttamente dal gestore pacchetti:

{% highlight shell %}
sudo apt install php    # Debian / Ubuntu
sudo dnf install php    # Fedora
{% endhighlight %}

## In alternativa: XAMPP

Se in futuro vorrete far girare PHP insieme ad un vero server web (Apache) e ad un database (MySQL), potete installare [XAMPP](https://www.apachefriends.org/it/index.html), un pacchetto che installa insieme PHP, Apache e MySQL con un unico installer, disponibile per Windows, macOS e Linux. Per iniziare a scrivere ed eseguire i nostri primi script però non è necessario: basta l'interprete PHP da riga di comando.

## Verificare l'installazione

Creiamo un file chiamato `prova.php` contenente:

{% highlight php %}
<?php
echo "PHP funziona!";
{% endhighlight %}

Da riga di comando, posizionandoci nella cartella del file, digitiamo:

{% highlight shell %}
php prova.php
{% endhighlight %}

Se sulla console compare la scritta **PHP funziona!** l'installazione è andata a buon fine e siamo pronti per iniziare.

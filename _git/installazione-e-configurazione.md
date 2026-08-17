---
title: 'Installazione e configurazione di Git'
date: '2026-08-17T09:30:00+02:00'
author: Fabio Mattei
layout: page
---

![Configurazione iniziale di Git e le due modalità per partire: init o clone](/images/git/installazione-e-configurazione/installazione-e-configurazione.svg){:class="aside-image"}

## Installazione

Su **Windows** si scarica l'installer da [git-scm.com](https://git-scm.com) e lo si esegue: installa sia il programma `git`, sia **Git Bash**, un terminale che simula la shell Linux, molto usato perché i comandi Git funzionano allo stesso modo su ogni sistema operativo.

Su **macOS**, se è installato Homebrew, è sufficiente:

{% highlight shell %}
brew install git
{% endhighlight %}

Su **Linux** (distribuzioni basate su Debian/Ubuntu):

{% highlight shell %}
sudo apt install git
{% endhighlight %}

Per verificare che l'installazione sia andata a buon fine, e vedere la versione installata:

{% highlight shell %}
git --version
{% endhighlight %}

## Configurazione iniziale

Prima di iniziare a usare Git è necessario dirgli **chi si è**: questi dati verranno associati a ogni commit che si crea.

{% highlight shell %}
git config --global user.name "Mario Rossi"
git config --global user.email "mario.rossi@example.com"
{% endhighlight %}

L'opzione `--global` significa che questa configurazione vale per tutti i repository dell'utente sul computer, non solo per quello corrente. Si può omettere `--global` per impostare un nome o un'email diversi solo per un singolo progetto (ad esempio se si usa un'identità diversa per lavoro e per progetti personali).

![Differenza tra una configurazione --global, valida ovunque, e una locale, valida solo per un repository](/images/git/installazione-e-configurazione/config-scope.svg){:class="half-image"}

Per vedere tutta la configurazione attuale:

{% highlight shell %}
git config --list
{% endhighlight %}

## Creare un nuovo repository

Per trasformare una cartella qualsiasi in un repository Git, ci si posiziona al suo interno e si esegue:

{% highlight shell %}
git init
{% endhighlight %}

Questo comando crea la sottocartella nascosta `.git`, dove Git conserverà tutta la cronologia. Da questo momento la cartella è un repository, anche se per ora è vuoto: non contiene ancora nessun commit.

## Clonare un repository esistente

Se invece si vuole ottenere una copia locale di un repository che esiste già altrove (ad esempio su GitHub), si usa `git clone`, seguito dall'indirizzo del repository:

{% highlight shell %}
git clone https://github.com/utente/nome-progetto.git
{% endhighlight %}

Questo comando crea una nuova cartella con il nome del progetto, scarica tutti i file nella loro versione più recente **e** l'intera cronologia dei commit, e configura automaticamente il remote `origin` in modo da poter scaricare e inviare aggiornamenti in futuro.

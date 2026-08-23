---
title: 'Gestione dei pacchetti in Linux'
date: '2026-08-23T00:00:00+01:00'
author: Fabio Mattei
layout: page
---

A differenza di Windows, dove ogni programma si scarica e installa a parte dal sito del produttore, su Linux i programmi si installano quasi sempre tramite un **gestore di pacchetti**: uno strumento che scarica, installa, aggiorna e rimuove software da repository centralizzati e verificati.

### Un gestore diverso per ogni famiglia di distribuzioni

Ogni distribuzione usa il proprio formato di pacchetto e il proprio strumento:

* **APT** (`.deb`) — usato da Debian, Ubuntu e derivate.
* **DNF** (`.rpm`) — usato da Fedora e Red Hat.
* **Pacman** — usato da Arch Linux.

### APT (Debian/Ubuntu)

{% highlight shell %}
sudo apt update              # aggiorna l'elenco dei pacchetti disponibili nei repository
sudo apt upgrade              # aggiorna tutti i pacchetti installati all'ultima versione
sudo apt install firefox      # installa un pacchetto
sudo apt remove firefox       # rimuove un pacchetto (mantiene i file di configurazione)
sudo apt purge firefox        # rimuove un pacchetto e i suoi file di configurazione
sudo apt autoremove           # elimina le dipendenze non più necessarie
{% endhighlight %}

`update` non installa nulla: scarica solo l'elenco aggiornato dei pacchetti disponibili. Va sempre eseguito prima di `upgrade` o `install`, per essere sicuri di ricevere le versioni più recenti.

### DNF (Fedora)

{% highlight shell %}
sudo dnf update                # aggiorna il sistema
sudo dnf install firefox       # installa un pacchetto
sudo dnf remove firefox        # rimuove un pacchetto
{% endhighlight %}

### Pacman (Arch Linux)

{% highlight shell %}
sudo pacman -Syu               # sincronizza i repository e aggiorna il sistema
sudo pacman -S firefox         # installa un pacchetto
sudo pacman -R firefox         # rimuove un pacchetto
{% endhighlight %}

### Cercare un pacchetto

{% highlight shell %}
apt search editor               # APT
dnf search editor                # DNF
pacman -Ss editor                 # Pacman
{% endhighlight %}

### Installare software non presente nei repository

Non tutto il software passa dai repository ufficiali. Alternative comuni:

* **File `.deb` scaricati manualmente** — si installano con `sudo dpkg -i pacchetto.deb`.
* **Snap** e **Flatpak** — formati di pacchetto universali, funzionano allo stesso modo su qualsiasi distribuzione, isolando l'applicazione dal resto del sistema:

{% highlight shell %}
sudo snap install spotify
flatpak install flathub com.spotify.Client
{% endhighlight %}

A differenza dei pacchetti tradizionali, Snap e Flatpak includono al loro interno tutte le librerie di cui l'applicazione ha bisogno, isolate dal resto del sistema (*sandboxing*): il pacchetto è più pesante, ma funziona allo stesso identico modo su qualsiasi distribuzione, senza conflitti con le versioni delle librerie già installate.

### I repository

Un **repository** è un server che ospita i pacchetti che un gestore come APT può scaricare e installare. Ogni distribuzione ha i propri repository ufficiali, elencati in un file di configurazione:

{% highlight shell %}
cat /etc/apt/sources.list
{% endhighlight %}

Oltre ai repository ufficiali, su Ubuntu e derivate è possibile aggiungere un **PPA** (Personal Package Archive), un repository di terze parti che distribuisce versioni più recenti o software non presenti nei repository ufficiali:

{% highlight shell %}
sudo add-apt-repository ppa:nome-del-ppa
sudo apt update
{% endhighlight %}

Aggiungere un PPA comporta però di fidarsi di chi lo mantiene: a differenza dei repository ufficiali, non sempre sono sottoposti agli stessi controlli di qualità e sicurezza.

### Le dipendenze

Molti programmi si appoggiano a librerie condivise per funzionare: queste librerie si chiamano **dipendenze**. Quando si installa un pacchetto, il gestore dei pacchetti calcola automaticamente tutte le dipendenze necessarie e le installa insieme al programma richiesto:

{% highlight shell %}
apt show firefox         # mostra informazioni sul pacchetto, incluse le sue dipendenze
apt depends firefox       # elenca esplicitamente le dipendenze
{% endhighlight %}

Se una dipendenza manca o è in conflitto con un'altra già installata, l'installazione fallisce con un errore esplicito, invece di lasciare il programma in uno stato instabile.

### Gestire versioni specifiche di un pacchetto

{% highlight shell %}
apt list --all-versions firefox        # mostra tutte le versioni disponibili di un pacchetto
sudo apt install firefox=100.0.1        # installa una versione specifica
sudo apt-mark hold firefox               # impedisce che il pacchetto venga aggiornato automaticamente
sudo apt-mark unhold firefox             # rimuove il blocco sull'aggiornamento
{% endhighlight %}

Bloccare la versione di un pacchetto (`hold`) è utile quando un aggiornamento introduce un problema noto o un'incompatibilità con altro software già in uso.

### Compilare software dal codice sorgente

Quando un programma non è disponibile come pacchetto precompilato, resta la possibilità di **compilarlo** direttamente dal codice sorgente. La sequenza classica (per programmi scritti in C/C++) è:

{% highlight shell %}
./configure       # verifica le dipendenze e prepara la compilazione per il sistema corrente
make               # compila il codice sorgente in un programma eseguibile
sudo make install  # copia il programma compilato nelle cartelle di sistema
{% endhighlight %}

Questo metodo richiede di installare manualmente gli strumenti di compilazione (`sudo apt install build-essential` su Debian/Ubuntu) e le eventuali dipendenze di sviluppo. È l'ultima risorsa quando né i repository ufficiali né Snap/Flatpak offrono il software desiderato.

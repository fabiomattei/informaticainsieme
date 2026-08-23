---
title: 'Diagnostica hardware in Linux'
date: '2026-08-23T00:00:00+01:00'
author: Fabio Mattei
layout: page
---

Quando una periferica non funziona, o si vuole semplicemente sapere cosa c'è dentro un computer, Linux offre una serie di strumenti da terminale per interrogare direttamente il kernel e l'hardware rilevato.

### Vedere tutto l'hardware collegato

{% highlight shell %}
lspci        # elenca i dispositivi collegati al bus PCI (schede grafiche, di rete, controller vari)
lsusb         # elenca i dispositivi collegati alle porte USB
lscpu          # mostra informazioni dettagliate sul processore (modello, core, cache)
lsblk           # elenca dischi e partizioni (già visto nella pagina sui dischi)
{% endhighlight %}

Ognuno di questi comandi legge informazioni che il kernel ha già raccolto durante l'avvio o al momento del collegamento del dispositivo, senza bisogno di installare software aggiuntivo nella maggior parte delle distribuzioni.

### Un dettaglio maggiore su un singolo dispositivo

{% highlight shell %}
lspci -v            # informazioni più dettagliate su ogni dispositivo PCI
lspci -k             # mostra anche quale driver del kernel è in uso per ogni dispositivo
lsusb -v              # informazioni dettagliate sui dispositivi USB
{% endhighlight %}

`lspci -k` è particolarmente utile per capire se un dispositivo ha effettivamente un driver caricato: un dispositivo elencato ma senza driver associato è spesso la causa di un malfunzionamento (ad esempio una scheda Wi-Fi che non si connette a nessuna rete).

### Il registro dei messaggi del kernel: dmesg

Il kernel tiene un registro degli eventi legati all'hardware — dispositivi collegati e scollegati, errori dei driver, avvisi — consultabile con:

{% highlight shell %}
dmesg              # mostra tutti i messaggi del kernel dall'avvio del sistema
dmesg | tail -20     # mostra solo gli ultimi 20 messaggi
dmesg -w              # resta in ascolto e mostra i nuovi messaggi in tempo reale
{% endhighlight %}

`dmesg -w`, lasciato aperto in un terminale mentre si collega una nuova periferica (una chiavetta USB, un mouse), mostra in tempo reale come il kernel la riconosce: è spesso il primo strumento a cui rivolgersi quando un dispositivo sembra non essere rilevato affatto.

{% highlight shell %}
dmesg | grep -i usb     # filtra solo i messaggi relativi a dispositivi USB
{% endhighlight %}

### I moduli del kernel

Molte funzionalità hardware sono gestite da **moduli**: piccoli pezzi di codice del kernel che possono essere caricati e rimossi senza riavviare il sistema, invece di essere compilati permanentemente dentro il kernel stesso. Il driver di una scheda di rete o di una scheda grafica è tipicamente distribuito come modulo.

{% highlight shell %}
lsmod                    # elenca tutti i moduli attualmente caricati
sudo modprobe nome_modulo   # carica un modulo
sudo modprobe -r nome_modulo # rimuove un modulo caricato
{% endhighlight %}

Questo permette, ad esempio, di ricaricare il driver di una periferica che si è bloccata senza dover riavviare l'intero computer.

### Memoria, CPU e sensori in tempo reale

Oltre a `top`/`htop` (già visti nella pagina sui [processi]({{ site.baseurl }}{% link _sistemioperativi/linux-processi.md %}.html)) per il carico generale del sistema, alcuni strumenti più specifici:

{% highlight shell %}
free -h            # memoria RAM e swap disponibili e in uso
lscpu               # dettagli sul processore, inclusa la frequenza massima
sensors              # temperature di CPU e altri sensori hardware (richiede il pacchetto lm-sensors)
{% endhighlight %}

`sensors` non è quasi mai installato di default: si ottiene con il gestore di pacchetti (`sudo apt install lm-sensors`), e alla prima esecuzione va configurato con `sudo sensors-detect`, che individua automaticamente quali sensori sono presenti sulla scheda madre.

### Informazioni su schede grafiche e driver video

{% highlight shell %}
lspci -k | grep -A 3 VGA      # mostra la scheda grafica e il driver in uso
glxinfo | grep "OpenGL renderer"   # mostra quale hardware sta effettivamente renderizzando la grafica (richiede mesa-utils)
{% endhighlight %}

Utile in particolare sui portatili con doppia scheda grafica (integrata + dedicata), per verificare quale delle due sta effettivamente lavorando in un dato momento.

### Un esempio pratico: una periferica USB non riconosciuta

Una sequenza tipica di diagnosi quando una chiavetta USB o un altro dispositivo collegato non funziona:

{% highlight shell %}
lsusb                    # il dispositivo compare nell'elenco? Se sì, è almeno rilevato fisicamente
dmesg | tail -20          # cosa ha scritto il kernel al momento del collegamento?
lsmod | grep nome_driver   # il modulo del driver risulta caricato?
{% endhighlight %}

Se il dispositivo non compare nemmeno in `lsusb`, il problema è probabilmente hardware (cavo, porta, periferica difettosa); se compare ma senza un driver caricato, il problema è invece software, e i messaggi di `dmesg` sono di solito il punto di partenza più utile per capire cosa non ha funzionato.

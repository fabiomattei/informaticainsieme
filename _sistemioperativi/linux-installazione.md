---
title: 'Come installare Linux'
date: '2026-08-23T00:00:00+01:00'
author: Fabio Mattei
layout: page
---

Prima di usare i comandi visti nelle altre pagine di questa sezione, serve ovviamente un sistema Linux su cui provarli. Esistono diversi modi per iniziare, con un livello di rischio e di impegno molto diverso l'uno dall'altro.

### Provare Linux senza installarlo: la modalità live

Quasi tutte le distribuzioni permettono di scaricare un'immagine **ISO** e scriverla su una chiavetta USB, ottenendo una **chiavetta live**: avviando il computer da questa chiavetta si può usare Linux direttamente, senza installare nulla sul disco e senza toccare in alcun modo Windows o macOS già presenti. È il modo più sicuro per farsi un'idea di una distribuzione prima di decidere se installarla davvero.

Passaggi generali:

1. Scaricare il file `.iso` della distribuzione scelta dal sito ufficiale (es. ubuntu.com per Ubuntu).
2. Scrivere l'immagine su una chiavetta USB con uno strumento dedicato, come **Rufus** (Windows) o **balenaEtcher** (Windows/macOS/Linux). Attenzione: questa operazione cancella tutto il contenuto della chiavetta.
3. Riavviare il computer e accedere al **boot menu** (il tasto varia per produttore: spesso `F12`, `F10` o `Esc` durante l'accensione) per scegliere di avviare dalla chiavetta USB invece che dal disco interno.
4. Nel menu che compare, scegliere "Try/Prova [distribuzione] senza installare".

In modalità live, ogni modifica ai file viene persa al riavvio: è un ambiente pensato per essere esplorato, non per lavorarci stabilmente.

### Macchina virtuale: un ambiente isolato dentro il sistema attuale

Un'alternativa altrettanto sicura, ma più comoda per un uso prolungato, è installare Linux dentro una **macchina virtuale**: un programma (come **VirtualBox**, gratuito, o **VMware**) che simula un computer completo all'interno di quello reale. Linux gira così in una finestra del sistema operativo già installato, senza mai toccare il disco fisico né i file esistenti.

Vantaggi: massima sicurezza (un errore dentro la macchina virtuale non tocca il sistema reale) e possibilità di avere più distribuzioni installate contemporaneamente. Svantaggio principale: le prestazioni sono inferiori rispetto a un'installazione diretta, perché parte della RAM e della CPU del computer reale viene dedicata a far girare quella virtuale.

### Dual boot: Linux accanto a Windows

Il **dual boot** installa Linux sullo stesso disco già occupato da Windows, in una partizione separata: all'accensione del computer compare un menu che permette di scegliere quale sistema avviare. È la scelta più diffusa per chi vuole usare Linux "sul serio" (con le prestazioni piene dell'hardware) senza rinunciare a Windows per i programmi che lo richiedono.

Punti da conoscere prima di procedere:

* **Fare un backup completo dei propri dati prima di iniziare.** Ridimensionare una partizione esistente per fare spazio a Linux comporta sempre un rischio, per quanto contenuto con gli strumenti moderni.
* **Ridurre la partizione di Windows** per liberare spazio non allocato, tramite lo strumento "Gestione disco" di Windows (visto anche nella pagina [Gestione dei file in Windows]({{ site.baseurl }}{% link _sistemioperativi/windows-gestione-file.md %}.html)) oppure direttamente dall'installer della distribuzione Linux.
* **Disattivare l'avvio rapido di Windows** (Fast Startup) prima di procedere: lascia il disco in uno stato che può confondere l'installer Linux, con il rischio di corrompere il file system di Windows.
* **Verificare la modalità di avvio**, UEFI o Legacy/BIOS: la maggior parte dei computer recenti usa UEFI, e l'installer Linux deve essere avviato nella stessa modalità già usata da Windows, altrimenti il doppio avvio non funzionerà correttamente.

Durante l'installazione, l'installer della distribuzione (ad esempio Ubuntu) rileva automaticamente Windows già presente e propone l'opzione "Installa Ubuntu accanto a Windows", occupandosi da solo di creare le partizioni necessarie e configurare il menu di avvio (**GRUB**), che a ogni accensione permetterà di scegliere quale sistema operativo caricare.

### Installazione su tutto il disco

Se il computer è dedicato interamente a Linux (o i dati esistenti non servono più), l'installer offre anche l'opzione di usare l'intero disco, cancellando qualunque sistema operativo già presente. È il percorso più semplice, ma irreversibile: da usare solo con la certezza che non ci sia nulla da salvare sul disco.

### Dopo l'installazione

Al primo avvio del sistema appena installato, i passaggi tipici sono:

{% highlight shell %}
sudo apt update && sudo apt upgrade    # porta il sistema appena installato all'ultima versione (su Debian/Ubuntu)
{% endhighlight %}

Da qui si può proseguire con quanto visto nelle altre pagine di questa sezione: il [terminale]({{ site.baseurl }}{% link _sistemioperativi/linux-terminale.md %}.html), la [gestione dei pacchetti]({{ site.baseurl }}{% link _sistemioperativi/linux-pacchetti.md %}.html) per installare i programmi necessari, e la [gestione degli utenti]({{ site.baseurl }}{% link _sistemioperativi/linux-utenti.md %}.html) per configurare eventuali altri account.

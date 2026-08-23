---
title: 'Gestione di dischi e mount in Linux'
date: '2026-08-23T00:00:00+01:00'
author: Fabio Mattei
layout: page
---

Come visto nella pagina [Gestione dei file in Linux]({{ site.baseurl }}{% link _sistemioperativi/linux-gestione-file.md %}.html), Linux non usa lettere di unità come Windows: dischi e partizioni vengono resi accessibili tramite il **mount**, cioè agganciati a un punto preciso dell'unica gerarchia di cartelle.

### Vedere i dischi collegati

{% highlight shell %}
lsblk           # elenca dischi e partizioni in forma ad albero
sudo fdisk -l    # mostra informazioni dettagliate su dischi e partizioni
{% endhighlight %}

### Il concetto di mount

Un disco o una partizione, appena collegati, non sono automaticamente accessibili: vanno **montati**, cioè associati a una cartella (detta *punto di mount*) attraverso cui il loro contenuto diventa visibile:

{% highlight shell %}
sudo mount /dev/sdb1 /mnt/usb
{% endhighlight %}

Da questo momento, il contenuto della partizione `/dev/sdb1` è accessibile navigando dentro `/mnt/usb`. Per rendere disponibile una cartella condivisa vuota, prima del comando `mount` la cartella di destinazione deve già esistere:

{% highlight shell %}
sudo mkdir -p /mnt/usb
{% endhighlight %}

Per scollegare in sicurezza un disco (assicurandosi che tutte le scritture pendenti siano completate):

{% highlight shell %}
sudo umount /mnt/usb
{% endhighlight %}

### Il file /etc/fstab

Il mount fatto con il comando `mount` vale solo fino al riavvio del computer. Per montare automaticamente un disco a ogni avvio, occorre aggiungere una riga al file **`/etc/fstab`** (*file system table*):

{% highlight shell %}
/dev/sdb1   /mnt/dati   ext4   defaults   0   2
{% endhighlight %}

Le colonne indicano, nell'ordine: la partizione, il punto di mount, il tipo di file system, le opzioni di montaggio, se includere il file system nel backup con `dump` (raramente usato), e l'ordine di controllo all'avvio con `fsck`.

Un errore in questo file può impedire l'avvio del sistema: è buona norma modificarlo con cautela e verificare la sintassi prima di riavviare, ad esempio provando il mount manualmente con `mount -a`, che monta tutte le voci presenti in `fstab`.

### Spazio libero e utilizzo

{% highlight shell %}
df -h                # spazio libero e occupato su ogni unità montata
du -sh /home/*        # spazio occupato da ciascuna cartella utente
{% endhighlight %}

### Identificare un disco in modo stabile: UUID

Il nome di un disco (`/dev/sdb1`) può cambiare tra un riavvio e l'altro, a seconda dell'ordine con cui le periferiche vengono rilevate. Per questo motivo, in `/etc/fstab` è preferibile identificare i dischi tramite il loro **UUID**, un identificativo univoco che non cambia:

{% highlight shell %}
sudo blkid           # mostra l'UUID di ogni partizione
{% endhighlight %}

{% highlight shell %}
UUID=1234-5678   /mnt/dati   ext4   defaults   0   2
{% endhighlight %}

### Partizionare un disco

Un disco appena collegato, prima di poter essere usato, va diviso in una o più **partizioni**. Lo strumento moderno per farlo è `parted` (o la sua interfaccia interattiva `cfdisk`):

{% highlight shell %}
sudo cfdisk /dev/sdb
{% endhighlight %}

`cfdisk` apre un'interfaccia testuale interattiva che permette di creare, ridimensionare ed eliminare partizioni. Attenzione: modificare le partizioni di un disco già in uso comporta il rischio concreto di perdere i dati esistenti; va sempre fatto con un backup e con la certezza di aver selezionato il disco corretto (`lsblk` prima di procedere aiuta a evitare errori fatali).

### Formattare una partizione

Una volta creata la partizione, va **formattata**, cioè le va assegnato un file system:

{% highlight shell %}
sudo mkfs.ext4 /dev/sdb1     # formatta la partizione con file system ext4
sudo mkfs.vfat /dev/sdb1      # formatta con FAT32, utile per chiavette USB compatibili anche con Windows
{% endhighlight %}

**ext4** è il file system predefinito sulla maggior parte delle distribuzioni Linux, per il suo equilibrio tra affidabilità e prestazioni. Formattare una partizione cancella irrimediabilmente tutti i dati che contiene.

### La swap: memoria virtuale su disco

La **swap** è un'area di disco usata dal kernel come estensione della RAM quando questa si esaurisce: le pagine di memoria meno usate vengono temporaneamente spostate su disco, più lento della RAM ma capace di evitare che il sistema si blocchi per mancanza di memoria.

{% highlight shell %}
free -h              # mostra memoria RAM e swap disponibili e in uso
swapon --show         # mostra le aree di swap attive
{% endhighlight %}

La swap può essere una partizione dedicata oppure, più comunemente oggi, un semplice file:

{% highlight shell %}
sudo fallocate -l 2G /swapfile     # crea un file di 2 GB
sudo chmod 600 /swapfile            # lo rende accessibile solo a root
sudo mkswap /swapfile                # lo prepara come area di swap
sudo swapon /swapfile                 # lo attiva
{% endhighlight %}

Per renderlo attivo automaticamente a ogni avvio, va aggiunta anche una riga a `/etc/fstab`, con lo stesso meccanismo visto per il mount dei dischi.

### Cenni su LVM

**LVM** (Logical Volume Manager) è un livello di astrazione tra le partizioni fisiche e il file system, che permette di gestire lo spazio disco in modo molto più flessibile: più dischi fisici possono essere raggruppati in un unico "volume group", da cui si ritagliano poi "volumi logici" ridimensionabili anche a sistema in funzione, senza dover spostare fisicamente i dati come richiederebbe il ridimensionamento di una partizione tradizionale. È una tecnologia diffusa soprattutto su server, dove la flessibilità nel gestire lo storage nel tempo è più importante che su un normale computer desktop.

{% highlight shell %}
sudo pvs      # mostra i dischi fisici gestiti da LVM (physical volumes)
sudo vgs       # mostra i gruppi di volumi (volume groups)
sudo lvs        # mostra i volumi logici (logical volumes)
{% endhighlight %}

Approfondire LVM va oltre lo scopo di questa introduzione, ma è utile sapere che esiste per non confondersi quando lo si incontra durante l'installazione di una distribuzione, che spesso lo propone come opzione predefinita per il partizionamento del disco.

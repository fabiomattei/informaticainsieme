---
title: 'Gestione di dischi e partizioni in Windows'
date: '2026-08-23T00:00:00+01:00'
author: Fabio Mattei
layout: page
---

Come visto nella pagina [Gestione dei file in Windows]({{ site.baseurl }}{% link _sistemioperativi/windows-gestione-file.md %}.html), ogni disco o partizione viene identificato da una lettera di unità. Lo strumento grafico con cui creare, ridimensionare e formattare queste partizioni si chiama **Gestione disco**.

### Aprire Gestione disco

Si raggiunge digitando `diskmgmt.msc` nella ricerca di Windows, oppure con tasto destro sul pulsante Start → "Gestione disco". La finestra mostra nella parte inferiore una rappresentazione grafica di ogni disco fisico collegato, diviso nelle sue partizioni.

### Dischi di base: MBR e GPT

Quando un disco nuovo viene collegato per la prima volta, va **inizializzato** scegliendo uno tra due schemi di partizionamento:

* **MBR** (Master Boot Record) — lo schema più datato, limitato a 4 partizioni primarie e a dischi fino a 2 TB.
* **GPT** (GUID Partition Table) — lo standard moderno, richiesto dai sistemi con avvio UEFI, senza i limiti di MBR sul numero di partizioni o sulla dimensione del disco.

Sui computer venduti negli ultimi anni, con avvio UEFI, la scelta corretta è quasi sempre GPT.

### Creare una nuova partizione

Cliccando con il tasto destro su uno spazio **non allocato** di un disco, e scegliendo "Nuovo volume semplice", si avvia una procedura guidata che chiede la dimensione della partizione, la lettera di unità da assegnarle e il file system con cui formattarla (in genere **NTFS**).

### Ridurre ed estendere un volume

* **Riduci volume** — libera spazio non allocato togliendolo da una partizione esistente, senza cancellarne il contenuto. È il passaggio necessario, ad esempio, prima di installare Linux in dual boot (vedi [Come installare Linux]({{ site.baseurl }}{% link _sistemioperativi/linux-installazione.md %}.html)).
* **Estendi volume** — aggiunge a una partizione esistente dello spazio non allocato adiacente, disponibile solo se tale spazio si trova immediatamente dopo la partizione da estendere.

### Formattare una partizione

Formattare una partizione la prepara per l'uso assegnandole un file system, cancellando allo stesso tempo tutto il suo contenuto precedente:

* **NTFS** — il file system predefinito di Windows, con supporto a permessi e file di grandi dimensioni.
* **exFAT** — pensato per unità esterne e chiavette USB che devono essere leggibili sia da Windows sia da macOS.
* **FAT32** — più datato, ancora usato per compatibilità con dispositivi molto vecchi, con il limite di file singoli non superiori a 4 GB.

### Cambiare lettera di unità

Cliccando con il tasto destro su una partizione e scegliendo "Cambia lettera e percorsi di unità" è possibile assegnare o modificare la lettera con cui quella partizione compare in Esplora file.

### Pulizia disco e ottimizzazione

Due strumenti di manutenzione, raggiungibili cercandoli nel menu Start:

* **Pulizia disco** — individua ed elimina file temporanei, cache di aggiornamento e altri file non più necessari, per liberare spazio.
* **Deframmenta e ottimizza unità** — su un disco meccanico (HDD) riorganizza i file per velocizzarne la lettura; su un'unità a stato solido (SSD) esegue invece il comando **TRIM**, che segnala all'unità quali blocchi sono liberi, mantenendone le prestazioni nel tempo. Deframmentare manualmente un SSD non è necessario e va evitato: Windows lo riconosce automaticamente e usa TRIM al posto della deframmentazione tradizionale.

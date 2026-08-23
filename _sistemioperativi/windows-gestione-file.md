---
title: 'Gestione dei file in Windows'
date: '2026-08-23T00:00:00+01:00'
author: Fabio Mattei
layout: page
---

Windows organizza i dati in **file** e **cartelle**, mostrati attraverso **Esplora file** (File Explorer), il programma con cui si naviga tra le unità del computer. Questa pagina descrive come Windows gestisce i file dal punto di vista dell'interfaccia grafica; per gli stessi comandi da riga di comando vedi la sezione [Shell Windows (CMD)]({{ site.baseurl }}{% link paginemenu/cmd.md %}).

### Il file system: NTFS

Il file system predefinito di Windows si chiama **NTFS** (New Technology File System). Rispetto ai file system più semplici, NTFS offre:

* **Permessi** — ogni file e cartella può avere permessi diversi per utenti e gruppi diversi (lettura, scrittura, modifica, controllo completo).
* **Journaling** — tiene traccia delle operazioni in corso, così in caso di spegnimento improvviso il file system può ripristinarsi senza corrompersi.
* **Supporto a file di grandi dimensioni** — nessun limite pratico alla dimensione di un singolo file, a differenza dei file system più datati come FAT32.

### Le unità

Windows identifica ogni disco o partizione con una **lettera di unità** (`C:`, `D:`, ecc.). La lettera `C:` è quasi sempre riservata al disco su cui è installato il sistema operativo. Le unità vengono visualizzate in Esplora file sotto la voce "Questo PC".

### Struttura delle cartelle di sistema

Ogni installazione di Windows organizza i file secondo alcune cartelle standard:

* **`C:\Users\NomeUtente`** — la cartella personale di ogni utente, con le sottocartelle Desktop, Documenti, Immagini, Download.
* **`C:\Program Files`** — dove vengono installati i programmi (versione a 64 bit).
* **`C:\Windows`** — i file del sistema operativo stesso; non andrebbe mai modificata manualmente.

### Proprietà e permessi

Cliccando con il tasto destro su un file o una cartella e scegliendo **Proprietà**, si accede a informazioni come dimensione, data di creazione/modifica e attributi (sola lettura, nascosto). Nella scheda **Sicurezza** si possono consultare e modificare i permessi NTFS: quali utenti o gruppi possono leggere, scrivere o eseguire quel file.

### Cestino

Quando un file viene eliminato da Esplora file, non scompare subito: viene spostato nel **Cestino**, da cui può essere ripristinato finché non viene svuotato. L'eliminazione definitiva (senza passare dal Cestino) si ottiene tenendo premuto `Maiusc` mentre si preme `Canc`.

### Ricerca dei file

La barra di ricerca in alto in ogni finestra di Esplora file permette di cercare per nome, ma anche filtrare per tipo, data di modifica o dimensione usando gli appositi filtri disponibili nella scheda **Ricerca**.

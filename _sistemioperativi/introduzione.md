---
title: 'Introduzione ai sistemi operativi'
date: '2026-08-23T00:00:00+01:00'
author: Fabio Mattei
layout: page
---

Un **sistema operativo** (SO) è il software che gestisce le risorse hardware di un computer e offre alle applicazioni un modo uniforme e semplice per usarle. Senza un sistema operativo, ogni programma dovrebbe parlare direttamente con processore, memoria, dischi e periferiche: il SO evita questo, facendo da intermediario tra hardware e software.

### Le funzioni principali

* **Gestione dei processi** — il SO decide quali programmi sono in esecuzione, quando e per quanto tempo ciascuno usa il processore (*scheduling*), permettendo a più applicazioni di funzionare apparentemente in contemporanea anche su un solo core.
* **Gestione della memoria** — assegna a ogni processo un'area di memoria RAM, la protegge dagli altri processi ed elimina i dati quando non servono più.
* **File system** — organizza i dati salvati su disco in file e cartelle, tenendo traccia di dove si trova ogni informazione e di chi può leggerla o modificarla.
* **Gestione delle periferiche** — tramite i *driver*, permette ai programmi di usare tastiera, mouse, stampante, scheda di rete e altri dispositivi senza doverne conoscere i dettagli tecnici.
* **Interfaccia utente** — fornisce un modo per interagire con il computer, sia grafico (GUI, come il desktop di Windows o macOS) sia testuale (riga di comando).

### Spazio utente e spazio kernel

Il cuore del sistema operativo si chiama **kernel**: è la parte che ha accesso diretto e privilegiato all'hardware. I programmi normali (browser, editor di testo, giochi) girano invece in **spazio utente**, con permessi limitati, e devono chiedere al kernel il permesso di usare le risorse tramite delle *system call*. Questa separazione impedisce che un programma difettoso o malevolo possa danneggiare l'intero sistema.

### Le principali famiglie di sistemi operativi

* **Windows** — sviluppato da Microsoft, è il SO più diffuso sui computer desktop. Interfaccia grafica, ampia compatibilità con software commerciale e periferiche.
* **Linux** — kernel open source alla base di moltissime distribuzioni (Ubuntu, Debian, Fedora...). Molto diffuso su server, sistemi embedded e, in versioni personalizzate, su Android.
* **macOS** — sviluppato da Apple per i propri computer, basato su un kernel Unix (Darwin).
* **Sistemi mobili** — Android (basato su kernel Linux) e iOS (basato su Darwin, come macOS) sono a tutti gli effetti sistemi operativi, adattati ai vincoli di smartphone e tablet.

Questi concetti sono la base per approfondire argomenti più specifici: la gestione dei processi, la memoria virtuale, i file system e le differenze pratiche tra le varie famiglie di sistemi operativi.

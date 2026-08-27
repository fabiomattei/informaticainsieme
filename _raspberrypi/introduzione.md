---
title: 'Cos''è il Raspberry Pi'
date: '2026-08-25T00:00:00+01:00'
author: Fabio Mattei
layout: page
---

Il **Raspberry Pi** è un piccolo computer a scheda singola (*single-board computer*), grande quanto una carta di credito, nato nel 2012 nel Regno Unito con l'obiettivo di rendere la programmazione e l'informatica accessibili a scuole e appassionati a basso costo. Oggi è diventato uno degli strumenti più diffusi non solo per l'apprendimento, ma anche per progetti maker, automazione domestica, server personali e prototipazione elettronica.

### Raspberry Pi non è Arduino

Chi arriva da [Arduino]({{ site.baseurl }}{% link _arduino/blink.md %}.html) spesso si aspetta qualcosa di simile, ma la differenza è sostanziale:

* **Arduino** è un **microcontrollore**: esegue un solo programma alla volta, caricato via USB, senza sistema operativo. È pensato per compiti dedicati e in tempo reale (leggere un sensore, controllare un motore), è economico ed è "istantaneo" all'accensione.
* **Raspberry Pi** è un **computer a tutti gli effetti**: ha una CPU multi-core, memoria RAM, si avvia con un sistema operativo (tipicamente basato su Linux) e può eseguire più programmi contemporaneamente, navigare sul web, ospitare un server.

In pratica: Arduino eccelle quando serve controllare hardware in modo semplice e affidabile; Raspberry Pi quando il progetto richiede la potenza e la flessibilità di un computer completo. Molti progetti avanzati, non a caso, li usano insieme: il Raspberry Pi per la logica complessa, Arduino per il controllo diretto e in tempo reale dei componenti elettronici.

### Cosa serve per iniziare

* Una scheda Raspberry Pi (i modelli più recenti sono i più indicati per iniziare).
* Una **scheda microSD** (almeno 16 GB) su cui installare il sistema operativo.
* Un alimentatore adatto al modello posseduto.
* Per la configurazione iniziale: monitor, tastiera e mouse, oppure un altro computer per una configurazione "senza testa" (*headless*, via rete).

### Installare il sistema operativo

Il modo più semplice per iniziare è **Raspberry Pi Imager**, un programma ufficiale scaricabile gratuitamente, che scrive il sistema operativo **Raspberry Pi OS** (basato su Debian Linux) sulla scheda microSD. Durante il processo, Imager permette anche di preconfigurare nome utente, password, rete Wi-Fi e accesso remoto via **SSH**, così da poter avviare la scheda già pronta all'uso, anche senza collegare monitor e tastiera.

### I pin GPIO

La caratteristica che rende il Raspberry Pi interessante per l'elettronica, oltre a essere un computer, è la fila di pin **GPIO** (*General Purpose Input/Output*) lungo un bordo della scheda: 40 piedini che si possono programmare via software per leggere segnali (un pulsante premuto, un sensore) o generarli (accendere un led, attivare un relè). Sono l'equivalente concettuale dei pin digitali di Arduino, ma pilotati da un sistema operativo completo invece che da un programma isolato.

A differenza di Arduino, i pin GPIO del Raspberry Pi lavorano a **3.3V** e non tollerano i 5V: collegare direttamente un componente pensato per 5V può danneggiare la scheda. È il primo dettaglio elettrico da imparare prima di iniziare a collegare qualsiasi cosa.

### Come si programmano

Il linguaggio più usato per controllare i GPIO è **Python**, già installato di default su Raspberry Pi OS, spesso tramite librerie come **gpiozero** o **RPi.GPIO** che rendono semplice leggere e scrivere lo stato dei pin. Nulla vieta di usare altri linguaggi (C, Node.js, e persino codice C++ in stile Arduino tramite librerie compatibili), ma Python resta il punto di partenza più naturale e didattico.

Il prossimo passo naturale è il classico primo progetto: [far lampeggiare un led collegato ai pin GPIO]({{ site.baseurl }}{% link _raspberrypi/blink-gpio.md %}.html).

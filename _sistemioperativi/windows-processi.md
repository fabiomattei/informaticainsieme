---
title: 'Gestione dei processi in Windows'
date: '2026-08-23T00:00:00+01:00'
author: Fabio Mattei
layout: page
---

Ogni programma in esecuzione su Windows corrisponde a uno o più **processi**. Lo strumento principale per osservarli e controllarli dall'interfaccia grafica è **Gestione attività** (*Task Manager*).

### Aprire Gestione attività

Si può aprire in diversi modi:

* Tasto destro sulla barra delle applicazioni → "Gestione attività".
* Combinazione `Ctrl + Shift + Esc`.
* Da `Ctrl + Alt + Canc`, scegliendo "Gestione attività" dal menu che compare.

### La scheda Processi

Mostra tutte le applicazioni e i processi in background attivi, raggruppati per app, con il relativo consumo di CPU, memoria, disco e rete in tempo reale. Cliccando sull'intestazione di una colonna (es. "CPU") si ordinano i processi per quel valore, utile per individuare rapidamente cosa sta rallentando il computer.

Per terminare un processo che non risponde: selezionarlo e cliccare **Termina attività** in basso a destra. È l'equivalente grafico dell'invio di un segnale di terminazione forzata.

### La scheda Prestazioni

Mostra grafici in tempo reale dell'utilizzo di CPU, memoria RAM, disco, rete e (se presente) GPU, utili per capire se un rallentamento dipende da una risorsa satura.

### La scheda Avvio

Elenca i programmi impostati per avviarsi automaticamente all'accensione del computer, con un indicatore del loro impatto sui tempi di avvio. Disabilitare da qui i programmi non necessari è uno dei modi più efficaci per velocizzare l'avvio di Windows.

### La scheda Servizi

I **servizi** sono processi che girano in background senza un'interfaccia visibile, spesso avviati automaticamente dal sistema (ad esempio il servizio di stampa o quello di aggiornamento). Da questa scheda si può vedere quali sono attivi; una gestione più completa (avvio manuale/automatico, arresto) si trova nell'app dedicata **Servizi**, raggiungibile digitando `services.msc` nella ricerca di Windows.

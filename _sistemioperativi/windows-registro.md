---
title: 'Il Registro di sistema in Windows'
date: '2026-08-23T00:00:00+01:00'
author: Fabio Mattei
layout: page
---

Il **Registro di sistema** (*Registry*) è il database gerarchico in cui Windows conserva la maggior parte delle proprie impostazioni: configurazioni del sistema operativo, preferenze degli utenti, opzioni dei programmi installati. Non ha un vero equivalente in Linux, dove le configurazioni sono invece distribuite in singoli file di testo dentro `/etc` e nelle cartelle personali degli utenti.

### Aprire l'Editor del Registro di sistema

Si apre digitando `regedit` nella ricerca di Windows. Richiede permessi di amministratore, perché permette di modificare impostazioni che riguardano l'intero sistema.

### La struttura: chiavi e valori

Il Registro è organizzato come un albero di cartelle, chiamate **chiavi** (*keys*), che possono contenere altre chiavi oppure dei **valori** (*values*): coppie nome/dato, di diversi tipi (testo, numero, sequenza di byte). È concettualmente simile a un file system, con le chiavi al posto delle cartelle e i valori al posto dei file.

### Gli hive principali

Alla radice dell'albero si trovano cinque cartelle principali, dette **hive**:

* **HKEY_CURRENT_USER (HKCU)** — impostazioni dell'utente attualmente collegato: preferenze applicative, sfondo del desktop, configurazioni personali.
* **HKEY_LOCAL_MACHINE (HKLM)** — impostazioni dell'intero computer, valide per tutti gli utenti: driver installati, servizi, configurazione hardware.
* **HKEY_CLASSES_ROOT (HKCR)** — associazioni tra estensioni dei file e programmi che le aprono.
* **HKEY_USERS (HKU)** — le impostazioni di tutti gli utenti registrati sul computer, non solo di quello attivo (HKEY_CURRENT_USER è di fatto un collegamento a una delle sue sottochiavi).
* **HKEY_CURRENT_CONFIG** — informazioni sul profilo hardware in uso all'avvio.

### Cercare una chiave

Il menu **Modifica → Trova** (o `Ctrl+F`) permette di cercare per nome di chiave, valore o dato, utile perché il percorso completo di un'impostazione specifica è quasi sempre difficile da ricordare a memoria.

### Modificare un valore

Facendo doppio clic su un valore se ne può modificare il dato; con il tasto destro su una chiave si possono creare nuove sottochiavi o nuovi valori. I tipi di valore più comuni sono:

* **REG_SZ** — una stringa di testo.
* **REG_DWORD** — un numero intero a 32 bit, spesso usato come interruttore (`0` disattivato, `1` attivato).
* **REG_BINARY** — una sequenza di byte grezzi.

### Fare un backup prima di modificare

Il Registro non ha un Cestino: una modifica sbagliata può rendere instabile o impossibile da avviare il sistema. Prima di intervenire manualmente, è buona norma esportare un backup della chiave che si sta per modificare (o dell'intero Registro):

Menu **File → Esporta**, scegliendo se esportare l'intero Registro o solo il ramo selezionato. Il file `.reg` generato può essere ridato in pasto a `regedit` con un doppio clic per ripristinare le impostazioni salvate.

### Un esempio pratico: disabilitare un programma all'avvio

Molti programmi che si avviano automaticamente con Windows registrano una voce in:

{% highlight text %}
HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run
{% endhighlight %}

Eliminando la voce corrispondente (con backup precauzionale prima di procedere) si impedisce a quel programma di avviarsi automaticamente. Lo stesso risultato, in modo più sicuro, si ottiene oggi dalla scheda **Avvio** di **Gestione attività**, già vista nella pagina [Gestione dei processi in Windows]({{ site.baseurl }}{% link _sistemioperativi/windows-processi.md %}.html): il Registro resta comunque il meccanismo che rende possibile quella funzione.

### Quando (non) usare il Registro

La maggior parte delle impostazioni quotidiane si gestisce oggi dalle Impostazioni di Windows o dal Pannello di controllo, senza mai toccare il Registro direttamente. Modificarlo a mano resta utile per configurazioni avanzate non esposte altrove (o suggerite da guide tecniche affidabili), ma va fatto con cautela: a differenza di un file di configurazione Linux, un errore nel Registro può compromettere l'avvio dell'intero sistema.

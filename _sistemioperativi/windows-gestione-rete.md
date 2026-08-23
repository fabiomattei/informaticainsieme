---
title: 'Gestione della rete in Windows'
date: '2026-08-23T00:00:00+01:00'
author: Fabio Mattei
layout: page
---

Windows offre una serie di strumenti grafici per configurare e monitorare la connessione di rete del computer, senza dover ricorrere alla riga di comando.

### Centro connessioni di rete e condivisione

Il pannello principale da cui gestire la rete è il **Centro connessioni di rete e condivisione**, raggiungibile dalle Impostazioni alla voce "Rete e Internet". Da qui si vede a colpo d'occhio se il computer è connesso, tramite quale rete (Wi-Fi o cavo Ethernet) e con che tipo di accesso (privato, pubblico, senza Internet).

### Adattatori di rete

Ogni interfaccia di rete del computer (scheda Wi-Fi, scheda Ethernet, VPN) è rappresentata da un **adattatore**, visibile in "Modifica opzioni scheda". Da qui è possibile:

* Abilitare o disabilitare un adattatore.
* Rinominarlo.
* Aprirne le proprietà per configurarne i parametri (indirizzo IP, DNS, protocolli attivi).

### Indirizzo IP: automatico o manuale

Nelle proprietà di un adattatore, alla voce **Protocollo Internet versione 4 (TCP/IPv4)**, si può scegliere tra:

* **Ottieni automaticamente un indirizzo IP** — il computer richiede un indirizzo a un server **DHCP** (tipicamente il router), scelta predefinita e adatta alla quasi totalità dei casi.
* **Usa il seguente indirizzo IP** — indirizzo IP, subnet mask, gateway e DNS vengono impostati manualmente; utile per server o dispositivi che devono avere sempre lo stesso indirizzo.

### Rete privata e rete pubblica

Quando ci si collega a una nuova rete, Windows chiede (o determina automaticamente) se si tratta di una rete **privata** (casa, ufficio) o **pubblica** (bar, aeroporto). Questa scelta determina il livello di visibilità del computer: su una rete privata è possibile condividere file e stampanti con gli altri dispositivi, su una rete pubblica queste funzioni restano disattivate per motivi di sicurezza.

### Condivisione di file e stampanti

Per condividere una cartella con altri computer della stessa rete: tasto destro sulla cartella → **Proprietà** → scheda **Condivisione** → **Condivisione avanzata**, dove si sceglie il nome con cui la cartella sarà visibile in rete e i permessi di accesso. Una cartella condivisa è raggiungibile dagli altri computer digitando in Esplora file il suo percorso di rete, nella forma `\\NomeComputer\NomeCondivisione`.

### Unità di rete mappate

Una cartella condivisa può essere collegata a una lettera di unità locale (es. `Z:`) tramite **Mappa unità di rete**, in modo da comparire in "Questo PC" come se fosse un disco del computer stesso, senza doverne digitare ogni volta il percorso.

### Windows Defender Firewall

Il **firewall** integrato in Windows controlla quali connessioni in entrata e in uscita sono permesse. Si gestisce da "Windows Defender Firewall" nel Pannello di controllo, dove è possibile consentire o bloccare il traffico per una specifica app o aprire una porta per un servizio che deve essere raggiungibile dall'esterno.

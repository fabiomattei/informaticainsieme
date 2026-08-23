---
title: 'Concetti base della sicurezza informatica'
date: '2026-08-23T00:00:00+01:00'
author: Fabio Mattei
layout: page
---

![La triade CIA: riservatezza, integrità, disponibilità](/images/sicurezza/concetti-base/concetti-base.svg){:class="aside-image"}

La **sicurezza informatica** (cybersecurity) è l'insieme delle pratiche, tecnologie e processi che proteggono sistemi, reti e dati da accessi non autorizzati, danneggiamenti o interruzioni. Non è un prodotto che si installa una volta per tutte, ma un processo continuo: le minacce cambiano nel tempo, e con esse le difese.

### La triade CIA

Il punto di partenza di ogni ragionamento sulla sicurezza sono tre obiettivi fondamentali, noti come **triade CIA**:

* **Riservatezza (Confidentiality)** — solo le persone autorizzate possono accedere a un dato. Si ottiene con autenticazione, permessi e cifratura.
* **Integrità (Integrity)** — i dati restano accurati e non vengono alterati, né per errore né per manomissione. Si verifica con checksum, firme digitali e controlli di accesso in scrittura.
* **Disponibilità (Availability)** — i dati e i servizi restano accessibili quando servono. Minacciata da guasti, sovraccarichi e attacchi mirati a interrompere un servizio.

Ogni misura di sicurezza — dalla password a un firewall — serve a proteggere almeno uno di questi tre obiettivi. Ogni attacco informatico, allo stesso modo, punta a comprometterne almeno uno.

### Minacce comuni

* **Malware** — software dannoso: virus, worm, trojan, ransomware. Si installa senza il consenso consapevole dell'utente e agisce a favore dell'attaccante.
* **Phishing** — messaggi (email, SMS, chat) che imitano un mittente affidabile per indurre la vittima a rivelare credenziali o dati sensibili, o a scaricare malware.
* **Ingegneria sociale (social engineering)** — manipolazione psicologica di una persona per farle compiere un'azione o rivelare informazioni, aggirando le difese tecniche. Il phishing ne è un caso particolare.
* **Attacco a forza bruta (brute force)** — tentativo sistematico di indovinare una password provando tutte le combinazioni possibili, o le più probabili (dizionario).
* **Denial of Service (DoS/DDoS)** — sovraccarico deliberato di un servizio con richieste, fino a renderlo inutilizzabile per gli utenti legittimi. Compromette la disponibilità.
* **SQL injection e altre injection** — inserimento di codice malevolo negli input di un'applicazione, per farlo eseguire dal sistema che lo riceve senza validarlo.
* **Man in the middle (MITM)** — un attaccante si inserisce nella comunicazione tra due parti, intercettando o alterando i dati scambiati senza che nessuna delle due se ne accorga.

### Principi di difesa

* **Difesa in profondità (defense in depth)** — non affidarsi a un'unica barriera: più livelli di protezione indipendenti (rete, sistema, applicazione, utente) fanno sì che il cedimento di uno non comprometta l'intero sistema.
* **Minimo privilegio (least privilege)** — ogni utente o processo ha solo i permessi strettamente necessari a svolgere il proprio compito, niente di più.
* **Superficie d'attacco (attack surface)** — l'insieme dei punti da cui un sistema può essere attaccato. Meno servizi esposti, meno codice raggiungibile dall'esterno, meno punti da difendere.
* **Aggiornamenti e patch** — gran parte degli attacchi sfrutta vulnerabilità già note e già corrette dai produttori: mantenere il software aggiornato chiude queste porte prima che vengano sfruttate.

Questi concetti sono la base su cui si costruisce ogni argomento più specifico: dalla crittografia alla sicurezza delle applicazioni web, fino alle buone pratiche di autenticazione.

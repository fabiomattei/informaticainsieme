---
title: 'Utenti e account in Windows'
date: '2026-08-23T00:00:00+01:00'
author: Fabio Mattei
layout: page
---

Come Linux, anche Windows è un sistema **multiutente**: più persone possono usare lo stesso computer, ciascuna con la propria cartella personale, le proprie impostazioni e i propri permessi.

### Tipi di account

* **Account amministratore** — può installare software, modificare impostazioni di sistema e gestire gli altri account.
* **Account standard** — può usare il computer e i propri file, ma non modificare impostazioni che riguardano tutti gli utenti né installare la maggior parte dei programmi senza l'approvazione di un amministratore.
* **Account Microsoft** — collegato a un indirizzo email, sincronizza impostazioni e file tra più dispositivi tramite OneDrive.
* **Account locale** — esiste solo su quel computer, senza collegamento a un account online.

### Gestire gli account dalle Impostazioni

Da **Impostazioni → Account → Famiglia e altri utenti** è possibile aggiungere un nuovo utente, scegliere se account Microsoft o locale, e assegnargli il ruolo di amministratore o standard.

### Controllo account utente (UAC)

Il **Controllo account utente** (User Account Control) è la finestra che compare quando un'operazione richiede privilegi di amministratore, chiedendo conferma esplicita prima di procedere. Serve a impedire che programmi malevoli o errori dell'utente modifichino il sistema senza un'approvazione consapevole — è l'equivalente concettuale di `sudo` su Linux, ma con richiesta di conferma tramite finestra invece che tramite password digitata.

### Gestione utenti e gruppi locali (strumento avanzato)

Sulle edizioni Pro ed Enterprise di Windows è disponibile lo strumento **Gestione utenti e gruppi locali** (`lusrmgr.msc`), che permette di:

* Creare, disabilitare o eliminare account utente.
* Creare gruppi personalizzati e assegnare utenti a più gruppi contemporaneamente.
* Impostare criteri sulla password (scadenza, complessità richiesta).

### Permessi sui file per utente

I permessi NTFS (già visti nella pagina [Gestione dei file in Windows]({{ site.baseurl }}{% link _sistemioperativi/windows-gestione-file.md %}.html)) si applicano proprio a livello di singolo utente o gruppo: dalla scheda **Sicurezza** delle proprietà di un file o cartella si può scegliere esattamente quali utenti hanno accesso e con quali permessi (lettura, scrittura, controllo completo).

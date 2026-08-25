---
title: 'Phishing: come funziona e come riconoscerlo'
date: '2026-08-25T00:00:00+01:00'
author: Fabio Mattei
layout: page
---

Non serve bucare un server per rubare una password: basta chiedere gentilmente, fingendosi qualcun altro. È l'idea alla base del **phishing**, la tecnica di attacco informatico più diffusa in assoluto, perché non sfrutta una falla nel software ma una falla molto più difficile da correggere: la fiducia delle persone.

### Cos'è il phishing

Il phishing è un tentativo di **ingegneria sociale**: un messaggio — email, SMS, chat, telefonata — costruito per sembrare legittimo (una banca, un corriere, un collega, un servizio online) allo scopo di indurre la vittima a compiere un'azione dannosa: cliccare un link, scaricare un allegato infetto, inserire credenziali su un sito falso, o effettuare un pagamento. Il nome gioca sull'assonanza con *fishing* (pescare): si lancia l'amo a molte persone, sapendo che basta che pochissime abbocchino.

### Le varianti principali

* **Email phishing** — la forma classica: milioni di email identiche, inviate a caso, che imitano banche, corrieri o piattaforme note.
* **Spear phishing** — attacco mirato a una persona o azienda specifica, costruito con informazioni raccolte in anticipo (nome del capo, progetti in corso, colleghi reali) per risultare molto più credibile.
* **Whaling** — spear phishing rivolto a dirigenti o figure di alto profilo ("pesci grossi"), spesso per autorizzare bonifici o accessi a dati riservati.
* **Smishing** — phishing via SMS, tipicamente finti avvisi di consegna pacchi o blocchi del conto corrente.
* **Vishing** — phishing telefonico (*voice phishing*), spesso un finto operatore della banca che chiede un codice OTP appena ricevuto via SMS.
* **Clone phishing** — copia quasi identica di un'email legittima già ricevuta in passato, con il link o l'allegato originale sostituito da uno malevolo.

### Perché funziona

Un buon messaggio di phishing non attacca il computer, attacca il comportamento umano, facendo leva su:

* **Urgenza** — "il tuo account verrà sospeso entro 24 ore", per spingere ad agire senza riflettere.
* **Paura** — "abbiamo rilevato un accesso sospetto", per generare panico immediato.
* **Autorità** — un mittente che finge di essere il capo, la banca o un ente pubblico, contando sul fatto che si tende a non mettere in discussione chi sembra avere un ruolo superiore.
* **Curiosità o desiderio** — "hai vinto un premio", "guarda questo video", leve più semplici ma ancora efficaci.

### I segnali da riconoscere

Nessun indizio da solo è una prova definitiva, ma la combinazione di più segnali deve mettere in allerta:

1. **Mittente sospetto**: l'indirizzo email reale (non solo il nome visualizzato) corrisponde davvero al dominio ufficiale dell'azienda? Un dominio quasi identico ma con un carattere diverso (`amaz0n.com`, `paypal-secure.com`) è un classico trucco.
2. **Link non corrispondente**: passando il mouse sopra un link (senza cliccare) si vede l'URL reale in basso nel browser o client email: se non coincide col testo mostrato, è un allarme.
3. **Senso di urgenza artificiale**: le organizzazioni serie non minacciano di solito la chiusura immediata di un account via email.
4. **Richiesta di dati che non dovrebbero essere chiesti così**: nessuna banca chiede la password intera o un codice OTP per telefono o email.
5. **Errori di lingua o formattazione**: loghi sgranati, saluti generici ("Gentile cliente" invece del proprio nome), errori grammaticali — sempre meno frequenti con l'uso dell'IA generativa, ma ancora un indizio utile.
6. **Allegati inattesi**: file `.zip`, `.exe` o macro di Office non richiesti, specialmente da mittenti sconosciuti.

### Cosa fare se si riceve un tentativo di phishing

* Non cliccare link né aprire allegati.
* Verificare l'informazione contattando l'ente reale tramite un canale ufficiale conosciuto (sito digitato a mano, numero verde), mai rispondendo al messaggio o usando i contatti presenti in esso.
* Se si è già inserita una password su un sito falso, cambiarla immediatamente ovunque venga riutilizzata, e attivare l'autenticazione a più fattori.
* Segnalare il messaggio come phishing al proprio provider email o all'ufficio IT, invece di limitarsi a cancellarlo: aiuta a bloccare l'attacco anche per altri.

### Le difese tecniche

Oltre all'attenzione personale, esistono strumenti tecnici che riducono il rischio: filtri antispam basati su reputazione del mittente, protocolli come **SPF**, **DKIM** e **DMARC** che permettono ai server di posta di verificare se un'email proviene davvero dal dominio dichiarato, e l'**autenticazione a più fattori (MFA)**, che rende inutile una password rubata se manca anche il secondo fattore di accesso. Nessuno di questi strumenti è però infallibile da solo: restano un complemento alla consapevolezza dell'utente, non un sostituto, perché come per la [disinformazione]({{ site.baseurl }}{% link _sicurezza/disinformazione.md %}.html), l'anello più debole della catena resta spesso la persona davanti allo schermo, non la tecnologia.

---
title: 'Password e autenticazione: come proteggere davvero un account'
date: '2026-08-25T00:00:00+01:00'
author: Fabio Mattei
layout: page
---

Una password è spesso l'unica cosa che separa un account da chiunque voglia entrarci al posto nostro. Eppure è anche la misura di sicurezza più trascurata: scelta di fretta, riusata ovunque, mai cambiata. Capire come funziona davvero l'autenticazione — e perché "una password" spesso non basta più — è il completamento pratico di quanto visto a proposito di [phishing]({{ site.baseurl }}{% link _sicurezza/phishing.md %}.html): un attaccante che ruba una password non ottiene nulla, se dietro c'è una buona gestione degli accessi.

### Cosa rende una password debole

* **Corta**: ogni carattere in più moltiplica il numero di combinazioni possibili; una password di 8 caratteri si può forzare (*brute force*) in tempi ormai brevissimi con hardware moderno, una di 16 caratteri no.
* **Prevedibile**: nomi, date di nascita, sequenze di tastiera (`qwerty`, `123456`) o parole comuni sono le prime che un attaccante prova, spesso con liste (*dictionary attack*) costruite proprio su questi schemi.
* **Riusata**: la stessa password su più siti è il rischio più sottovalutato. Se un solo sito subisce una violazione (*data breach*) e le password rubate finiscono online, un attaccante le prova automaticamente su altri servizi — un attacco noto come **credential stuffing** — sperando che qualcuno l'abbia riutilizzata.

### Cosa rende una password forte

Non serve una sequenza illeggibile di simboli impossibile da ricordare: la lunghezza conta più della complessità. Un metodo efficace e usabile è la **passphrase**: una sequenza di parole casuali e non correlate tra loro, ad esempio `cavalloTramontoBiciclettaVulcano42`, molto più lunga di una password tradizionale e quindi più difficile da forzare, ma comunque memorizzabile.

Le regole essenziali restano poche:

1. **Lunghezza** — almeno 12-16 caratteri, meglio se di più.
2. **Unicità** — una password diversa per ogni servizio, così che una violazione ne comprometta solo uno.
3. **Nessuna informazione personale** — niente nomi, date, squadre del cuore: sono le prime cose che un attaccante che ci conosce (o ha guardato i nostri social) prova.
4. **Cambio solo se necessario** — cambiarla periodicamente senza motivo, come richiedevano molte policy aziendali in passato, si è rivelato controproducente: spinge le persone a scegliere password più deboli o a variarle di poco. Meglio cambiarla subito in caso di sospetta violazione.

### Il problema con "ricordarsele tutte"

Rispettare tutte queste regole per decine di account è impossibile a mente. È qui che entrano in gioco i **password manager**: programmi che generano password lunghe e casuali per ogni sito e le custodiscono cifrate dietro un'unica password principale (*master password*), l'unica che serve davvero ricordare. Non è una misura opzionale per esperti: è oggi la strada più realistica per avere password davvero uniche e complesse su ogni servizio, senza doverle ricordare tutte.

### Un secondo fattore, per sicurezza

Anche la password più forte può essere rubata: con il phishing, con un data breach su un sito terzo, con un malware che registra i tasti premuti (*keylogger*). Per questo esiste l'**autenticazione a più fattori (MFA)**, che richiede, oltre alla password, qualcosa che solo il legittimo proprietario possiede o è:

* **Qualcosa che sai** — la password stessa.
* **Qualcosa che hai** — un'app di autenticazione (che genera codici temporanei, i **TOTP**), una chiave fisica di sicurezza, o un codice ricevuto via SMS.
* **Qualcosa che sei** — impronta digitale, riconoscimento facciale (biometria).

Anche se un attaccante ottiene la password, senza il secondo fattore non può accedere. Tra i metodi elencati, l'SMS è il più debole (vulnerabile a un attacco noto come **SIM swapping**, in cui l'attaccante convince l'operatore telefonico a trasferire il numero su una propria SIM), mentre app di autenticazione e chiavi fisiche offrono una protezione molto più solida.

### L'evoluzione: le passkey

La tendenza più recente va oltre la password: le **passkey**, basate sullo standard **FIDO2/WebAuthn**, sostituiscono del tutto la password con una coppia di chiavi crittografiche generata dal dispositivo (telefono, computer). Non c'è nulla da ricordare né da digitare, e non esiste un "segreto condiviso" che un sito possa perdere in un data breach, perché la chiave privata non lascia mai il dispositivo dell'utente. Sempre più servizi le stanno adottando come alternativa, o affiancamento, alla password tradizionale.

### Cosa fare in pratica

* Usare un password manager e lasciargli generare password uniche per ogni servizio.
* Attivare l'MFA ovunque sia disponibile, preferendo app di autenticazione o chiavi fisiche all'SMS.
* Verificare periodicamente se le proprie credenziali sono comparse in violazioni note, con servizi come *Have I Been Pwned*.
* Non condividere mai una password né un codice OTP per telefono, email o chat: nessun servizio legittimo lo chiede in questo modo, come visto anche a proposito del [phishing]({{ site.baseurl }}{% link _sicurezza/phishing.md %}.html).

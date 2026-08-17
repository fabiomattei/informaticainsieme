---
title: Database
date: '2020-02-10T06:20:07+01:00'
author: Fabio Mattei
layout: page
---

![Tabelle di un database collegate da chiavi](/images/database/database/database.svg){:class="aside-image"}

L’archiviazione e il reperimento delle informazioni rappresentano una delle maggiori risposte che l’informatica ha saputo dare alle aziende e alle istituzioni.

Possiamo soltanto immaginare come fossero gli archivi delle banche o degli istituti anagrafici fino a pochi decenni fa e possiamo soltanto rabbrividire pensando al lavoro richiesto per generare un estratto conto con le transazioni degli ultimi 5 anni prima dell’invenzione dei database. Faldoni di carta, schedari, timbri e impiegati che ricopiavano a mano gli stessi dati su più registri: un lavoro lento, costoso e pieno di occasioni per commettere errori.

### Le caratteristiche di una base di dati

Una base di dati nasce per rispondere a esigenze molto concrete, che un semplice archivio cartaceo — o un moderno foglio di calcolo — fatica a soddisfare:

* **Grandi quantità di dati** — un database deve poter gestire milioni o miliardi di record senza perdere in velocità.
* **Condivisione** — più utenti e più programmi devono poter accedere contemporaneamente agli stessi dati, senza pestarsi i piedi a vicenda.
* **Persistenza** — i dati devono sopravvivere alla chiusura del programma che li ha creati e restare disponibili nel tempo, anche a distanza di anni.
* **Affidabilità** — le informazioni non devono corrompersi né andare perse, nemmeno in caso di guasti hardware o interruzioni impreviste.
* **Efficienza** — il recupero di un’informazione precisa, magari tra milioni di righe, deve avvenire in tempi ragionevoli.

### Perché non basta un file di testo o un foglio Excel?

Chiunque abbia provato a gestire un archivio con un foglio di calcolo conosce i problemi che nascono quando i dati crescono:

* **Ridondanza** — lo stesso dato viene ripetuto in più punti. Se l’indirizzo di un cliente compare su ogni suo ordine, quel dato è scritto decine di volte;
* **Anomalie di aggiornamento** — se il cliente cambia indirizzo, bisogna ricordarsi di modificarlo ovunque compaia. Basta dimenticare una riga per ritrovarsi con dati incoerenti;
* **Accesso concorrente** — se due persone aprono e modificano lo stesso file contemporaneamente, una delle due rischia di sovrascrivere il lavoro dell’altra;
* **Integrità** — nulla impedisce di inserire un’età negativa o una data inesistente: il file non conosce le regole del mondo reale che dovrebbe rappresentare;
* **Sicurezza** — è difficile stabilire con precisione chi può leggere o modificare quali dati.

Un **DBMS** (Database Management System, cioè un software come SQLite, MySQL, PostgreSQL o Oracle) risolve questi problemi mettendo a disposizione una struttura rigorosa, dei vincoli sui dati, meccanismi per gestire l’accesso concorrente e permessi per stabilire chi può fare cosa. È importante non confondere i due termini: il **database** è l’insieme organizzato dei dati, il **DBMS** è il software che lo gestisce.

### Come si studia un database

Lo studio dei database segue un percorso preciso: si parte dalla **progettazione concettuale**, cioè dal capire quali informazioni vogliamo memorizzare e come sono collegate tra loro, per poi passare alla **realizzazione pratica** usando il linguaggio SQL. Questo percorso si articola in tre fasi:

1. **Modellazione** — Si disegna il modello entità-relazione (ER), uno schema visivo che descrive le entità del problema reale (ad esempio clienti, prodotti, ordini) e i legami che le uniscono, indipendentemente da come verranno poi implementate.
2. **Traduzione** — Il modello ER viene convertito in tabelle relazionali: ogni entità diventa una tabella, ogni legame viene realizzato tramite chiavi primarie e chiavi esterne che collegano le righe delle diverse tabelle tra loro.
3. **Interrogazione** — Si usa SQL per creare concretamente le tabelle, inserire i dati e recuperarli con query anche molto complesse, che filtrano, ordinano, raggruppano e combinano le informazioni provenienti da più tabelle.

Un buon progetto segue sempre quest’ordine: una tabella progettata male è difficile da correggere una volta che contiene migliaia di righe, mentre un errore sullo schema ER si corregge in pochi minuti con una gomma.

### Strumenti

Esistono decine di sistemi di gestione di basi di dati, dai grandi motori client-server usati dalle aziende (MySQL, PostgreSQL, Oracle, SQL Server) ai motori più leggeri pensati per essere incorporati in un’applicazione. Gli esempi pratici di questo capitolo usano **SQLite**, un database leggero che salva tutti i dati in un singolo file, senza bisogno di installare un server separato. È lo stesso motore usato da Firefox, Android, WhatsApp e molte altre applicazioni diffuse: semplice da usare per imparare, ma abbastanza potente da essere impiegato in produzione. Per chi preferisce un ambiente grafico anziché scrivere query a mano, viene presentato anche **MS Access**.

Gli argomenti del nostro studio saranno:

- [Il modello entità relazione]({{ site.baseurl }}{% link _database/modello-entita-relazione.md %}.html) — come progettare la struttura del database su carta, prima ancora di scrivere una riga di codice.
- [La relazione]({{ site.baseurl }}{% link _database/la-relazione.md %}.html) — come i legami tra entità diventano collegamenti reali tra tabelle, tramite chiavi primarie ed esterne.
- [Il linguaggio SQL]({{ site.baseurl }}{% link _database/il-linguaggio-sql.md %}.html) — i comandi per creare le tabelle, inserire, modificare, cancellare e interrogare i dati.
- [Le forme normali]({{ site.baseurl }}{% link _database/forme-normali.md %}.html) — le regole per progettare tabelle senza ridondanze né anomalie di aggiornamento.
- [MS Access]({{ site.baseurl }}{% link _database/ms-access.md %}.html) — un ambiente grafico per creare e interrogare database senza scrivere SQL.
- [SQLite e python]({{ site.baseurl }}{% link _database/sqlite-e-python.md %}.html) — come collegare un database ai propri programmi Python.
- [MySQL]({{ site.baseurl }}{% link _database/mysql.md %}.html) — un database client-server, per applicazioni con più utenti connessi in rete.
</content>

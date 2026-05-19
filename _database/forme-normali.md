---
title: 'Forme normali'
date: '2026-05-19T00:00:00+01:00'
author: Fabio Mattei
layout: page
---

Le **forme normali** sono un insieme di regole per valutare la qualità di un database relazionale. Seguirle riduce la **ridondanza** — lo stesso dato memorizzato in più posti — e previene le **anomalie** che ne derivano.

### Le anomalie della ridondanza

Quando un dato è duplicato in più righe possono verificarsi tre tipi di anomalie:

- **Anomalia di aggiornamento**: modificare un dato richiede di aggiornare più righe. Se ne viene dimenticata una il database diventa inconsistente.
- **Anomalia di inserimento**: non si riesce ad aggiungere un'informazione senza aggiungerne un'altra non correlata.
- **Anomalia di cancellazione**: eliminare una riga cancella per effetto collaterale informazioni che dovevano essere conservate.

Le forme normali eliminano queste anomalie imponendo regole precise sulla struttura delle tabelle. Le tre forme più importanti sono la prima (1FN), la seconda (2FN) e la terza (3FN), ciascuna più restrittiva della precedente.

### Prima forma normale (1FN)

**Regola**: ogni cella deve contenere un valore **atomico**, cioè non ulteriormente divisibile. Non sono ammessi elenchi, insiemi o valori ripetuti all'interno della stessa colonna.

**Esempio di violazione:**

| IdStudente | Nome | Telefoni |
|---|---|---|
| 1 | Mario Rossi | 0123456, 9876543 |
| 2 | Giulia Bianchi | 1112223 |

La colonna `Telefoni` contiene più valori nella stessa cella. È impossibile cercare "tutti gli studenti con numero 9876543" senza analizzare il testo colonna per colonna.

**Tabelle in 1FN:**

Ogni valore occupa una riga propria. Se uno studente ha più telefoni si crea una tabella dedicata:

STUDENTI(IdStudente, Nome)

| IdStudente | Nome |
|---|---|
| 1 | Mario Rossi |
| 2 | Giulia Bianchi |

TELEFONI(IdStudente\*, Telefono)

| IdStudente | Telefono |
|---|---|
| 1 | 0123456 |
| 1 | 9876543 |
| 2 | 1112223 |

### Seconda forma normale (2FN)

**Regola**: la tabella deve essere in 1FN e ogni attributo non chiave deve dipendere dall'**intera chiave primaria**, non solo da una sua parte. Questa regola si applica alle tabelle la cui chiave primaria è composta da più colonne.

**Esempio di violazione:**

Supponiamo di registrare i voti degli studenti nelle materie:

ESAMI(IdStudente, IdMateria, NomeMateria, Docente, Voto)

La chiave primaria è **(IdStudente, IdMateria)**. Analizziamo le dipendenze:

- `Voto` dipende da (IdStudente, IdMateria) — dipendenza completa ✓
- `NomeMateria` dipende solo da IdMateria — **dipendenza parziale** ✗
- `Docente` dipende solo da IdMateria — **dipendenza parziale** ✗

| IdStudente | IdMateria | NomeMateria | Docente | Voto |
|---|---|---|---|---|
| 1 | 10 | Matematica | Prof. Rossi | 8 |
| 1 | 20 | Italiano | Prof. Bianchi | 7 |
| 2 | 10 | Matematica | Prof. Rossi | 6 |
| 2 | 20 | Italiano | Prof. Bianchi | 9 |

**Anomalia**: se il docente di Matematica cambia, bisogna aggiornare ogni riga che contiene IdMateria = 10. Dimenticarne anche una sola produce un'inconsistenza.

**Tabelle in 2FN:**

Gli attributi con dipendenza parziale vengono spostati in una tabella separata con la chiave da cui dipendono:

MATERIE(IdMateria, NomeMateria, Docente)

| IdMateria | NomeMateria | Docente |
|---|---|---|
| 10 | Matematica | Prof. Rossi |
| 20 | Italiano | Prof. Bianchi |

ESAMI(IdStudente\*, IdMateria\*, Voto)

| IdStudente | IdMateria | Voto |
|---|---|---|
| 1 | 10 | 8 |
| 1 | 20 | 7 |
| 2 | 10 | 6 |
| 2 | 20 | 9 |

Ora cambiare il docente di Matematica richiede una sola modifica nella tabella MATERIE.

### Terza forma normale (3FN)

**Regola**: la tabella deve essere in 2FN e nessun attributo non chiave deve dipendere da un altro attributo non chiave. Si parla in questo caso di **dipendenza transitiva**: A → B → C, dove A è la chiave primaria, B e C sono attributi non chiave.

**Esempio di violazione:**

FORNITORI(IdFornitore, NomeFornitore, Città, Regione)

La chiave primaria è **IdFornitore**. Analizziamo le dipendenze:

- `NomeFornitore` dipende da IdFornitore ✓
- `Città` dipende da IdFornitore ✓
- `Regione` dipende da **Città**, non dalla chiave primaria — **dipendenza transitiva** ✗

| IdFornitore | NomeFornitore | Città | Regione |
|---|---|---|---|
| 1 | Rossi Srl | Roma | Lazio |
| 2 | Bianchi SpA | Milano | Lombardia |
| 3 | Verdi & Co | Roma | Lazio |

**Anomalia**: la coppia Roma → Lazio è ripetuta in più righe. Se per errore una riga viene aggiornata con una regione diversa il database diventa inconsistente senza che nulla lo segnali.

**Tabelle in 3FN:**

L'attributo con dipendenza transitiva viene estratto in una tabella separata, con come chiave l'attributo da cui dipende:

CITTÀ(Città, Regione)

| Città | Regione |
|---|---|
| Roma | Lazio |
| Milano | Lombardia |

FORNITORI(IdFornitore, NomeFornitore, Città\*)

| IdFornitore | NomeFornitore | Città |
|---|---|---|
| 1 | Rossi Srl | Roma |
| 2 | Bianchi SpA | Milano |
| 3 | Verdi & Co | Roma |

Ora la coppia Città → Regione esiste in un solo posto e non può diventare inconsistente.

### Riepilogo

| Forma normale | Requisiti aggiuntivi rispetto alla precedente |
|---|---|
| 1FN | Tutti i valori sono atomici; niente gruppi ripetuti |
| 2FN | Ogni attributo non chiave dipende dall'intera chiave primaria |
| 3FN | Nessun attributo non chiave dipende da un altro attributo non chiave |

Il processo di portare un database alle forme normali si chiama **normalizzazione**. La 3FN è il traguardo pratico per la maggior parte dei database reali.

### Esercizi

**Esercizio 1**

La seguente tabella registra i prestiti di una biblioteca:

PRESTITI(IdPrestito, NomeUtente, EmailUtente, TitoloLibro, Autore, CasaEditrice, DataPrestito)

1. La tabella è in 1FN? Motiva la risposta.
2. Identifica le dipendenze parziali, se esistono (la chiave primaria è IdPrestito).
3. Identifica le dipendenze transitive.
4. Normalizza la tabella fino alla 3FN mostrando le tabelle risultanti con chiavi primarie ed esterne.

**Esercizio 2**

La seguente tabella traccia gli ordini di un negozio online:

ORDINI(IdOrdine, DataOrdine, NomeCliente, CittàCliente, CapCliente, IdProdotto, NomeProdotto, Categoria, Quantità, PrezzoUnitario)

La chiave primaria è **(IdOrdine, IdProdotto)**.

1. Identifica tutte le dipendenze parziali (violazioni 2FN).
2. Identifica tutte le dipendenze transitive (violazioni 3FN).
3. Normalizza fino alla 3FN mostrando le tabelle risultanti con chiavi primarie ed esterne.

**Esercizio 3 — Violazione della 1FN**

La seguente tabella registra i dipendenti di un'azienda con le loro competenze informatiche:

| IdDipendente | Nome | Cognome | Ufficio | Competenze |
|---|---|---|---|---|
| 1 | Marco | Ferretti | Vendite | Word, Excel, PowerPoint |
| 2 | Anna | Conti | IT | Python, SQL, Linux |
| 3 | Luigi | Amato | Vendite | Word, PowerPoint |

1. Perché questa tabella non è in 1FN?
2. Descrivi l'anomalia concreta che si verifica se vuoi cercare tutti i dipendenti che conoscono Excel.
3. Normalizza la tabella in 1FN mostrando le tabelle risultanti.
4. Scrivi i comandi CREATE TABLE SQL per le tabelle ottenute.

**Esercizio 4 — In che forma normale si trova?**

Analizza la seguente tabella che registra le visite mediche:

VISITE(IdVisita, DataVisita, IdPaziente, NomePaziente, CognomePaziente, CodiceFiscale, IdMedico, NomeMedico, Specializzazione, Diagnosi)

La chiave primaria è **IdVisita**.

1. La tabella è in 1FN? Motiva la risposta.
2. La tabella è in 2FN? Motiva la risposta.
3. La tabella è in 3FN? Identifica le dipendenze transitive presenti.
4. Normalizza fino alla 3FN mostrando le tabelle risultanti.

**Esercizio 5 — Anomalie su dati concreti**

La seguente tabella registra i risultati di un campionato di calcio:

PARTITE(CodiceSquadra, NomeSquadra, Città, Regione, Allenatore, DataPartita, Avversario, Risultato)

La chiave primaria è **(CodiceSquadra, DataPartita)**.

| CodiceSquadra | NomeSquadra | Città | Regione | Allenatore | DataPartita | Avversario | Risultato |
|---|---|---|---|---|---|---|---|
| LAZ | Lazio | Roma | Lazio | Baroni | 15/09/2024 | Juventus | 2-1 |
| LAZ | Lazio | Roma | Lazio | Baroni | 22/09/2024 | Milan | 1-1 |
| JUV | Juventus | Torino | Piemonte | Thiago Motta | 15/09/2024 | Lazio | 1-2 |
| MIL | Milan | Milano | Lombardia | Fonseca | 22/09/2024 | Lazio | 1-1 |

1. Mostra un esempio concreto di **anomalia di aggiornamento** usando i dati della tabella.
2. Mostra un esempio concreto di **anomalia di inserimento**: cosa succede se vuoi registrare una nuova squadra che non ha ancora giocato nessuna partita?
3. Mostra un esempio concreto di **anomalia di cancellazione**: cosa si perde cancellando l'unica partita del Milan?
4. Identifica le dipendenze parziali e transitive presenti.
5. Normalizza fino alla 3FN.

**Esercizio 6 — Chiave composta con più attributi**

La seguente tabella rappresenta l'orario scolastico settimanale:

ORARIO(Giorno, Ora, IdAula, NomeAula, Capienza, IdDocente, NomeDocente, IdMateria, NomeMateria, Classe)

La chiave primaria è **(Giorno, Ora, IdAula)**: identifica univocamente ogni lezione (in un dato momento in una data aula può esserci una sola lezione).

1. Elenca tutte le dipendenze parziali dalla chiave composta.
2. Dopo aver eliminato le dipendenze parziali, verifica se le tabelle ottenute sono in 3FN.
3. Normalizza l'intero schema fino alla 3FN mostrando le tabelle risultanti con chiavi primarie ed esterne.
4. Scrivi i comandi CREATE TABLE SQL per tutte le tabelle.

**Esercizio 7 — Progettazione da descrizione**

Un'agenzia di viaggi vuole gestire le prenotazioni dei propri clienti. I requisiti sono:

- Ogni **cliente** ha nome, cognome, email e città di residenza.
- Ogni **viaggio** ha una destinazione, una data di partenza, una data di ritorno e un prezzo base.
- Ogni **prenotazione** collega un cliente a un viaggio e registra la data in cui è stata effettuata.
- Una prenotazione può includere più **passeggeri** (il cliente stesso e altri componenti del gruppo); di ogni passeggero si registrano nome, cognome e data di nascita.

1. Progetta lo schema in 3FN partendo da questi requisiti. Per ogni tabella indica nome, attributi, chiave primaria e chiavi esterne.
2. Verifica esplicitamente che nessuna delle tue tabelle violi la 2FN o la 3FN.
3. Scrivi i comandi CREATE TABLE SQL per tutte le tabelle.

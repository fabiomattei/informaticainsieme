---
title: 'MS Access'
date: '2020-02-22T05:29:00+01:00'
author: Fabio Mattei
layout: page
---

Access è un software creato dalla Microsoft che ci permette di creare dei database personali. Il database risiede in un file e dunque per sua natura non può essere utilizzato da più persone contemporaneamente. Risulta però un comodo strumento per chi deve gestire dei dati da solo o sta iniziando ad usare i database.

#### Creiamo un nuovo database

Per creare un database in access è sufficiente creare un nuovo file.

![Nuovo file in access](/images/database/access/access_nuovofile.jpg)

#### Creiamo una tabella

![Nuova tabella in access](/images/database/access/access_creatabella.jpg)

Per creare una tabella bisogna cliccare sul menù crea quindi cliccare su tabella. Come prima cosa bisognerà fornire un nome alla tabella, nel nostro esempio creeremo due tabelle: studenti ed esami.

Quando creo una nuova tabella, Access crea subito un campo **ID** (identificatore) e lo imposta come **chiave primaria**. Tale campo è di tipo numerico maè possibile cambiare le impostazioni ed utilizzare ad esempio la stringa del codice fiscale come chiave primaria.

Per aggiungere un nuovo campo, puoi avvalerti dei pulsanti **Testo Breve, Numero, Valuta, Data e ora, Sì/No**e **Altri campi** presenti nella scheda **Campi**, oppure cliccare sulla voce **Fare clic per aggiungere** collocata all’interno della tabella. Per eliminare un campo, è sufficiente fare **clic destro** su di esso e selezionare la voce **Elimina campo** dal menu proposto.

#### Aggiungiamo i dati

Abbiamo delle tabelle, ora possiamo aggiungere i dati. L’operazione è molto semplice, basta cliccare nelle caselle e digitare i dati. Si opera un po’ come in Excel.

![Inserire dati in access](/images/database/access//access_inseiredati.jpg)

Per eliminare una riga (record o tupla), fai **clic destro** sul rettangolo grigio collocato alla sinistra del primo campo che lo identifica, seleziona la voce **Elimina record** dal menu che si apre e clicca successivamente sul pulsante **Sì**.

#### Le relazioni

Le relazioni creano collegamenti logici tra diverse entità e sono la vera essenza dei tabase. Ciò che viene messo in collegamento sono le rispettive **chiavi primarie**. Rotornando all’esempio precedente, è possibile creare una relazione tra le entità **Studente** ed **Esame**, del tipo *Studente* “sostiene” *Esame,* creando una connessione tra le chiavi che identificano univocamente i record di ciascuna tabella, cioè **Codice fiscale** e **Codice esame**.

Per crearne una, clicca sul menù **Strumenti database** collocata in alto, poi sul pulsante **Relazioni**: dal piccolo pannello proposto, seleziona la prima tabella che intendi coinvolgere nella relazione, premi sul pulsante **Aggiungi** e ripeti l’operazione con le successive. Una volta aggiunte tutte le tabelle necessarie, premi sul pulsante **Chiudi**.

A questo punto, fai clic sul nuovo pannello **Relazioni** che compare nella schermata di Access, seleziona con il mouse la **chiave primaria** che identifica i record della prima tabella (contrassegnata da un’icona a forma di lampadina), trascinala sulla **chiave primaria** della seconda tabella e premi successivamente sul pulsante **Crea**: la comparsa di una freccia che collega una tabella all’altra conferma l’avvenuta creazione della relazione.

Una volta create le relazioni necessarie, premi sul pulsante **Chiudi** collocato in alto e poi sul pulsante **Sì** per ritornare alla visualizzazione tabelle.

![Nuova tabella in access](/images/database/access/access_relazioni.jpg)

#### Le query (interrogazioni)

Ora che abbiamo finalmente tutti i dati all’interno del nostro database access, possiamo procedere con le interrogazioni. Attraverso le interrogazioni posso trovare *tutti gli studenti che hanno preso un voto sufficiente all’esame*, oppure posso trovare, *tutti gli studenti che hanno provato a dare un certo esame*.

Per generare una query, clicca sulla sezione **Crea** collocata in alto, poi sul pulsante **Struttura query**: seleziona la tabella di cui ti interessa visualizzare i risultati dal menu proposto, premi sul pulsante **Aggiungi** e, infine, su **Chiudi**. Dopodiché clicca sul pannello **Query1** che compare a schermo.

![Nuova tabella in access](/images/database/access/access_interrogazioni.jpg)

Inizialmente, la schermata può sembrarti un po’ ostica, ma si tratta in realtà di una delle più potenti funzionalità di Access: clicca sulla cella **Campo** più a sinistra, seleziona un campo della tabella usando il menu a tendina proposto e clicca su di esso per aggiungerlo alla query, dopodiché ripeti l’operazione con i campi successivi.

![Nuova tabella in access](/images/database/access/access_interrogazioni2.jpg)

A questo punto, puoi raffinare la tua ricerca agendo sulle varie voci della piccola tabella mostrata in basso, modificandole in questo modo.

- **Ordinamento** – puoi ordinare i risultati della tua ricerca in base a un determinato campo impostando le voci Crescente o Decrescente in corrispondenza del campo stesso.
- **Mostra** – apponendo il segno di spunta nella casella, il campo viene mostrato nei risultati.
- **Criteri** – qui puoi impostare il criterio con cui raffinare i risultati in base al campo scelto (ad esempio, visualizzare gli studenti il cui anno di corso è maggiore o uguale al secondo).
- **Oppure** – sezione utile per specificare operazioni di unione logica.

Una volta definita correttamente la query, clicca sul pulsante **Esegui**: verrà generata al volo una tabella contenente i risultati dell’interrogazione eseguita.

![Nuova tabella in access](/images/database/access/access_interrogazioni3.jpg)

Se hai dimestichezza con il linguaggio SQL e conosci le operazioni di controllo (**ALTER, JOIN, CROSS JOIN** e così via), puoi utilizzarle sfruttando i pulsanti **Aggiornamento**, **Eliminazione**, **Passthrough** e **Accodamento** presenti nella schermata **Visualizzazione Struttura** analizzata poc’anzi. Qualora desiderassi impartire manualmente delle istruzioni SQL, premi sul pulsante **Definizione dati** per procedere.

#### Report

La generazione di un report è sicuramente il metodo più rapido per avere accesso immediato ai dati che ti interessa analizzare all’interno del tuo database. Tanto per farti un esempio, ritornando al nostro database Studenti/Esami, potresti voler generare un report che mostri tutti gli studenti che hanno sostenuto l’esame di Programmazione 1.

![Nuova tabella in access](/images/database/access/access_report.jpg)

Come è facile immaginare, puoi creare un report partendo da una **tabella** o da una specifica **query** definita con la procedura che ti ho illustrato poco fa: in linea generale, ciò che devi fare è cliccare sulla tabella o sulla query di cui ti interessa generare il report, recarti nella sezione **Crea,** premere sul pulsante **Creazione guidata report** e seguire le istruzioni a schermo per definire i campi da includere, i criteri da seguire e, eventualmente, le opzioni di raggruppamento. Una volta generato il report, puoi modificarne la struttura facendo **clic destro** sulla relativa voce collocata nel riquadro **Report** del pannello di sinistra, e selezionando **Visualizzazione struttura** dal menu contestuale proposto.

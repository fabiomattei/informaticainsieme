---
title: 'GDPR - Regolamento Generale sulla Protezione dei Dati'
date: '2026-07-27'
author: Fabio Mattei
layout: page
---

Il **GDPR** (*General Data Protection Regulation*, in italiano Regolamento Generale sulla Protezione dei Dati, Regolamento UE 2016/679) è la normativa europea che disciplina il trattamento dei dati personali. È entrato in vigore il 25 maggio 2018 ed è direttamente applicabile in tutti gli Stati membri, senza bisogno di essere recepito da leggi nazionali: è per questo che, a differenza di una direttiva, si parla di "regolamento".

Il GDPR nasce per rispondere a un problema molto concreto: nell'epoca dei social network, del cloud e dei big data, i dati personali sono diventati una delle risorse più preziose per le aziende, ma anche uno degli oggetti più esposti ad abusi, fughe di notizie e sorveglianza. Il regolamento prova a riequilibrare il rapporto di forza tra chi raccoglie i dati e chi li fornisce.

### A chi si applica

Il GDPR si applica a chiunque tratti dati personali di persone che si trovano nell'Unione Europea, indipendentemente da dove abbia sede l'azienda. Una società americana o cinese che offre servizi a cittadini europei deve rispettare il regolamento tanto quanto un'azienda italiana. Questo principio di **extraterritorialità** è uno degli aspetti più innovativi della norma ed è il motivo per cui aziende come Google, Meta o Amazon hanno dovuto adeguare le proprie pratiche a livello globale.

### I principi fondamentali

Il trattamento dei dati personali deve rispettare alcuni principi cardine, elencati nell'articolo 5 del regolamento:

- **Liceità, correttezza e trasparenza**: i dati vanno raccolti con una base giuridica valida (consenso, contratto, obbligo di legge, ecc.) e l'interessato deve sapere come vengono usati.
- **Limitazione della finalità**: i dati raccolti per uno scopo specifico non possono essere riutilizzati liberamente per scopi diversi e incompatibili.
- **Minimizzazione**: si devono raccogliere solo i dati strettamente necessari, non "tutto quello che si può".
- **Esattezza**: i dati devono essere corretti e aggiornati.
- **Limitazione della conservazione**: i dati non vanno conservati più a lungo di quanto necessario.
- **Integrità e riservatezza**: i dati vanno protetti da accessi non autorizzati, perdite o distruzioni accidentali, tipicamente tramite misure tecniche come la cifratura.
- **Accountability** (responsabilizzazione): chi tratta i dati deve essere in grado di dimostrare, documenti alla mano, di rispettare tutti i principi precedenti.

### I diritti dell'interessato

Il GDPR riconosce alla persona a cui i dati si riferiscono (l'"interessato") una serie di diritti azionabili nei confronti di chi tratta i suoi dati:

- **Diritto di accesso**: sapere quali dati vengono trattati e come.
- **Diritto di rettifica**: correggere dati inesatti.
- **Diritto alla cancellazione** ("diritto all'oblio"): ottenere la cancellazione dei propri dati in determinate condizioni.
- **Diritto alla limitazione del trattamento**: bloccare temporaneamente l'uso dei propri dati.
- **Diritto alla portabilità**: ricevere i propri dati in un formato strutturato e trasferirli a un altro fornitore.
- **Diritto di opposizione**: opporsi al trattamento, ad esempio per finalità di marketing diretto.

### I soggetti coinvolti

Il regolamento distingue diversi ruoli:

- **Titolare del trattamento**: chi decide finalità e modalità del trattamento (es. l'azienda che gestisce un sito e-commerce).
- **Responsabile del trattamento**: chi tratta i dati per conto del titolare (es. il fornitore di hosting o di un servizio di email marketing).
- **DPO** (*Data Protection Officer*, Responsabile della Protezione dei Dati): figura obbligatoria per enti pubblici e per organizzazioni che trattano dati su larga scala, con il compito di vigilare sulla conformità al regolamento.

### Sanzioni

Le violazioni del GDPR possono comportare sanzioni molto pesanti: fino a **20 milioni di euro** o al **4% del fatturato globale annuo** dell'azienda, se superiore. È proprio l'entità di queste sanzioni ad aver reso il GDPR un punto di riferimento anche fuori dall'Europa, ispirando leggi simili in California (CCPA), Brasile (LGPD) e altrove.

### Perché riguarda anche chi programma

Per chi sviluppa software, il GDPR non è solo una questione legale ma ha un impatto diretto sulla progettazione dei sistemi. Introduce infatti i concetti di **privacy by design** e **privacy by default**: la protezione dei dati va pensata fin dalla progettazione di un'applicazione (ad esempio cifrando i dati sensibili, minimizzando i campi raccolti in un form, anonimizzando i dataset) e le impostazioni predefinite di un servizio devono essere quelle più tutelanti per l'utente, senza richiedergli di modificarle manualmente.

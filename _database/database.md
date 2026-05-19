---
title: Database
date: '2020-02-10T06:20:07+01:00'
author: Fabio Mattei
layout: page
---

L’archiviazione e il reperimento delle informazioni rappresentano una delle maggiori risposte che l’informatica ha saputo dare alle aziende e alle istituzioni.

Possiamo soltanto immaginare come fossero gli archivi delle banche o degli istituti anagrafici fino a pochi decenni fa e possiamo soltanto rabbrividire pensando al lavoro richiesto per generare un estratto conto con le transazioni degli ultimi 5 anni prima dell’invenzione dei database.

Le basi di dati rispondono al problema di poter memorizzare **grandi** moli di dati che siano **condivise** tra più attori e **persistenti** nel tempo. Le basi di dati rispondono al bisogno di **affidabilità** — conservare le informazioni in modo intatto — e di **efficienza** nel recuperarle quando necessario.

### Come si studia un database

Lo studio dei database segue un percorso preciso: si parte dalla **progettazione concettuale**, cioè dal capire quali informazioni vogliamo memorizzare e come sono collegate tra loro, per poi passare alla **realizzazione pratica** usando il linguaggio SQL. Questo percorso si articola in tre fasi:

1. **Modellazione** — Si disegna il modello entità-relazione (ER), uno schema visivo che descrive le entità del problema reale e i legami tra di esse.
2. **Traduzione** — Il modello ER viene convertito in tabelle relazionali, con chiavi primarie e chiavi esterne che mantengono i collegamenti.
3. **Interrogazione** — Si usa SQL per creare le tabelle, inserire i dati e recuperarli con query anche complesse.

### Strumenti

Gli esempi pratici di questo capitolo usano **SQLite**, un database leggero che salva tutti i dati in un singolo file. È lo stesso motore usato da Firefox, Android, WhatsApp e molte altre applicazioni diffuse. Per chi preferisce un ambiente grafico, viene presentato anche **MS Access**.

Gli argomenti del nostro studio saranno:

- [Il modello entità relazione]({{ site.baseurl }}{% link _database/modello-entita-relazione.md %}.html)
- [Il linguaggio SQL]({{ site.baseurl }}{% link _database/il-linguaggio-sql.md %}.html)
- [Le forme normali]({{ site.baseurl }}{% link _database/forme-normali.md %}.html)
- [MS Access]({{ site.baseurl }}{% link _database/ms-access.md %}.html)
- [SQLite e python]({{ site.baseurl }}{% link _database/sqlite-e-python.md %}.html)

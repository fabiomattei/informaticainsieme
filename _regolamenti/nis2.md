---
title: 'NIS2 - La direttiva europea sulla cybersicurezza'
date: '2026-07-27'
author: Fabio Mattei
layout: page
---

La **direttiva NIS2** (*Network and Information Security 2*, Direttiva UE 2022/2555) è la normativa europea che impone requisiti di cybersicurezza alle organizzazioni che offrono servizi essenziali o importanti per la società. Aggiorna e amplia una precedente direttiva del 2016 (la "NIS1"), che si era rivelata insufficiente di fronte alla crescita degli attacchi informatici contro infrastrutture critiche, ospedali, reti energetiche e catene di approvvigionamento.

A differenza del [GDPR]({{ site.baseurl }}{% link _regolamenti/gdpr.md %}.html), della [DMA]({{ site.baseurl }}{% link _regolamenti/digital-markets-act.md %}.html) o della [DSA]({{ site.baseurl }}{% link _regolamenti/digital-services-act.md %}.html), la NIS2 è una **direttiva** e non un regolamento: non è direttamente applicabile, ma richiede che ogni Stato membro la recepisca con una propria legge nazionale, con la possibilità di adattarne alcuni dettagli al proprio contesto.

### Chi deve rispettarla

La NIS2 individua due categorie di organizzazioni soggette agli obblighi, in base al settore in cui operano e alle loro dimensioni (tipicamente medie e grandi imprese):

- **Soggetti essenziali**: energia, trasporti, settore bancario, infrastrutture dei mercati finanziari, sanità, acqua potabile, infrastrutture digitali (data center, cloud provider, operatori DNS), pubblica amministrazione.
- **Soggetti importanti**: servizi postali, gestione dei rifiuti, produzione e distribuzione di prodotti chimici e alimentari, manifattura di dispositivi elettronici e macchinari, fornitori di servizi digitali (marketplace online, motori di ricerca, social network).

Rispetto alla NIS1, la NIS2 amplia notevolmente il numero di settori coperti, arrivando a includere anche la catena di fornitura: un'azienda soggetta alla direttiva deve valutare anche i rischi legati ai propri fornitori e partner tecnologici.

### Gli obblighi principali

- **Gestione del rischio**: adottare misure tecniche, operative e organizzative per gestire i rischi alla sicurezza delle reti e dei sistemi informativi, proporzionate al rischio effettivo.
- **Sicurezza della catena di approvvigionamento**: valutare la sicurezza dei fornitori e dei servizi che l'organizzazione utilizza, incluso il software di terze parti.
- **Gestione degli incidenti**: predisporre procedure per rilevare, gestire e rispondere agli incidenti di sicurezza.
- **Notifica obbligatoria degli incidenti**: segnalare gli incidenti significativi alle autorità competenti entro tempi stretti — un preallarme entro **24 ore** dalla scoperta dell'incidente, seguito da una notifica più dettagliata entro **72 ore**.
- **Formazione e consapevolezza**: garantire una formazione adeguata del personale, inclusi i vertici aziendali, sui temi della cybersicurezza.
- **Uso di crittografia e autenticazione**: adottare pratiche di base come l'autenticazione a più fattori e la cifratura dei dati, quando appropriato.

### La responsabilità del management

Una delle novità più rilevanti della NIS2 è che introduce la **responsabilità diretta degli organi di gestione** (consigli di amministrazione, dirigenti) per la conformità dell'organizzazione agli obblighi di cybersicurezza. Non è più un tema delegabile esclusivamente al reparto IT: i vertici aziendali devono approvare le misure di gestione del rischio e possono essere ritenuti personalmente responsabili in caso di violazioni gravi.

### Le sanzioni

Le sanzioni previste dalla NIS2 (nei limiti stabiliti da ciascuno Stato membro in fase di recepimento) possono arrivare fino a **10 milioni di euro** o al **2% del fatturato globale annuo** per i soggetti essenziali, e fino a **7 milioni di euro** o all'**1,4%** del fatturato per i soggetti importanti.

### Perché riguarda chi programma

Per chi sviluppa software e gestisce infrastrutture, la NIS2 rende obbligatorie per legge pratiche che dovrebbero comunque essere considerate buone abitudini: gestione delle patch di sicurezza, controllo degli accessi, cifratura dei dati in transito e a riposo, piani di disaster recovery e valutazione della sicurezza delle librerie e dei servizi di terze parti usati in un progetto. È lo stesso principio del *security by design*, applicato non più come raccomandazione ma come obbligo normativo per interi settori dell'economia.

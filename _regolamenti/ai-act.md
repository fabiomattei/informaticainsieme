---
title: 'AI Act - Il Regolamento europeo sull''intelligenza artificiale'
date: '2026-07-27'
author: Fabio Mattei
layout: page
---

L'**AI Act** (Regolamento UE 2024/1689) è il primo regolamento al mondo dedicato in modo specifico all'intelligenza artificiale. Approvato nel 2024, è il tentativo dell'Unione Europea di normare una tecnologia che si è diffusa più rapidamente di quanto qualsiasi legislatore riuscisse a inseguire, dai chatbot generativi ai sistemi di riconoscimento facciale.

L'approccio scelto dal legislatore europeo non è quello di regolare "l'IA" come categoria unica, ma di classificare i **sistemi di IA in base al rischio** che comportano per i diritti fondamentali e la sicurezza delle persone. Più un sistema è rischioso, più stringenti sono gli obblighi a cui è sottoposto.

### I quattro livelli di rischio

- **Rischio inaccettabile — pratiche vietate**: sistemi considerati una minaccia inaccettabile per i diritti fondamentali, e quindi **vietati** in assoluto. Rientrano in questa categoria il *social scoring* da parte di enti pubblici (valutare e classificare le persone in base al comportamento sociale), il riconoscimento delle emozioni sul posto di lavoro e nelle scuole, la manipolazione subliminale del comportamento, e — salvo eccezioni molto limitate legate a indagini su reati gravi — l'identificazione biometrica in tempo reale in spazi pubblici.
- **Alto rischio**: sistemi usati in ambiti che incidono in modo significativo sulla vita delle persone, come selezione del personale, accesso al credito, dispositivi medici, gestione delle infrastrutture critiche, sistemi giudiziari o di gestione della migrazione. Sono ammessi, ma sottoposti a obblighi rigorosi: valutazione di conformità prima dell'immissione sul mercato, gestione del rischio, qualità dei dati di addestramento, supervisione umana, documentazione tecnica e tracciabilità (logging).
- **Rischio limitato**: sistemi soggetti principalmente a obblighi di **trasparenza**. Un chatbot deve dichiarare di essere un'IA e non un umano; i contenuti generati o manipolati artificialmente (i cosiddetti *deepfake*) devono essere etichettati come tali.
- **Rischio minimo**: la stragrande maggioranza dei sistemi di IA in uso oggi (filtri antispam, videogiochi, sistemi di raccomandazione non critici) non è soggetta a obblighi specifici, oltre alle normative già esistenti.

### I modelli di IA per finalità generali (GPAI)

L'AI Act dedica una disciplina specifica ai **modelli di IA per finalità generali** (*General Purpose AI*, GPAI) — la categoria a cui appartengono i grandi modelli linguistici come GPT, Claude o Gemini, che non sono progettati per un singolo scopo ma possono essere adattati a moltissimi usi diversi. Chi sviluppa questi modelli deve fornire documentazione tecnica, rispettare il diritto d'autore sui dati di addestramento e pubblicare una sintesi dei contenuti usati per l'addestramento. Per i modelli più potenti, classificati come a **rischio sistemico** in base alla potenza di calcolo impiegata per addestrarli, si aggiungono obblighi di valutazione dei rischi, test di sicurezza e segnalazione degli incidenti gravi.

### Tempistiche di entrata in vigore

L'AI Act non si applica tutto in un colpo solo, ma con un'entrata in vigore scaglionata nel tempo, per dare alle aziende il tempo di adeguarsi:

- **Febbraio 2025**: divieto delle pratiche a rischio inaccettabile.
- **Agosto 2025**: obblighi per i modelli GPAI.
- **Agosto 2026**: piena applicazione degli obblighi per i sistemi ad alto rischio e degli obblighi di trasparenza.
- **Agosto 2027**: termine ultimo per l'adeguamento dei sistemi ad alto rischio già integrati in prodotti regolamentati (es. dispositivi medici, macchinari).

### Le sanzioni

Le sanzioni per le violazioni dell'AI Act sono tra le più alte mai previste da un regolamento europeo: fino a **35 milioni di euro** o al **7% del fatturato globale annuo** per l'uso di pratiche vietate, e fino al **3%** del fatturato per la violazione degli altri obblighi.

### Perché riguarda chi studia informatica

L'AI Act non è solo un tema per giuristi: per chi progetta o addestra sistemi di IA, definisce vincoli molto concreti sul ciclo di vita di un progetto — dalla qualità e provenienza dei dati di addestramento alla necessità di documentare le scelte tecniche, fino alla progettazione di meccanismi di supervisione umana per i sistemi ad alto rischio. È lo stesso tipo di attenzione "by design" richiesta dal [GDPR]({{ site.baseurl }}{% link _regolamenti/gdpr.md %}.html) per la privacy, applicata questa volta alla sicurezza e all'affidabilità dei sistemi di intelligenza artificiale.

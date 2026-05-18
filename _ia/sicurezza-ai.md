---
title: 'Sicurezza AI'
date: '2026-05-18T09:00:00+02:00'
author: Fabio Mattei
layout: page
---

Quando un sistema di intelligenza artificiale entra in un'organizzazione, porta con sé capacità nuove — e superfici di attacco nuove. Alcune di queste superfici sono varianti di problemi già noti nella sicurezza informatica tradizionale; altre sono specifiche dei modelli linguistici e dei sistemi agentici, e richiedono modi di pensare che i framework classici non coprono del tutto.

## I Rischi dei Sistemi AI Attuali

I sistemi AI che usiamo oggi — modelli linguistici integrati in applicazioni, chatbot, strumenti di analisi — sono esposti a una serie di rischi che è utile conoscere prima ancora di pensare alle contromisure.

Il più discusso è la **prompt injection**: un attaccante inserisce istruzioni nascoste nell'input del modello — in un documento, in una pagina web, in un messaggio — per manipolare il comportamento del sistema senza che l'utente legittimo se ne accorga. È l'equivalente di un'iniezione SQL, ma applicata al linguaggio naturale, e si difende in modo altrettanto difficile perché il confine tra istruzione e dato non è sintattico.

Strettamente correlato è il rischio di **data poisoning**: chi ha accesso ai dati di addestramento o di fine-tuning può introdurre esempi manipolati che alterano il comportamento del modello in modo sottile e difficile da rilevare. Non serve compromettere il modello dopo il training — basta contaminare i dati prima.

Il **data leakage** riguarda invece la privacy: i modelli possono esporre informazioni sensibili presenti nei dati di addestramento o nel contesto della conversazione, a volte senza che ci sia un attacco esplicito. Infine, la **supply chain AI** — librerie, modelli pre-addestrati, dataset, API di terze parti — introduce dipendenze la cui sicurezza non è sempre verificabile.

## I Rischi dei Sistemi Agentici

Con i sistemi agentici — agenti AI che pianificano, usano strumenti, chiamano API, eseguono codice e prendono decisioni in sequenza — la superficie di attacco si espande drasticamente, e i rischi assumono una natura diversa.

L'**agent hijacking** è la versione agentica della prompt injection: un agente viene dirottato da istruzioni malevole incontrate durante l'esecuzione, ad esempio in una pagina web che sta elaborando. La differenza rispetto al modello statico è che l'agente può compiere azioni concrete — inviare email, modificare file, chiamare API — prima che qualcuno se ne accorga.

Il **privilege creep** descrive un fenomeno già noto nella gestione delle identità umane: nel tempo, gli agenti accumulano accessi e permessi che non servono più all'attività corrente. A differenza di un dipendente, un'identità non umana non si licenzia e non viene riorganizzata: rimane con tutti i suoi permessi finché qualcuno non li revoca attivamente.

Le **azioni autonome irreversibili** sono forse il rischio più concreto: un agente che cancella dati, invia comunicazioni, modifica configurazioni o esegue transazioni può causare danni che non si annullano con un rollback. La velocità di esecuzione degli agenti rende il danno difficile da contenere.

A tutto questo si aggiungono l'**emergent behavior** — comportamenti non previsti che emergono dall'interazione tra agenti o tra agente e ambiente — e la progressiva perdita di **osservabilità**: man mano che le catene decisionali si allungano, capire perché un sistema ha fatto una certa scelta diventa sempre più difficile.

## Rischi Sistemici e di Governance

Al di là degli attacchi tecnici, c'è un rischio di natura organizzativa che spesso viene sottovalutato: la velocità di adozione dell'AI supera la velocità con cui le organizzazioni costruiscono la capacità di governarla.

Sistemi vengono deployati prima che esistano policy chiare su chi può usarli, con quali dati e con quali limiti. L'autonomia degli agenti supera i controlli interni esistenti. I framework di risk management tradizionali — ISO, NIST, Zero Trust — non sono stati progettati per sistemi che ragionano, pianificano e agiscono in modo semi-autonomo, e adattarli richiede tempo e competenze che molte organizzazioni non hanno ancora.

## Zero Trust Applicata all'AI

Il principio fondante della Zero Trust è semplice: **non fidarti mai, verifica sempre**. Non si parte dall'assunzione che chi è dentro il perimetro sia affidabile — perché il perimetro, nell'architettura distribuita di oggi, non esiste più in modo significativo. Ogni accesso viene valutato nel contesto: chi sta chiedendo, da dove, con quale dispositivo, su quale dato, con quale rischio associato. La fiducia non è permanente: si concede, si misura, si revoca.

Applicata all'AI generativa, questa logica diventa uno strumento pratico per governare prompt, dati, plugin, agenti e azioni automatiche.

Il primo principio è la **verifica esplicita**: prima di concedere l'accesso a dati, strumenti o azioni, bisogna sapere chi o cosa sta chiedendo. Utenti, agenti, modelli, API e connettori devono essere identificati e valutati, non assunti come affidabili per default.

Il secondo è il **minimo privilegio**: ogni componente del sistema — umano o non umano — riceve solo gli accessi strettamente necessari per il compito corrente, per il tempo strettamente necessario. Un agente che deve leggere un documento non ha bisogno di poter inviare email. Un modello che deve rispondere a domande su un dataset non ha bisogno di accedere all'intera base dati aziendale.

Il terzo è **assumere la violazione**: progettare il sistema partendo dall'ipotesi che qualcosa sarà compromesso. Questo significa pensare in anticipo a prompt injection, a data leak, a tool abusati, a output manipolati — e costruire i controlli di conseguenza, non come risposta a un incidente già avvenuto.

*Riferimenti: NIST SP 800-207, Microsoft Zero Trust, OWASP Top 10 for LLM Apps.*

## Come Si Implementa in Pratica

L'obiettivo non è "fidarsi del modello" — è governare l'intero percorso: dalla richiesta iniziale ai dati recuperati, dal ragionamento del modello agli strumenti usati, fino alla risposta prodotta e al monitoraggio successivo.

Il punto di partenza è sempre un **inventario**: sapere quali sistemi AI sono in uso, chi li usa, con quali dati e con quali permessi. Senza inventario non c'è governance. A questo si affianca la **classificazione dei dati** che l'AI può raggiungere, in modo che le policy di accesso possano essere calibrate sul livello di sensibilità.

Le **identità degli agenti** vanno gestite con la stessa attenzione delle identità umane: provisioning, review periodica, deprovisioning. I **guardrail su prompt e output** filtrano l'ingresso di istruzioni malevole e l'uscita di contenuti problematici. Le **azioni ad alto impatto** — quelle che modificano dati, inviano comunicazioni o allocano risorse — richiedono un meccanismo di approvazione esplicita prima di essere eseguite.

Infine, **log, test e capacità di risposta**: senza tracciabilità non si può investigare un incidente, e senza test regolari — incluso il red teaming — non si sa se i controlli funzionano davvero.

## Zero Trust e Sistemi Agentici: la Corrispondenza

I principi Zero Trust non sono solo slogan: applicati ai sistemi agentici, ogni principio corrisponde a una mitigazione concreta.

"Never trust, always verify" diventa autenticazione step-based: ogni azione dell'agente va autenticata, non solo l'accesso iniziale. "Least privilege" diventa scope minimo per tool e API, con token temporanei che scadono anziché credenziali permanenti. "Continuous evaluation" diventa rivalutazione del rischio per ogni passo decisionale dell'agente, non una volta sola all'inizio. "Assume breach" significa trattare l'agente come potenzialmente compromesso e costruire i controlli di conseguenza. "Strong identity" significa gestire le identità non umane con lo stesso rigore di quelle umane, incluso il ciclo di vita completo. I "policy decision points" garantiscono che ogni azione rilevante passi per un punto centrale di autorizzazione.

Questa corrispondenza non risolve tutti i problemi — i sistemi agentici sono ancora un territorio parzialmente inesplorato dal punto di vista della sicurezza — ma offre un punto di partenza solido per chi deve costruire controlli reali invece di aspettare che il problema si presenti.

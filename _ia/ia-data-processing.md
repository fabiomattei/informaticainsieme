---
title: 'Data Processing'
date: '2022-04-28T09:57:43+02:00'
author: Fabio Mattei
layout: page
---

![Pipeline dei dati grezzi che diventano previsioni o segnalazioni di anomalia](/images/ia/ia-data-processing/ia-data-processing.svg){:class="aside-image"}

Si tratta di algoritmi che analizzano dati specifici per estrapolare informazioni e compiere azioni in conseguenza. 
In questa categoria rientrano diversi utilizzi, come l’Analisi Predittiva (analisi di dati per fornire previsioni 
sull’andamento futuro di un determinato fenomeno) e il Rilevamento di frodi (identificazione di elementi non conformi 
a un modello previsto).

A differenza dell'IA generativa, di cui si parla molto sui media, l'Intelligent Data Processing lavora quasi sempre "dietro le quinte": non produce testi o immagini da mostrare all'utente, ma numeri, previsioni e decisioni che alimentano altri sistemi. Per questo motivo è meno visibile, ma è probabilmente la forma di IA più diffusa nel mondo aziendale.

## Analisi Predittiva

L'analisi predittiva utilizza dati storici per stimare cosa accadrà in futuro. Gli algoritmi più usati sono la [regressione lineare]({{ site.baseurl }}{% link _ia/regressione-lineare.md %}.html), gli alberi decisionali e le [reti neurali]({{ site.baseurl }}{% link _ia/reti-neurali.md %}.html), a seconda della complessità del fenomeno da prevedere.

Alcuni esempi concreti:

* **Manutenzione predittiva**: sensori installati su macchinari industriali (temperatura, vibrazioni, consumo elettrico) alimentano un modello che prevede quando un componente è vicino al guasto, permettendo di sostituirlo prima che si rompa e fermi la produzione.
* **Previsione della domanda**: catene di supermercati e siti di e-commerce usano modelli predittivi per stimare quanti prodotti verranno venduti nei prossimi giorni, ottimizzando magazzino e logistica.
* **Previsioni meteorologiche e climatiche**: enormi quantità di dati raccolti da satelliti e stazioni meteo vengono elaborati per prevedere l'andamento del tempo nei giorni successivi.
* **Scoring creditizio**: le banche utilizzano modelli predittivi per stimare la probabilità che un richiedente restituisca un prestito, basandosi sulla sua storia finanziaria.
* **Manutenzione predittiva in ambito sanitario**: analizzando i parametri vitali di un paziente ricoverato, un modello può segnalare in anticipo un possibile peggioramento delle condizioni cliniche.

## Rilevamento delle frodi

Il rilevamento delle frodi (*fraud detection*) consiste nell'identificare automaticamente transazioni o comportamenti che si discostano dal modello considerato "normale". Si tratta tipicamente di un problema di **individuazione di anomalie**: il sistema impara come appare un comportamento tipico e segnala tutto ciò che se ne discosta in modo significativo.

Alcuni ambiti di applicazione:

* **Carte di credito**: se una carta normalmente utilizzata a Roma viene improvvisamente usata per un acquisto a Singapore pochi minuti dopo, il sistema blocca automaticamente la transazione sospetta e allerta il titolare.
* **Assicurazioni**: gli algoritmi analizzano le richieste di risarcimento per individuare pattern tipici delle frodi, ad esempio incidenti che si ripetono con dinamiche sospettosamente simili o coinvolgono sempre le stesse officine.
* **Account online**: piattaforme come social network e servizi di posta elettronica utilizzano l'analisi comportamentale per riconoscere account falsi o compromessi, osservando ad esempio la velocità con cui vengono compiute delle azioni (un umano non può inviare centinaia di messaggi in pochi secondi).
* **Sistemi bancari (antiriciclaggio)**: le banche sono obbligate per legge a monitorare i movimenti di denaro alla ricerca di pattern tipici del riciclaggio, come una serie di piccoli versamenti pensati per non superare le soglie di segnalazione obbligatoria.

## Perché è un problema difficile

Sia l'analisi predittiva sia il rilevamento delle frodi condividono una difficoltà di fondo: i casi "anomali" o "rari" (un guasto, una frode) sono per definizione pochi rispetto ai casi normali. Questo rende difficile addestrare un modello che sia preciso: un sistema troppo permissivo lascia passare frodi reali, uno troppo aggressivo blocca continuamente transazioni legittime, generando disagio agli utenti onesti.

Per questo motivo questi sistemi vengono continuamente ricalibrati, e spesso lavorano in coppia con un controllo umano: l'IA segnala i casi sospetti, ma è una persona a decidere l'azione finale nei casi più delicati (ad esempio il blocco di un conto corrente).

## Domande

* Un algoritmo di scoring creditizio che nega un prestito basandosi su dati storici rischia di perpetuare discriminazioni già presenti nella società (ad esempio verso chi vive in certe zone o ha un certo tipo di lavoro)? Come si potrebbe verificarlo?
* È giusto che una transazione legittima venga bloccata automaticamente da un algoritmo, senza che nessuno l'abbia rivista? Che alternative ci sono?
* In quali altri ambiti della tua vita quotidiana pensi che un sistema di data processing stia già prendendo decisioni che ti riguardano, senza che tu te ne accorga?


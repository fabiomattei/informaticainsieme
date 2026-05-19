---
title: 'IA Guida Autonoma'
date: '2022-04-28T09:57:43+02:00'
author: Fabio Mattei
layout: page
---

## Introduzione

La guida autonoma è uno degli obiettivi più ambiziosi dell'intelligenza artificiale applicata. Un veicolo che guida da solo deve percepire l'ambiente circostante, interpretarlo, prendere decisioni in tempo reale e agire — tutto senza intervento umano, in un contesto fisico caotico e imprevedibile come il traffico stradale.

Non si tratta di un singolo algoritmo ma di un sistema complesso in cui computer vision, reti neurali, sensori, mappe ad alta definizione e pianificazione del percorso cooperano continuamente. Ogni secondo di guida richiede migliaia di decisioni: dove si trova il veicolo con precisione centimetrica, quali oggetti lo circondano, cosa si muove e in che direzione, qual è la traiettoria più sicura.

## I livelli di autonomia

La Society of Automotive Engineers (SAE) ha definito una scala da 0 a 5 che classifica il grado di automazione di un veicolo:

| Livello | Nome | Descrizione |
|---|---|---|
| 0 | Nessuna automazione | Il guidatore controlla tutto |
| 1 | Assistenza al guidatore | Un sistema aiuta con sterzo *o* accelerazione/frenata (es. cruise control adattivo) |
| 2 | Automazione parziale | Il sistema controlla sterzo *e* accelerazione/frenata, ma il guidatore deve tenere gli occhi sulla strada |
| 3 | Automazione condizionale | Il sistema guida autonomamente in condizioni definite; il guidatore può distrarsi ma deve essere pronto a riprendere il controllo |
| 4 | Alta automazione | Guida completamente autonoma in aree geografiche o condizioni prestabilite, senza necessità di intervento umano |
| 5 | Piena automazione | Guida autonoma in qualsiasi condizione, ovunque un essere umano potrebbe guidare |

La maggior parte dei veicoli commerciali attuali si trova ai livelli 2–3. Tesla Autopilot e sistemi simili rientrano nel livello 2: richiedono che il guidatore rimanga attento e pronto a intervenire. Il livello 4 esiste in contesti limitati — robotaxi che operano in aree geografiche ristrette, come quelli di Waymo a San Francisco. Il livello 5 non esiste ancora in forma commerciale.

## Come funziona: i sensori

Un veicolo autonomo percepisce il mondo attraverso più tipi di sensori che si integrano fra loro:

**Videocamere** — posizionate tutto attorno al veicolo, forniscono immagini ad alta risoluzione. Sono essenziali per leggere i segnali stradali, riconoscere le strisce pedonali, i semafori e i pedoni. Hanno però un limite: faticano in condizioni di scarsa visibilità (nebbia, pioggia intensa, notte).

**LiDAR** (*Light Detection and Ranging*) — emette migliaia di impulsi laser al secondo e misura il tempo di ritorno per costruire una mappa tridimensionale precisa dell'ambiente circostante. Funziona bene di notte e rileva oggetti con grande accuratezza geometrica, ma è costoso e meno efficace sotto la pioggia battente.

**Radar** — misura la distanza e la velocità degli oggetti usando onde radio. Funziona in qualsiasi condizione meteorologica ed è la base del cruise control adattivo e dell'emergency braking. Ha risoluzione spaziale inferiore al LiDAR.

**GPS ad alta precisione** — localizza il veicolo con precisione centimetrica, integrato con mappe stradali ad alta definizione che includono la posizione delle corsie, i segnali, le precedenze e le caratteristiche del manto stradale.

Nessun sensore da solo è sufficiente: i sistemi più robusti fondono i dati di tutti questi sensori (**sensor fusion**) per ottenere una rappresentazione del mondo più completa e affidabile di quella che ciascun sensore potrebbe offrire singolarmente.

## Come funziona: la percezione

Acquisiti i dati dai sensori, il sistema deve interpretarli. Questa fase si chiama **percezione** e risponde alla domanda: cosa c'è intorno al veicolo?

Reti neurali convoluzionali analizzano le immagini delle videocamere per rilevare e classificare oggetti: altre auto, pedoni, ciclisti, animali, ostacoli. I modelli devono essere addestrati su milioni di esempi per riconoscere oggetti in condizioni diverse di luce, angolazione e parziale occlusione — una persona che sporge parzialmente da dietro un'auto è ancora una persona.

Il sistema deve anche **prevedere il comportamento** degli oggetti rilevati: un pedone che si avvicina al bordo del marciapiede potrebbe stare per attraversare; un'auto con le quattro frecce potrebbe rallentare improvvisamente. Queste previsioni vengono prodotte da modelli addestrati su enormi quantità di dati di traffico reale.

## Come funziona: la pianificazione e il controllo

Una volta costruita la rappresentazione dell'ambiente, il sistema deve decidere cosa fare: la **pianificazione**. Questo livello risponde alla domanda: qual è la traiettoria migliore?

Il pianificatore considera:
- la destinazione e il percorso calcolato
- la posizione e la velocità degli altri veicoli
- le regole del codice della strada (precedenze, limiti di velocità, divieti)
- il comfort del passeggero (accelerazioni e frenate non brusche)
- la sicurezza in caso di comportamenti inattesi degli altri utenti della strada

L'output è una traiettoria — una sequenza di posizioni e velocità target — che il livello di **controllo** traduce in comandi fisici per sterzo, acceleratore e freno.

## Le sfide aperte

Nonostante i progressi enormi degli ultimi anni, la guida autonoma al livello 5 rimane un problema irrisolto. Le difficoltà principali sono:

**I casi limite** (*edge cases*). I sistemi funzionano bene nelle situazioni comuni, ma il traffico reale è pieno di situazioni rare e imprevedibili: un bambino che insegue un pallone tra le auto, una buca coperta d'acqua, un cantiere improvvisato, un vigile che fa segni in mezzo a un incrocio. Raccogliere abbastanza dati per addestrare il sistema su tutte le possibili eccezioni è praticamente impossibile.

**Le condizioni meteorologiche avverse**. Neve, pioggia intensa, nebbia fitta degradano le prestazioni di videocamere e LiDAR. Le strisce sull'asfalto scompaiono sotto la neve. I sensori si sporcano.

**Gli ambienti non mappati**. I sistemi di livello 4 dipendono da mappe ad alta definizione costantemente aggiornate. In zone non mappate — strade secondarie, cantieri, percorsi modificati — le prestazioni peggiorano sensibilmente.

**L'interazione con gli esseri umani**. Gli esseri umani comunicano tra loro in modo informale: uno sguardo, un gesto della mano, un cenno. Un veicolo autonomo non riesce facilmente a interpretare questi segnali né a emetterne di equivalenti, rendendo difficile la convivenza in situazioni ambigue.

## Il problema etico: il dilemma del carrello

La guida autonoma rende concreto un dilemma filosofico classico, noto come il **dilemma del carrello** (*trolley problem*). Immaginate una situazione in cui un incidente è inevitabile: il veicolo può colpire un pedone che ha attraversato col rosso, oppure sterzare bruscamente mettendo a rischio il passeggero. Chi deve essere protetto?

Nei casi reali questo tipo di scelta emerge raramente in forma così netta, ma solleva domande genuine: chi programma queste priorità? Devono essere decise dal produttore, dal governo, dall'acquirente del veicolo? La risposta dovrebbe essere uguale in tutti i Paesi?

Uno studio condotto dal MIT (*Moral Machine Experiment*) ha raccolto le preferenze di milioni di persone in tutto il mondo simulando questo tipo di scenari. I risultati hanno mostrato differenze significative tra culture diverse, suggerendo che non esiste una risposta universale accettata.

## Chi è in gioco: le aziende

Diversi attori stanno sviluppando sistemi di guida autonoma con approcci diversi:

**Waymo** (Alphabet/Google) — considerata la più avanzata tecnicamente. Opera robotaxi senza conducente di sicurezza a San Francisco, Phoenix e altre città americane. Usa LiDAR, radar e videocamere con forte dipendenza da mappe ad alta definizione.

**Tesla** — scelta opposta: punta su videocamere come unico sensore principale (senza LiDAR) e raccoglie dati da milioni di veicoli in uso reale per migliorare i modelli continuamente. Il sistema si chiama Full Self-Driving (FSD) ed è al livello 2, nonostante il nome.

**Cruise** (General Motors) — ha avuto una licenza per robotaxi a San Francisco, poi revocata nel 2023 dopo un incidente grave. È un esempio di come la fiducia del regolatore possa essere persa rapidamente.

**Mobileye**, **Baidu Apollo**, **Zoox** (Amazon) e vari produttori di automobili tradizionali (Mercedes, BMW, Volkswagen) hanno tutti programmi attivi a vari stadi di sviluppo.

## Il quadro legale

Le leggi sulla guida autonoma variano enormemente da Paese a Paese. In molti Stati americani è possibile testare veicoli autonomi su strade pubbliche con autorizzazione. In Europa la situazione è più frammentata: la Germania ha approvato nel 2021 una legge che consente veicoli di livello 4 su strade pubbliche in condizioni specifiche; l'Italia sta adeguando il codice della strada.

Una questione legale aperta riguarda la **responsabilità in caso di incidente**: se un veicolo autonomo causa un incidente, chi è responsabile? Il passeggero che non stava guidando? Il produttore del veicolo? Il produttore del software? Questo nodo non è ancora risolto in nessun ordinamento giuridico in modo definitivo.

## Domande aperte

- Un veicolo autonomo dovrebbe sempre rispettare alla lettera il codice della strada, anche quando un essere umano sceglierebbe di infrangerlo per evitare un incidente?
- Chi decide le priorità etiche quando un incidente è inevitabile? Dovrebbe farlo il governo, il produttore o il singolo acquirente?
- Ha senso possedere un'auto privata in un mondo di robotaxi autonomi, o cambierebbe completamente il modello di mobilità?
- Come si garantisce che i dati di percorso raccolti dai veicoli — che registrano dove andiamo, quando e con chi — non vengano usati per sorvegliare i cittadini?

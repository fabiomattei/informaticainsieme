---
title: 'IA Recommendation System'
date: '2022-04-28T09:57:43+02:00'
author: Fabio Mattei
layout: page
---

## Sistema di raccomandazione

Un sistema di raccomandazioni (o sistema di raccomandazione) è un sistema di algoritmi di apprendimento automatico che utilizza i dati per aiutare a prevedere, restringere e trovare ciò che le persone stanno cercando tra un numero esponenzialmente crescente di opzioni.

## Cos'è un sistema di raccomandazione?

Un sistema di raccomandazioni è un algoritmo di intelligenza artificiale o IA, solitamente associato al machine learning, che utilizza i Big Data per suggerire o raccomandare prodotti aggiuntivi ai consumatori. Questi possono essere basati su diversi criteri, tra cui acquisti passati, cronologia delle ricerche, informazioni demografiche e altri fattori. I sistemi di raccomandazione sono estremamente utili in quanto aiutano gli utenti a scoprire prodotti e servizi che altrimenti non avrebbero trovato da soli.
I sistemi di raccomandazione sono addestrati a comprendere le preferenze, le decisioni precedenti e le caratteristiche di persone e prodotti utilizzando i dati raccolti sulle loro interazioni. Questi includono impressioni, clic, "Mi piace" e acquisti. Grazie alla loro capacità di prevedere interessi e desideri dei consumatori a un livello altamente personalizzato, i sistemi di raccomandazione sono molto apprezzati dai fornitori di contenuti e prodotti. Possono indirizzare i consumatori verso praticamente qualsiasi prodotto o servizio di loro interesse, dai libri ai video, dai corsi di salute all'abbigliamento.


![Recommendation system](/images/ia/recommendation1.png)


## Tipi di sistemi di raccomandazione
Sebbene esista un vasto numero di algoritmi e tecniche di raccomandazione, la maggior parte rientra in queste ampie categorie: filtraggio collaborativo, filtraggio dei contenuti e filtraggio contestuale.

* **Gli algoritmi di filtraggio collaborativo** raccomandano articoli (questa è la parte di filtraggio) in base alle informazioni sulle preferenze di molti utenti (questa è la parte collaborativa). Questo approccio sfrutta la similarità del comportamento delle preferenze degli utenti: date le precedenti interazioni tra utenti e articoli, gli algoritmi di raccomandazione imparano a prevedere le interazioni future. Questi sistemi di raccomandazione costruiscono un modello a partire dal comportamento passato di un utente, come articoli acquistati in precedenza o valutazioni assegnate a tali articoli e decisioni simili prese da altri utenti. L'idea è che se alcune persone hanno preso decisioni e acquistato prodotti simili in passato, come la scelta di un film, allora c'è un'alta probabilità che concordino su ulteriori selezioni future. Ad esempio, se un sistema di raccomandazione basato sul filtraggio collaborativo sa che tu e un altro utente avete gusti cinematografici simili, potrebbe consigliarti un film che sa essere già apprezzato da quest'altro utente.

![Recommendation system](/images/ia/recommendation2.png)

* **Il filtraggio dei contenuti**, al contrario, utilizza gli attributi o le caratteristiche di un elemento (la parte relativa al contenuto) per consigliare altri elementi simili alle preferenze dell'utente. Questo approccio si basa sulla somiglianza delle caratteristiche dell'elemento e dell'utente, e, date le informazioni sull'utente e sugli elementi con cui ha interagito (ad esempio, l'età dell'utente, la categoria di cucina di un ristorante, la recensione media di un film), modella la probabilità di una nuova interazione. Ad esempio, se un sistema di raccomandazione basato sul filtraggio dei contenuti rileva che ti sono piaciuti i film C'è posta per te e Insonnia d'amore, potrebbe consigliarti un altro film dello stesso genere e/o con lo stesso cast, come Joe contro il vulcano.

![Recommendation system](/images/ia/recommendation3.png)

* **I sistemi di raccomandazione ibridi** combinano i vantaggi delle tipologie sopra descritte per creare un sistema di raccomandazione più completo. 

* **Il filtro contestuale** include le informazioni contestuali degli utenti nel processo di raccomandazione. Netflix ha parlato al GTC di come ottenere raccomandazioni migliori inquadrando ogni raccomandazione come una previsione di sequenza contestuale. Questo approccio utilizza una sequenza di azioni contestuali dell'utente, oltre al contesto attuale, per prevedere la probabilità dell'azione successiva. Nell'esempio di Netflix, data una sequenza per ciascun utente (il paese, il dispositivo, la data e l'ora in cui ha guardato un film), è stato addestrato un modello per prevedere cosa guardare successivamente.

![Recommendation system](/images/ia/recommendation4.png)

## Casi d'uso e applicazioni

* **E-commerce e vendita al dettaglio**: Merchandising personalizzato. Immagina che un utente abbia già acquistato una sciarpa. Perché non offrire un cappello abbinato per completare il look? Questa funzionalità viene spesso implementata tramite algoritmi basati sull'intelligenza artificiale, come le sezioni "Completa il look" o "Potrebbero interessarti anche" su piattaforme di e-commerce come Amazon, Walmart, Target e molte altre. In media, un sistema di raccomandazione intelligente offre un aumento del 22,66% nei tassi di conversione per i prodotti web.
* **Media e intrattenimento**: contenuti personalizzati. I motori di raccomandazione basati sull'intelligenza artificiale possono analizzare il comportamento di acquisto di un individuo e individuare modelli che gli consentiranno di suggerire contenuti che molto probabilmente corrispondono ai suoi interessi. Questo è ciò che Google e Facebook applicano attivamente quando consigliano annunci pubblicitari, o ciò che Netflix fa dietro le quinte quando consiglia film e programmi TV.
* **Servizi bancari personalizzati** Un prodotto di massa consumato digitalmente da milioni di persone, il settore bancario è un'ottima fonte di raccomandazioni. Conoscere la situazione finanziaria dettagliata di un cliente e le sue preferenze passate, insieme ai dati di migliaia di utenti simili, è estremamente utile. Vantaggi dei sistemi di raccomandazioneI sistemi di raccomandazione sono una componente fondamentale che promuove esperienze utente personalizzate, un coinvolgimento più profondo con i clienti e potenti strumenti di supporto decisionale nei settori della vendita al dettaglio, dell'intrattenimento, della sanità, della finanza e di altri settori. Su alcune delle più grandi piattaforme commerciali, le raccomandazioni rappresentano fino al 30% del fatturato. Un miglioramento dell'1% nella qualità delle raccomandazioni può tradursi in miliardi di dollari di fatturato.

Le aziende implementano sistemi di raccomandazione per una serie di motivi, tra cui:

* Migliorare la fidelizzazione. Soddisfacendo costantemente le preferenze di utenti e clienti, le aziende hanno maggiori probabilità di fidelizzarli come abbonati o acquirenti. Quando un cliente percepisce di essere veramente compreso da un brand e non di ricevere informazioni casuali, è molto più propenso a rimanere fedele e a continuare ad acquistare sul tuo sito.
* Aumento delle vendite. Diversi studi dimostrano un aumento dei ricavi da upselling dal 10 al 50% grazie a suggerimenti accurati sui prodotti con la dicitura "potrebbero interessarti anche". Le vendite possono essere incrementate con strategie di sistemi di raccomandazione semplici come l'aggiunta di suggerimenti sui prodotti corrispondenti a una conferma d'acquisto; la raccolta di informazioni dai carrelli della spesa elettronici abbandonati; la condivisione di informazioni su "cosa stanno acquistando i clienti ora"; e la condivisione di acquisti e commenti di altri acquirenti.
* Aiutare a formare abitudini e tendenze dei clienti. Offrire costantemente contenuti accurati e pertinenti può innescare segnali che creano abitudini solide e influenzano i modelli di utilizzo dei clienti.
* Velocizzare il ritmo di lavoro. Analisti e ricercatori possono risparmiare fino all'80% del loro tempo quando ricevono suggerimenti personalizzati su risorse e altri materiali necessari per ulteriori ricerche. 
* Aumentare il valore del carrello. Le aziende con decine di migliaia di articoli in vendita avrebbero difficoltà a codificare i suggerimenti di prodotto per un inventario di questo tipo. Utilizzando diversi metodi di filtraggio, questi titani dell'e-commerce possono trovare il momento giusto per suggerire nuovi prodotti che i clienti potrebbero acquistare, sia sul loro sito web che tramite email o altri mezzi.

## Come funzionano i sistemi di raccomandazione

Il modo in cui un modello di raccomandazione formula le raccomandazioni dipenderà dal tipo di dati a disposizione. Se si dispone solo di dati sulle interazioni avvenute in passato, probabilmente si sarà interessati al filtraggio collaborativo. Se si dispone di dati che descrivono l'utente e gli elementi con cui ha interagito (ad esempio, l'età di un utente, la categoria di cucina di un ristorante, la recensione media di un film), è possibile modellare la probabilità di una nuova interazione, date queste proprietà al momento attuale, aggiungendo filtri di contenuto e contesto.

### Fattorizzazione di matrice per le raccomandazioni

Le tecniche di fattorizzazione di matrice (MF) sono alla base di molti algoritmi popolari, tra cui il word embedding e il topic modeling, e sono diventate una metodologia dominante nell'ambito delle raccomandazioni basate sul filtraggio collaborativo. La MF può essere utilizzata per calcolare la similarità nelle valutazioni o nelle interazioni dell'utente al fine di fornire raccomandazioni. Nella semplice matrice degli elementi utente riportata di seguito, a Ted e Carol piacciono i film B e C. A Bob piace il film B. Per consigliare un film a Bob, la fattorizzazione della matrice calcola che agli utenti a cui è piaciuto B è piaciuto anche C, quindi C è una possibile raccomandazione per Bob.

![Recommendation system](/images/ia/recommendation5.png)

La fattorizzazione di matrici utilizzando l'algoritmo dei minimi quadrati alternati (ALS) approssima la matrice sparsa di valutazione degli elementi utente u-per-i come prodotto di due matrici dense, le matrici fattoriali utente e elemento di dimensione u × f e f × i (dove u è il numero di utenti, i il numero di elementi e f il numero di caratteristiche latenti). Le matrici fattoriali rappresentano caratteristiche latenti o nascoste che l'algoritmo cerca di scoprire. Una matrice cerca di descrivere le caratteristiche latenti o nascoste di ciascun utente e l'altra cerca di descrivere le proprietà latenti di ciascun film. Per ciascun utente e per ciascun elemento, l'algoritmo ALS apprende iterativamente (f) "fattori" numerici che rappresentano l'utente o l'elemento. A ogni iterazione, l'algoritmo fissa alternativamente una matrice fattoriale e ottimizza per l'altra, e questo processo continua fino alla convergenza.
Alternating lease squares (ALS).

![Recommendation system](/images/ia/recommendation6.png)

CuMF è una libreria di fattorizzazione di matrici basata su NVIDIA® CUDA® che ottimizza il metodo dei minimi quadrati alternati (ALS) per risolvere MF su larga scala. CuMF utilizza una serie di tecniche per massimizzare le prestazioni su GPU singole e multiple. Queste tecniche includono l'accesso intelligente ai dati sparsi sfruttando la gerarchia di memoria della GPU, l'utilizzo del parallelismo dei dati in combinazione con il parallelismo del modello per ridurre al minimo il sovraccarico di comunicazione tra le GPU e un nuovo schema di riduzione del parallelo basato sulla topologia.

### Modelli di reti neurali profonde per la raccomandazione

Esistono diverse varianti di reti neurali artificiali (RNA), come le seguenti:
Le RNA in cui le informazioni vengono solo trasmesse da uno strato all'altro sono chiamate reti neurali feedforward. I percettroni multistrato (MLP) sono un tipo di RNA feedforward costituita da almeno tre strati di nodi: uno strato di input, uno strato nascosto e uno strato di output. Le MLP sono reti flessibili che possono essere applicate a una varietà di scenari.

Le reti neurali convoluzionali sono gli elaboratori di immagini per identificare gli oggetti.

Le reti neurali ricorrenti sono i motori matematici per analizzare i pattern linguistici e i dati sequenziati.
I modelli di raccomandazione basati sul deep learning (DL) si basano su tecniche esistenti come la fattorizzazione per modellare le interazioni tra variabili e gli embedding per gestire le variabili categoriali. Un embedding è un vettore appreso di numeri che rappresentano le caratteristiche delle entità in modo che entità simili (utenti o elementi) abbiano distanze simili nello spazio vettoriale. Ad esempio, un approccio di deep learning al filtraggio collaborativo apprende gli embedding di utenti ed elementi (vettori di caratteristiche latenti) in base alle interazioni di utenti ed elementi con una rete neurale.
Le tecniche di deep learning sfruttano anche le vaste e in rapida crescita nuove architetture di rete e algoritmi di ottimizzazione per addestrare grandi quantità di dati, sfruttare la potenza del deep learning per l'estrazione di caratteristiche e costruire modelli più espressivi.
Gli attuali modelli basati su deep learning per i sistemi di raccomandazione: DLRM, Wide and Deep (W&D), Neural Collaborative Filtering (NCF), Variational AutoEncoder (VAE) e BERT (per NLP) fanno parte del portfolio di modelli di deep learning accelerati da GPU NVIDIA che copre un'ampia gamma di architetture di rete e applicazioni in molti domini diversi oltre ai sistemi di raccomandazione, tra cui l'analisi di immagini, testo e parlato. Questi modelli sono progettati e ottimizzati per l'addestramento con TensorFlow e PyTorch.


### Filtri Collaborativi Neurali

Il modello di Neural Collaborative Filtering (NCF) è una rete neurale che fornisce un filtraggio collaborativo basato sulle interazioni di utenti ed elementi. Il modello tratta la fattorizzazione della matrice da una prospettiva non lineare. NCF TensorFlow accetta una sequenza di coppie (ID utente, ID elemento) come input, quindi li inserisce separatamente in una fase di fattorizzazione della matrice (in cui gli embedding vengono moltiplicati) e in una rete di percettroni multistrato (MLP). Gli output della fattorizzazione della matrice e della rete MLP vengono quindi combinati e inseriti in un singolo strato denso che prevede la probabilità che l'utente in input interagisca con l'elemento in input.

![Recommendation system](/images/ia/recommendation7.png)


# Autoencoder Variazionale per il Filtraggio Collaborativo

Una rete neurale autoencoder ricostruisce il livello di input a livello di output utilizzando la rappresentazione ottenuta nel livello nascosto. Un autoencoder per il filtraggio collaborativo apprende una rappresentazione non lineare di una matrice utente-elemento e la ricostruisce determinando i valori mancanti.
L'Autoencoder Variazionale per il Filtraggio Collaborativo (VAE-CF) accelerato da GPU NVIDIA è un'implementazione ottimizzata dell'architettura descritta per la prima volta in "Variational Autoencoders for Collaborative Filtering". VAE-CF è una rete neurale che fornisce un filtraggio collaborativo basato sulle interazioni tra utente e elemento. I dati di training per questo modello sono costituiti da coppie di ID utente-elemento per ciascuna interazione tra un utente e un elemento.
Il modello è costituito da due parti: l'encoder e il decoder. L'encoder è una rete neurale feedforward completamente connessa che trasforma il vettore di input, contenente le interazioni per un utente specifico, in una distribuzione variazionale n-dimensionale. Questa distribuzione variazionale viene utilizzata per ottenere una rappresentazione delle caratteristiche latenti di un utente (o embedding). Questa rappresentazione latente viene quindi immessa nel decodificatore, che è anch'esso una rete feedforward con una struttura simile a quella del codificatore. Il risultato è un vettore delle probabilità di interazione tra elementi per un particolare utente.

![Recommendation system](/images/ia/recommendation8.png)

## Apprendimento di sequenze contestuali

Una rete neurale ricorrente (RNN) è una classe di reti neurali dotata di memoria o cicli di feedback che consentono di riconoscere meglio i pattern nei dati. Le RNN risolvono compiti complessi che riguardano contesto e sequenze, come l'elaborazione del linguaggio naturale, e vengono utilizzate anche per fornire suggerimenti sulle sequenze contestuali. Ciò che distingue l'apprendimento di sequenze da altri compiti è la necessità di utilizzare modelli con una memoria dati attiva, come le LSTM (Memoria a lungo e breve termine) o le GRU (Unità ricorrenti gated) per apprendere la dipendenza temporale nei dati di input. Questa memoria degli input passati è fondamentale per il successo dell'apprendimento di sequenze. I modelli di deep learning basati su trasformatori, come BERT (Bidirectional Encoder Representations from Transformers), rappresentano un'alternativa alle RNN che applicano una tecnica di attenzione: l'analisi di una frase focalizzando l'attenzione sulle parole più rilevanti che la precedono e la seguono. I modelli di deep learning basati su trasformatori non richiedono l'elaborazione sequenziale dei dati in ordine, consentendo una parallelizzazione molto maggiore e tempi di addestramento ridotti sulle GPU rispetto alle RNN.

![Recommendation system](/images/ia/recommendation9.png)

In un'applicazione di NLP, il testo di input viene convertito in vettori di parole utilizzando tecniche come il word embedding. Con il word embedding, ogni parola nella frase viene tradotta in un insieme di numeri prima di essere inserita in varianti di RNN, Transformer o BERT per comprenderne il contesto. Questi numeri cambiano nel tempo mentre la rete neurale si addestra, codificando proprietà uniche come la semantica e le informazioni contestuali per ogni parola, in modo che parole simili siano vicine tra loro in questo spazio numerico e parole dissimili siano distanti. Questi modelli di DL forniscono un output appropriato per un compito linguistico specifico, come la previsione della parola successiva e il riepilogo del testo, che vengono utilizzati per produrre una sequenza di output.


![Recommendation system](/images/ia/recommendation10.png)

Le raccomandazioni basate sul contesto di sessione applicano i progressi nella modellazione sequenziale, derivanti dal deep learning e dall'elaborazione del linguaggio naturale (NLP), alle raccomandazioni. I modelli RNN addestrati sulla sequenza di eventi dell'utente in una sessione (ad esempio, prodotti visualizzati, data e ora delle interazioni) imparano a prevedere l'elemento successivo/i in una sessione. Le interazioni dell'utente con gli elementi in una sessione sono incorporate in modo simile alle parole in una frase. Ad esempio, i film visualizzati vengono tradotti in un insieme di numeri prima di essere inseriti in varianti RNN come LSTM, GRU o Transformer per comprenderne il contesto.

### Wide & Deep

Wide & Deep si riferisce a una classe di reti che utilizza l'output di due parti che lavorano in parallelo – modello ampio e modello profondo – i cui output vengono sommati per creare una probabilità di interazione. Il modello ampio è un modello lineare generalizzato di caratteristiche insieme alle loro trasformazioni. Il modello profondo è una rete neurale densa (DNN), una serie di cinque strati MLP nascosti di 1024 neuroni, ognuno dei quali inizia con un denso embedding di caratteristiche. Le variabili categoriali vengono incorporate in spazi vettoriali continui prima di essere inviate alla DNN tramite incorporamenti appresi o definiti dall'utente.
Ciò che rende questo modello così efficace per le attività di raccomandazione è che fornisce due canali di apprendimento dei pattern nei dati, "profondo" e "superficiale". La DNN complessa e non lineare è in grado di apprendere rappresentazioni complesse delle relazioni nei dati e di generalizzare a elementi simili tramite incorporamenti, ma ha bisogno di vedere molti esempi di queste relazioni per farlo bene. La parte lineare, d'altra parte, è in grado di "memorizzare" relazioni semplici che possono verificarsi solo una manciata di volte nel set di addestramento.
In combinazione, questi due canali di rappresentazione finiscono spesso per fornire una maggiore potenza di modellazione rispetto a ciascuno di essi singolarmente. NVIDIA ha collaborato con molti partner del settore che hanno segnalato miglioramenti nelle metriche offline e online utilizzando Wide & Deep in sostituzione dei modelli di apprendimento automatico più tradizionali.

![Recommendation system](/images/ia/recommendation11.png)

### DLRM

DLRM è un modello basato su DL per le raccomandazioni introdotto dalla ricerca di Facebook. È progettato per utilizzare input sia categoriali che numerici, solitamente presenti nei dati di addestramento del sistema di raccomandazione. Per gestire i dati categoriali, gli strati di embedding mappano ciascuna categoria in una rappresentazione densa prima di essere inseriti nei percettroni multistrato (MLP). Le feature numeriche possono essere inserite direttamente in un MLP.
Al livello successivo, le interazioni di secondo ordine tra diverse feature vengono calcolate esplicitamente calcolando il prodotto scalare tra tutte le coppie di vettori di embedding e le feature dense elaborate. Queste interazioni a coppie vengono inserite in un MLP di primo livello per calcolare la probabilità di interazione tra una coppia utente-elemento.

![Recommendation system](/images/ia/recommendation12.png)

Rispetto ad altri approcci di raccomandazione basati su DL, DLRM differisce per due motivi. In primo luogo, calcola l'interazione delle feature in modo esplicito, limitando l'ordine di interazione alle interazioni a coppie. In secondo luogo, DLRM tratta ogni vettore di feature incorporato (corrispondente alle feature categoriali) come una singola unità, mentre altri metodi (come Deep e Cross) trattano ogni elemento del vettore di feature come una nuova unità che dovrebbe produrre termini incrociati diversi. Queste scelte progettuali contribuiscono a ridurre i costi di calcolo/memoria, mantenendo al contempo un'accuratezza competitiva. DLRM fa parte di NVIDIA Merlin, un framework per la creazione di sistemi di raccomandazione ad alte prestazioni basati su DL, di cui parleremo di seguito.

## Perché i sistemi di raccomandazione funzionano meglio con le GPU

I sistemi di raccomandazione sono in grado di promuovere l'engagement sulle piattaforme consumer più diffuse. E con l'aumentare della scala dei dati (da decine di milioni a miliardi di esempi), le tecniche di apprendimento automatico (DL) stanno mostrando vantaggi rispetto ai metodi tradizionali. Di conseguenza, la combinazione di modelli più sofisticati e la rapida crescita dei dati hanno alzato l'asticella delle risorse computazionali.

Le operazioni matematiche alla base di molti algoritmi di apprendimento automatico sono spesso moltiplicazioni di matrici. Questi tipi di operazioni sono altamente parallelizzabili e possono essere notevolmente accelerate utilizzando una GPU.
Una GPU è composta da centinaia di core in grado di gestire migliaia di thread in parallelo. Poiché le reti neurali sono create da un gran numero di neuroni identici, sono per natura altamente parallele. Questo parallelismo si adatta naturalmente alle GPU, che possono offrire prestazioni 10 volte superiori rispetto alle piattaforme basate solo su CPU. Per questo motivo, le GPU sono diventate la piattaforma preferita per l'addestramento di sistemi basati su reti neurali di grandi dimensioni e complessi, e la natura parallela delle operazioni di inferenza si presta bene anche all'esecuzione su GPU.







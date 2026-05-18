---
title: "Alan Turing"
date: '2026-05-18'
author: Fabio Mattei
layout: page
---

## La mente che cambiò il corso della guerra e dell'informatica

Ci sono persone la cui vita è così densa di conseguenze da risultare quasi incredibile. Alan Turing è una di queste. In poco più di quarant'anni di esistenza contribuì a fondare l'informatica teorica, accorciò la Seconda Guerra Mondiale di anni salvando milioni di vite, gettò le basi di una branca intera della matematica applicata — e fu ripagato con l'umiliazione, la mutilazione chimica e la morte. La storia di Turing è inseparabile da quella delle macchine che pensano: è lui che ha posto la domanda, e lui che ha iniziato a costruire la risposta.

## La macchina immaginaria che definì il calcolo

![Alan Turing, fotografia di Elliott & Fry (1951, dominio pubblico)](/images/storia/alan-turing/alan-turing-1951.jpg){:class="aside-image" style="max-width: 35%;"}

Alan Turing nacque a Londra nel 1912. Fin da ragazzo mostrò un'attitudine per la matematica che andava ben oltre la semplice abilità tecnica: era attratto dai problemi fondamentali, da quelle domande che stanno un gradino sopra i calcoli e riguardano la natura stessa del ragionamento.

Negli anni Trenta, mentre completava il suo dottorato di ricerca a Cambridge e poi a Princeton, si trovò al centro di uno dei dibattiti più profondi della matematica del Novecento. Il matematico tedesco David Hilbert aveva posto una sfida all'intera comunità scientifica: esiste un metodo meccanico, una procedura definita, capace di determinare se qualsiasi affermazione matematica è dimostrabile oppure no? Era il cosiddetto *Entscheidungsproblem*, il "problema della decisione".

Turing risponde nel 1936 con un articolo destinato a diventare uno dei testi fondativi dell'informatica: *On Computable Numbers, with an Application to the Entscheidungsproblem*. La sua risposta alla domanda di Hilbert è negativa — non esiste un tale metodo universale — ma il modo in cui arriva a questa conclusione è ciò che rende il paper straordinario. Per dimostrare il suo teorema, Turing inventa un dispositivo concettuale: una macchina astratta, infinitamente semplice nella sua descrizione, capace di simulare qualsiasi procedura di calcolo.

La **macchina di Turing** è un nastro infinito diviso in celle, una testina che legge e scrive simboli, e un insieme finito di regole che determinano cosa fare in ogni situazione. Nient'altro. Eppure Turing dimostra che questa macchina è in grado di calcolare tutto ciò che è calcolabile — e che esiste una versione universale di essa capace di simulare qualsiasi altra macchina di Turing. Era, in termini teorici, la descrizione formale di un computer prima ancora che esistessero i computer. John von Neumann, che qualche anno dopo avrebbe progettato l'architettura dei calcolatori moderni, era perfettamente consapevole del debito verso questo lavoro.

## Bletchley Park e la rottura del codice Enigma

Quando nel settembre 1939 la Germania invase la Polonia e la Gran Bretagna entrò in guerra, Turing aveva già un accordo con il Government Code and Cypher School: avrebbe messo la sua mente al servizio dello sforzo bellico. Si trasferì a **Bletchley Park**, una villa vittoriana nel Buckinghamshire trasformata nel principale centro britannico di crittoanalisi, e lì rimase per tutta la durata del conflitto.

![La macchina Enigma usata dai tedeschi durante la Seconda Guerra Mondiale (CIA, dominio pubblico)](/images/storia/alan-turing/enigma-machine.jpg){:class="aside-image" style="max-width: 40%;"}

Il problema che attendeva era formidabile. I tedeschi cifravano le loro comunicazioni militari con la macchina **Enigma**: un dispositivo elettromeccanico che trasformava ogni lettera del testo in una lettera diversa attraverso una serie di rotori intercambiabili, un pannello di connessioni e un meccanismo di avanzamento che cambiava la configurazione a ogni pressione di tasto. Il numero di possibili configurazioni era astronomico — dell'ordine di 10 alla 114 — e le impostazioni venivano cambiate ogni giorno. Decifrare un messaggio senza conoscere la configurazione corrente era, in apparenza, matematicamente impossibile.

I polacchi avevano già fatto progressi significativi prima della guerra, riuscendo a ricostruire il funzionamento interno di Enigma e a costruire dispositivi meccanici — chiamati *bomba* — per cercare le configurazioni possibili. Turing prese quel lavoro come punto di partenza e lo portò a un livello completamente diverso.

La sua intuizione cruciale fu di sfruttare non la forza bruta, ma la struttura del problema. I messaggi tedeschi contenevano elementi prevedibili — formule di rito, indicatori meteorologici, parole ripetute — e Turing capì che queste "cribs", come venivano chiamate in gergo, potevano essere usate per escludere la stragrande maggioranza delle configurazioni possibili senza doverle testare una per una. Progettò una macchina elettromeccanica — la **Bombe britannica** — che implementava questo ragionamento in parallelo, scartando configurazioni incompatibili con i pattern noti e restringendo rapidamente il campo delle soluzioni da cercare.

![La Bombe di Bletchley Park, ricostruzione esposta al museo (CC BY-SA 3.0, Magnus Manske)](/images/storia/alan-turing/bombe-bletchley.jpg){:style="max-width: 50%;"}

Le prime Bombe entrarono in funzione nel 1940. Nel giro di mesi, i messaggi della Luftwaffe venivano decifrati con regolarità. Turing contribuì poi a rompere anche le cifrature della marina militare tedesca, le più difficili, che usavano varianti di Enigma con rotori aggiuntivi. L'accesso alle comunicazioni degli U-Boot tedeschi fu determinante nella Battaglia dell'Atlantico: sapere dove si trovavano i sommergibili permetteva ai convogli alleati di evitarli, e ai cacciatorpediniere di trovarli.

Gli storici discutono ancora sull'entità esatta del contributo di Bletchley Park all'esito della guerra, ma la stima più accreditata è che le operazioni di decifrazione abbiano abbreviato il conflitto di almeno due anni, evitando potenzialmente milioni di morti. Winston Churchill, quando gli fu chiesto chi aveva contribuito di più alla vittoria, avrebbe risposto: "Il professor Turing."

## Le basi della ricerca operativa

Parallelamente al lavoro su Enigma, Turing fu coinvolto in un'altra sfida che avrebbe avuto conseguenze durature oltre la guerra: come ottimizzare le decisioni in condizioni di incertezza e con risorse limitate.

La **ricerca operativa** — o *operations research* — è la branca della matematica che si occupa di risolvere problemi decisionali attraverso modelli formali. La domanda di fondo è sempre la stessa: data una situazione con certi obiettivi e certi vincoli, qual è la scelta migliore? Come disporre le navi di scorta per massimizzare la protezione di un convoglio? Come allocare le risorse di produzione per ridurre i costi? Come pianificare le rotte di rifornimento per minimizzare il tempo?

Durante la guerra, queste domande avevano risposte che si misuravano in vite umane. I matematici di Bletchley Park, Turing incluso, applicarono metodi quantitativi a problemi che prima venivano risolti per intuizione ed esperienza. Le tecniche sviluppate in quegli anni — modelli probabilistici, teoria delle decisioni, ottimizzazione sotto vincoli — sopravvissero alla fine del conflitto e diventarono strumenti fondamentali per l'industria, la logistica, la finanza e l'ingegneria gestionale. Ogni volta che un algoritmo pianifica un percorso di consegna, distribuisce turni di lavoro o ottimizza una catena di produzione, c'è un filo che risale a quel periodo.

## Il test di Turing e la questione delle macchine pensanti

Dopo la guerra, Turing tornò alla ricerca fondamentale. Lavorò allo sviluppo di uno dei primi computer reali — il Manchester Mark 1 — e cominciò a interrogarsi sulla domanda che avrebbe definito il suo lascito intellettuale più duraturo: possono le macchine pensare?

Nel 1950 pubblicò su *Mind* il saggio *Computing Machinery and Intelligence*, che apre con una delle frasi più citate della storia dell'informatica: "I propose to consider the question, 'Can machines think?'" Turing non risponde direttamente — ritiene la domanda mal posta — ma propone al suo posto un esperimento: il **gioco dell'imitazione**, che il mondo avrebbe poi conosciuto come **Test di Turing**. Un esaminatore umano comunica via testo con un interlocutore di cui non conosce la natura: se non riesce a distinguere in modo affidabile se sta parlando con una macchina o con un essere umano, la macchina supera il test.

Il test non è mai stato né una definizione definitiva di intelligenza né un criterio tecnico preciso — Turing stesso lo sapeva — ma ha avuto il merito di spostare il dibattito dalla metafisica alla pratica: invece di chiedersi se le macchine "davvero" pensano, si chiede cosa sarebbero capaci di fare. È una domanda che ancora oggi non ha risposta definitiva.

## L'arresto, la condanna e la morte

Nel gennaio del 1952, Turing denunciò un furto subito nella sua abitazione di Manchester. Nel corso delle indagini, la polizia scoprì che aveva una relazione con un altro uomo. In Gran Bretagna l'omosessualità era allora un reato penale, punibile con il carcere.

Turing non negò. Fu processato e condannato: poteva scegliere tra la prigione e la castrazione chimica — un trattamento ormonale con dietilstilbestrolo, uno stilbenoide estrogeno, che avrebbe dovuto "curare" la sua omosessualità. Scelse la castrazione chimica per poter continuare a lavorare.

Gli effetti fisici e psicologici del trattamento furono devastanti. Due anni dopo l'arresto, l'8 giugno 1954, Turing fu trovato morto nel suo letto. Accanto a lui, un mela con alcuni morsi. Il coroner stabilì il suicidio per avvelenamento da cianuro, anche se alcuni biografi hanno sollevato dubbi sull'accidentalità della morte.

Aveva quarantuno anni.

Nel 2009, cinquantacinque anni dopo la sua morte, il primo ministro britannico Gordon Brown si scusò pubblicamente a nome del governo per "il modo orribile" in cui Turing era stato trattato. Nel 2013 la Regina Elisabetta II gli concesse la grazia reale postuma. Nel 2021 la sua immagine è apparsa sulla banconota da 50 sterline della Banca d'Inghilterra.


Nessuno di questi gesti restituisce nulla. Ma dicono qualcosa sull'enormità di quello che fu fatto a un uomo che aveva salvato il mondo.

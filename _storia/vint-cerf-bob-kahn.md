---
title: "Vint Cerf e Bob Kahn"
date: '2026-07-27'
author: Fabio Mattei
layout: page
---

## Il primo messaggio: due lettere e un crash

Il 29 ottobre 1969, un computer dell'Università della California a Los Angeles tentò di inviare la parola "LOGIN" a un computer dello Stanford Research Institute, a centinaia di chilometri di distanza, attraverso una rete sperimentale finanziata dal governo americano. Il sistema si bloccò dopo aver trasmesso solo due lettere: "L" e "O". Fu, per un accidente tecnico quasi comico, il primo messaggio mai scambiato su quella che sarebbe diventata l'infrastruttura di comunicazione più importante della storia umana. Quella rete si chiamava **ARPANET**. Il problema di farla comunicare con altre reti simili, nate negli anni successivi, sarebbe stato risolto da due uomini: **Vint Cerf** e **Bob Kahn**.

## ARPANET: una rete, non ancora Internet

![Interface Message Processor (IMP), il primo apparato di instradamento a commutazione di pacchetto, esposto al Computer History Museum (foto di Jordiipa, CC BY-SA 3.0)](/images/storia/vint-cerf-bob-kahn/arpanet-imp.jpg){:style="max-width: 46%;"}

Alla fine degli anni Sessanta, l'agenzia governativa americana per la ricerca avanzata sulla difesa, l'**ARPA** (poi DARPA), finanziò la costruzione di una rete sperimentale per collegare i computer di diverse università e centri di ricerca statunitensi, permettendo loro di condividere risorse di calcolo costose e scarse. Il progetto si basava su un'idea relativamente nuova, sviluppata indipendentemente da Paul Baran negli Stati Uniti e Donald Davies nel Regno Unito: la **commutazione di pacchetto**, in cui i dati vengono spezzati in piccoli blocchi — pacchetti — instradati singolarmente attraverso la rete e riassemblati a destinazione, invece di richiedere un collegamento dedicato e continuo come nella telefonia tradizionale.

I nodi che gestivano questo instradamento erano gli **IMP** (Interface Message Processor), minicomputer dedicati costruiti dall'azienda Bolt Beranek and Newman (BBN) — antenati diretti di quelli che oggi chiameremmo router. ARPANET funzionava, ed era un successo per gli standard dell'epoca. Ma era, appunto, *una* rete: un sistema chiuso, con un proprio protocollo interno chiamato NCP (Network Control Program), pensato per collegare macchine omogenee all'interno di quella singola infrastruttura. Non era ancora pensata per collegarsi ad altre reti.

## Il problema del 1973: reti diverse che non si parlano

Nel giro di pochi anni, altre reti a commutazione di pacchetto nacquero con scopi diversi: una rete radio pacchettizzata per comunicazioni mobili, una rete satellitare per collegamenti internazionali. Ciascuna aveva caratteristiche tecniche proprie — velocità, affidabilità, dimensione massima dei pacchetti — incompatibili tra loro. Il problema che emerse a metà degli anni Settanta non era più "come costruire una rete", ma "come far comunicare reti diverse, con architetture diverse, come se fossero una sola" — un problema di **internetworking**.

![Vint Cerf, uno dei "padri di Internet" (foto di Duncan.Hull, CC BY-SA 4.0)](/images/storia/vint-cerf-bob-kahn/vint-cerf.jpg){:class="aside-image" style="max-width: 32%;"}

Fu **Bob Kahn**, allora all'ARPA, a porre il problema in questi termini, e a coinvolgere **Vint Cerf**, professore a Stanford che aveva già lavorato ai protocolli di ARPANET durante il dottorato a UCLA. I due iniziarono a lavorarci insieme nella primavera del 1973. Secondo il racconto che è diventato parte della leggenda fondativa di Internet, le prime intuizioni chiave del nuovo protocollo furono abbozzate su carta intestata di un hotel a Palo Alto, durante una sessione di lavoro in cui Cerf e Kahn provavano a immaginare come un singolo protocollo comune potesse nascondere, a livello di rete, tutte le differenze tecniche tra le infrastrutture sottostanti.

## TCP/IP: il linguaggio comune di reti diverse

Il risultato di quel lavoro fu pubblicato nel maggio 1974 in un articolo scientifico firmato da entrambi, intitolato *A Protocol for Packet Network Intercommunication*. Il protocollo che descriveva — il **TCP** (Transmission Control Protocol) — introduceva un'idea elegante: ogni rete sottostante poteva restare tecnicamente diversa dalle altre, purché esistessero dei dispositivi speciali — i **gateway**, oggi chiamati router — capaci di tradurre i pacchetti da un formato di rete all'altro, mentre il protocollo TCP garantiva, a un livello più alto, che i dati arrivassero a destinazione completi, nell'ordine giusto, indipendentemente da quante reti diverse avessero attraversato nel tragitto.

Nel 1978 il protocollo originale fu diviso in due componenti distinti: il **TCP** vero e proprio, responsabile dell'affidabilità della trasmissione, e l'**IP** (Internet Protocol), responsabile dell'indirizzamento e dell'instradamento dei singoli pacchetti. Questa separazione — nota oggi come architettura **TCP/IP** — permise anche la nascita di protocolli alternativi più leggeri e meno affidabili ma più veloci (come UDP) per le applicazioni che non necessitavano della garanzia di consegna completa offerta da TCP.

## Il giorno in cui nacque, davvero, Internet

![Bob Kahn, co-inventore del protocollo TCP/IP (foto di Veni Markovski, CC BY-SA 3.0)](/images/storia/vint-cerf-bob-kahn/bob-kahn.jpg){:class="aside-image" style="max-width: 32%;"}

Il passaggio da teoria a pratica richiese quasi un decennio di raffinamenti e test. Il momento simbolico più preciso in cui si può far coincidere la nascita di Internet, nel senso moderno del termine, è il **1° gennaio 1983** — ricordato negli ambienti tecnici come il "flag day": il giorno in cui ARPANET abbandonò definitivamente il vecchio protocollo NCP e passò in modo irreversibile a TCP/IP, diventando così, tecnicamente, parte di quella rete di reti — un *internet*, appunto — che oggi chiamiamo semplicemente Internet.

Da quel momento, qualsiasi rete che adottasse TCP/IP poteva collegarsi alle altre seguendo lo stesso linguaggio comune. È il motivo per cui, decenni dopo, la rete di un piccolo ufficio in Italia può scambiare dati con un server in Corea o un satellite in orbita: tutti parlano lo stesso protocollo che Cerf e Kahn abbozzarono su carta da lettere di un hotel nel 1973.

## Vite parallele, un solo lascito comune

Cerf proseguì la sua carriera come dirigente e figura pubblica dell'industria di Internet: lavorò a DARPA come program manager supervisionando la ricerca sulle reti a pacchetto, contribuì al primo sistema di posta elettronica commerciale su larga scala (MCI Mail), e dal 2005 ricopre in Google il ruolo, quasi onorifico ma sostanziale, di "Chief Internet Evangelist" — promotore instancabile di un Internet aperto e universalmente accessibile. Kahn fondò nel 1986 la **Corporation for National Research Initiatives**, dedicandosi a progetti di infrastruttura digitale a lungo termine, tra cui l'architettura per l'identificazione permanente degli oggetti digitali.

Nel 2004 Cerf e Kahn ricevettero insieme il **Turing Award**, il riconoscimento più prestigioso dell'informatica mondiale, per l'invenzione dei protocolli TCP/IP. L'anno successivo il presidente George W. Bush conferì a entrambi la **Presidential Medal of Freedom**. Sono ricordati, insieme, come i "padri di Internet" — un titolo che rende giustizia non a un singolo momento di ispirazione, ma a un problema tecnico specifico, risolto insieme, che ha reso possibile trasformare una collezione di reti incompatibili in un'unica infrastruttura globale.

## Crediti immagini

- Interface Message Processor (IMP), Computer History Museum: foto di Jordiipa, [licenza CC BY-SA 3.0](https://creativecommons.org/licenses/by-sa/3.0/) — [fonte: Wikimedia Commons](https://commons.wikimedia.org/wiki/File:CHM_IMP.JPG)
- Vint Cerf: foto di Duncan.Hull, [licenza CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/) — [fonte: Wikimedia Commons](https://commons.wikimedia.org/wiki/File:Dr_Vint_Cerf_ForMemRS.jpg)
- Bob Kahn: foto di Veni Markovski, [licenza CC BY-SA 3.0](https://creativecommons.org/licenses/by-sa/3.0/) — [fonte: Wikimedia Commons](https://commons.wikimedia.org/wiki/File:Bob_Kahn.jpg)

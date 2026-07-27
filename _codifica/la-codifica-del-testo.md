---
id: 560
title: 'La codifica del testo'
date: '2020-10-06T23:56:27+02:00'
author: Fabio Mattei
layout: page
guid: 'https://www.esercizidiinformatica.it/?page_id=560'
---

![ASCII](/images/codifica/ascii.gif)

La tabella ASCII (acronimo di American Standard Code for Information Interchange, Codice Standard Americano per lo 
Scambio di Informazioni) è un codice per la codifica di caratteri inventato per creare una corrispondenza tra numeri 
e caratteri digitabili da tastiera.

Gli elaboratori elettronici, possono soltanto lavorare con i simboli 1 e 0, per tanto c’era la necessità di 
associare ad ogni carattere una sequenza di 1 e di 0.

**Bob Bemer**, nel **1961**, risolse questo problema inventando la tabella ASCII.

La tabella ASCII associa a ciascun simbolo un numero che può essere facilmente convertito in numero binario.

### Perché serve una tabella di codifica

Un computer, come abbiamo visto parlando della [codifica dei numeri]({{site.baseurl}}/codifica/la-codifica-dei-numeri.html), 
può rappresentare soltanto sequenze di 0 e 1. Un testo, però non è fatto di numeri ma di **lettere**, **cifre**, 
**segni di punteggiatura** e **simboli** (spazio, punto, virgola, ecc.).

Per poter memorizzare ed elaborare un testo, il computer ha quindi bisogno di una convenzione condivisa che stabilisca, 
per ogni carattere, quale numero (e quindi quale sequenza di bit) lo rappresenta. Questa convenzione è la **tabella di 
codifica dei caratteri**, e la più famosa e diffusa è proprio la tabella **ASCII**.

### Come è fatta la tabella ASCII

La tabella ASCII originale utilizza **7 bit** per ogni carattere: con 7 bit si possono rappresentare 2⁷ = **128 simboli 
diversi** (numerati da 0 a 127),. Questi 128 codici comprendono:

- le **lettere maiuscole** dalla A alla Z;
- le **lettere minuscole** dalla a alla z;
- le **cifre** da 0 a 9;
- i principali **segni di punteggiatura** (. , ; : ! ? ecc.);
- alcuni **caratteri di controllo**, non stampabili, usati per esempio per andare a capo (*newline*) o per segnalare la fine di un testo.

In seguito è stata definita anche una versione **estesa** della tabella, che utilizza **8 bit** (un intero byte) per 
carattere, arrivando così a rappresentare **256 simboli**. Gli ulteriori 128 codici sono stati sfruttati da diversi 
produttori per aggiungere lettere accentate, simboli grafici e caratteri specifici di alcune lingue: per questo 
motivo esistono diverse "tabelle ASCII estese", non sempre compatibili tra loro.

### Esempio

Codifichiamo la parola CIAO. Ad ogni lettera associamo il corrispondente numero nella tabella ASCII.

| C | I | A | O |
|---|---|---|---|
| 67 | 73 | 65 | 79 |

Notiamo che le lettere maiuscole e le lettere minuscole sono codificate in modo diverso perché per l’elaboratore 
sono simboli diversi e indipendenti.

| C | i | a | o |
|---|---|---|---|
| 67 | 105 | 97 | 111 |

### Dal carattere al binario

Ogni numero della tabella ASCII può essere convertito in binario esattamente come abbiamo visto nell'articolo sulla 
[codifica dei numeri]({{site.baseurl}}/codifica/la-codifica-dei-numeri.html). Ad esempio la lettera **C**, che nella 
tabella ASCII corrisponde al numero 67, viene memorizzata dal computer come:

| carattere | numero ASCII | codifica binaria (8 bit) |
|---|---|---|
| C | 67 | 01000011 |
| I | 73 | 01001001 |
| A | 65 | 01000001 |
| O | 79 | 01001111 |

Quindi la parola CIAO, per il computer, non è altro che la sequenza di bit `01000011 01001001 01000001 01001111`, 
cioè **4 byte**, uno per ogni lettera.

### I limiti della tabella ASCII

La tabella ASCII è nata negli Stati Uniti ed è pensata per la lingua inglese, che non utilizza lettere accentate 
o alfabeti diversi da quello latino. Questo crea un problema quando dobbiamo codificare:

- lingue con **lettere accentate** (come l'italiano: à, è, ì, ò, ù);
- alfabeti **non latini** (cirillico, greco, arabo, cinese, giapponese, ecc.);
- simboli particolari (ad esempio le **emoji** 😀).

Con soli 256 codici disponibili (usando 8 bit) non c'è spazio sufficiente per rappresentare tutti i caratteri di 
tutte le lingue del mondo insieme.

### Unicode e UTF-8

Per risolvere questo problema è stato creato lo standard **Unicode**, che assegna un numero univoco (chiamato 
*code point*) a ogni carattere esistente in qualsiasi lingua o sistema di scrittura, comprese le emoji. Mentre 
l'ASCII prevede al massimo 256 simboli, Unicode ne prevede oltre un milione.

Per memorizzare questi numeri in modo efficiente si usano delle codifiche come **UTF-8**, che ha una caratteristica 
molto importante: utilizza un **numero variabile di byte** per ogni carattere.

- i caratteri più comuni (quelli della tabella ASCII originale, come le lettere dell'alfabeto inglese e le cifre) 
  vengono codificati con **1 solo byte**, esattamente come nella tabella ASCII classica;
- i caratteri accentati e degli altri alfabeti europei richiedono **2 byte**;
- molti caratteri delle lingue asiatiche richiedono **3 byte**;
- le emoji e altri simboli più rari richiedono fino a **4 byte**.

Questo rende UTF-8 **compatibile con l'ASCII**: un testo scritto solo in caratteri inglesi risulta identico sia che 
venga letto come ASCII sia che venga letto come UTF-8, e allo stesso tempo permette di rappresentare qualsiasi 
lingua o simbolo del mondo. Per questo motivo UTF-8 è oggi la codifica di gran lunga più utilizzata su siti web, 
sistemi operativi e applicazioni.

### Quanto spazio occupa un testo?

Conoscere la codifica utilizzata permette di calcolare quanta memoria occupa un testo. Ad esempio, la parola 
**CIAO**, codificata in ASCII (o in UTF-8, dato che si tratta di sole lettere dell'alfabeto inglese), occupa 
**4 byte**, uno per ogni carattere.

Se invece dovessimo codificare la parola **città**, che contiene una lettera accentata, in UTF-8 occuperebbe 
**5 lettere ma 6 byte**: le lettere "c", "t", "t", "a" occupano 1 byte ciascuna, mentre la "à" accentata, non 
presente nell'ASCII originale, richiede 2 byte.



---
title: 'La codifica del software'
date: '2026-07-27T00:00:00+02:00'
author: Fabio Mattei
layout: page
---

![Codice sorgente tradotto in eseguibile tramite compilatore o interprete](/images/codifica/la-codifica-del-software/la-codifica-del-software.svg){:class="aside-image"}

Anche il software, cioè i programmi che il computer esegue, deve essere codificato in 0 e 1, esattamente come abbiamo visto per i numeri, il testo, le immagini e i video.

Un programma esiste però in due forme molto diverse tra loro: il **codice sorgente** e il **file eseguibile**.

### Il codice sorgente

Il codice sorgente è il programma così come viene scritto dal programmatore, utilizzando un **linguaggio di programmazione** (Python, C++, Java, ecc.).

Il codice sorgente è, a tutti gli effetti, un **file di testo**: viene scritto con un editor o un IDE, si può aprire e leggere con un qualsiasi programma per la gestione di testo, e viene salvato utilizzando una codifica di caratteri come **ASCII** o **UTF-8**, esattamente come abbiamo visto nell'articolo sulla [codifica del testo]({{site.baseurl}}/codifica/la-codifica-del-testo.html).

Un esempio di codice sorgente scritto in Python:

```python
print("Ciao mondo")
```

Queste righe sono, per il computer, semplicemente una sequenza di caratteri (e quindi di byte) come qualsiasi altro testo. Il **processore**, tuttavia, non è in grado di eseguire direttamente il codice sorgente: la CPU comprende soltanto un insieme molto limitato di istruzioni elementari, chiamato **linguaggio macchina**, espresso interamente in 0 e 1.

### Il file eseguibile

Per poter essere eseguito, il codice sorgente deve quindi essere **tradotto** in linguaggio macchina. Questa traduzione produce un **file eseguibile**, cioè un file binario contenente direttamente le istruzioni che il processore è in grado di eseguire.

A differenza del codice sorgente, un file eseguibile:

- **non è un file di testo**: aprendolo con un editor di testo si vedono caratteri senza senso, perché i byte non rappresentano caratteri ma **istruzioni macchina** e dati;
- **non è pensato per essere letto o scritto da un essere umano**, ma soltanto eseguito dal computer;
- è specifico per un particolare **tipo di processore** e **sistema operativo**: un eseguibile creato per Windows su processore Intel, ad esempio, normalmente non funziona su un Mac con processore Apple Silicon.

### Compilazione e interpretazione

Esistono due strategie principali per passare dal codice sorgente all'esecuzione:

- **compilazione**: un programma chiamato **compilatore** traduce tutto il codice sorgente in un file eseguibile in un solo passaggio, prima di eseguire il programma. Linguaggi come **C** e **C++** funzionano così;
- **interpretazione**: un programma chiamato **interprete** legge il codice sorgente riga per riga e lo esegue immediatamente, senza produrre un file eseguibile separato. Linguaggi come **Python** funzionano prevalentemente così.

Alcuni linguaggi, come **Java**, usano una via intermedia: il codice sorgente viene compilato in un **bytecode**, una forma intermedia tra il codice sorgente e il linguaggio macchina, che viene poi eseguito da un programma chiamato **macchina virtuale**.

| forma del programma | leggibile dall'uomo | formato | esempio di estensione |
|---|---|---|---|
| codice sorgente | sì | testo | `.py`, `.c`, `.java` |
| eseguibile / bytecode | no | binario | `.exe`, `.class`, `.pyc` |

### Perché il codice sorgente resta importante

Se il computer esegue soltanto il linguaggio macchina, perché conservare anche il codice sorgente? Perché il codice sorgente, essendo testo leggibile, è l'unica versione del programma che un essere umano può capire, correggere e modificare. Un file eseguibile, una volta prodotto a partire da istruzioni binarie, è di fatto illeggibile per una persona: per questo motivo i programmatori condividono e conservano il codice sorgente, e producono l'eseguibile soltanto al momento di eseguire o distribuire il programma.

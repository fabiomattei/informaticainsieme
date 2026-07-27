---
title: 'Lezione 06 — Come nasce una risposta, parola per parola'
date: '2026-07-27T00:00:00+02:00'
author: Fabio Mattei
layout: page
---

### 6.1 Non una risposta, ma un mazzo di probabilità

Ecco un fraintendimento comune da sfatare subito: quando chiedi qualcosa a un chatbot, il modello **non calcola una risposta e basta**. A ogni singolo passo — per ogni singola parola che sta per scrivere — il modello produce internamente qualcosa di più simile a un mazzo di carte, dove ogni carta è una parola possibile e ha scritto sopra quanto è probabile che sia quella giusta. Per continuare "Il gatto si è arrampicato su un...", il mazzo potrebbe avere in cima "albero" con una probabilità alta, "tetto" poco più sotto, "armadio" ancora più giù, e così via fino a migliaia di parole con probabilità infinitesima ("frigorifero", "sottomarino"...).

Il vero problema non è calcolare questo mazzo di probabilità — quello lo abbiamo già visto, è il risultato di tutta la macchina delle lezioni precedenti. Il problema è: **una volta che hai il mazzo, quale carta scegli?**

### 6.2 La scelta più ovvia — e perché è noiosa

La strategia più ovvia è: prendi sempre la carta più probabile. Si chiama scelta **golosa** (greedy). Sembra ragionevole, ma ha un difetto pratico fastidioso: tende a produrre testo ripetitivo e prevedibile, perché ogni volta che c'è un bivio (due parole quasi ugualmente probabili), il modello sceglie sempre e solo la stessa, cadendo facilmente in loop tipo "il gatto è un animale che è un animale che è un animale...". Inoltre, scegliere sempre e solo la carta più alta significa che, a parità di domanda, il modello darebbe **sempre esattamente la stessa risposta, parola per parola** — il che spiegherebbe perché a volte i chatbot sembrano un po' più "vivi" e vari di così.

### 6.3 Un dado invece di una scelta fissa

La strategia alternativa, usata dalla maggior parte dei chatbot che usi ogni giorno, è **pescare** dal mazzo invece di prendere sempre la carta in cima — ma pescare in modo che le carte più probabili abbiano più chance di uscire, esattamente come un'urna con palline pesate: se "albero" ha probabilità 60% e "tetto" ha probabilità 25%, immagina un'urna con 60 palline "albero", 25 palline "tetto", e così via per tutte le altre parole possibili, in proporzione alla loro probabilità. Estrai una pallina a caso: quasi sempre uscirà "albero" o "tetto", ma ogni tanto — non spessissimo, ma non mai — uscirà qualcos'altro, magari "ramo" o "cornicione". Questo si chiama **campionamento** (sampling), ed è il motivo per cui la stessa identica domanda, fatta due volte allo stesso chatbot, può produrre due risposte leggermente diverse.

### 6.4 La temperatura: quanto rischia il dado

Qui entra in gioco un parametro che forse hai già visto nominare, se hai usato strumenti più avanzati per parlare con un LLM: la **temperatura**. Pensala come una manopola che ridisegna l'urna prima di pescare:

- **Temperatura bassa** (vicina a zero): l'urna viene "schiacciata" per esasperare ancora di più le differenze — la pallina più probabile arriva quasi a riempire l'urna da sola, le altre quasi scompaiono. Il risultato è un testo prevedibile, quasi deterministico, adatto a compiti dove vuoi la risposta "più probabile e sicura" (un calcolo, un fatto preciso).
- **Temperatura alta**: l'urna viene invece "appiattita" — anche le palline meno probabili diventano quasi altrettanto numerose di quella in testa. Il risultato è un testo più sorprendente, creativo, a volte bizzarro o meno coerente: utile per scrivere una poesia o una storia di fantasia, rischioso per un fatto storico dove vuoi precisione.

Una tecnica complementare, usata quasi sempre insieme alla temperatura, è restringere prima il mazzo alle sole carte più plausibili — scartando in partenza la lunghissima coda di parole con probabilità quasi nulla ("frigorifero", "sottomarino") — e pescare solo tra quelle rimaste. È come giocare a carte con un mazzo ridotto alle carte sensate per quella mano, invece che con l'intero mazzo da 52 sempre in tavola: evita che, per pura sfortuna nell'estrazione, esca ogni tanto qualcosa di completamente assurdo.

### 6.5 Perché la stessa domanda dà risposte diverse

Mettendo insieme i pezzi: ogni parola della risposta di un chatbot nasce da (1) il calcolo del mazzo di probabilità, frutto di tutto ciò che abbiamo visto nelle lezioni precedenti, seguito da (2) un'estrazione — più o meno rischiosa a seconda della temperatura — da quel mazzo. Ripetuto parola dopo parola, questo processo costruisce l'intera risposta un pezzo alla volta, senza che il modello "sappia" in anticipo l'intera frase che sta per scrivere: proprio come te, quando parli a braccio, spesso non sai esattamente come finirà la frase che hai appena iniziato.

---

> **Prova tu — L'urna a due temperature**
>
> Costruisci due urne giocattolo (bastano dei foglietti in un sacchetto, o carte da gioco) per completare la frase: **"Il gatto si è arrampicato su un..."**, con queste probabilità di partenza per quattro parole candidate:
>
> | Parola | Probabilità originale |
> |---|---|
> | albero | 60% |
> | tetto | 25% |
> | ramo | 10% |
> | frigorifero | 5% |
>
> **Urna a bassa temperatura**: rendi le proporzioni ancora più estreme a favore della parola già più probabile — ad esempio 85% albero, 12% tetto, 2% ramo, 1% frigorifero. Scrivi quante palline/foglietti useresti per ciascuna parola se l'urna ne contenesse 100 in totale.
>
> **Urna ad alta temperatura**: appiattisci invece le proporzioni, avvicinandole tutte a un quarto ciascuna (25% ciascuna). Scrivi di nuovo quante palline userebbe ciascuna parola su 100 totali.
>
> Ora rispondi: in quale delle due urne è più probabile — pur restando un evento raro — pescare "frigorifero"? E in quale urna, pescando 5 volte di fila (rimettendo ogni volta la pallina estratta), ti aspetti di ottenere quasi sempre la stessa identica parola? Ragionamento completo in Appendice A.

---

*Continua con la [Lezione 07 — Quanto ci possiamo fidare]({{ site.baseurl }}{% link _ia/come-pensano-le-macchine-lezione-07-quanto-ci-possiamo-fidare.md %}.html)*

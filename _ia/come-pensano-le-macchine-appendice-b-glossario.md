---
title: 'Appendice B — Glossario'
date: '2026-07-27T00:00:00+02:00'
author: Fabio Mattei
layout: page
---

![Una schedina del glossario in primo piano su una pila di altre schede, con termine, definizione e rimando alla lezione](/images/ia/come-pensano-le-macchine-appendice-b-glossario/come-pensano-le-macchine-appendice-b-glossario.svg){:class="aside-image"}

Termini in ordine alfabetico, con un rimando alla lezione dove sono spiegati per la prima volta. Nessuna definizione qui contiene formule: per la versione tecnica e formale di ciascun termine, vedi il glossario del libro "Come Funzionano i Large Language Model".

**Agente** — un LLM che, invece di rispondere in un colpo solo, ripete un ciclo di "pensa, agisci, osserva il risultato, decidi il prossimo passo", incatenando ricerche e strumenti in autonomia. Lezione 9.

**AI costituzionale** — tecnica per allenare un modello a criticare e correggere le proprie risposte confrontandole con un piccolo insieme di principi scritti, invece di far correggere ogni risposta da una persona. Lezione 8.

**Allucinazione** — quando un modello produce con sicurezza un'informazione inventata o falsa, presentandola con lo stesso stile di un fatto vero; non è un guasto occasionale ma una conseguenza diretta di come funziona il meccanismo di previsione. Lezione 8.

**Arena** — piattaforma dove persone confrontano alla cieca le risposte di due chatbot diversi alla stessa domanda, votando quella preferita, per costruire una classifica di gradimento. Lezione 7.

**Attenzione** — il meccanismo con cui, per ogni parola di un testo, il modello decide quanto "ascoltare" ciascuna delle altre parole per interpretarla correttamente nel contesto. Lezione 3.

**Benchmark** — una raccolta di domande con risposta nota, usata per misurare oggettivamente quanto è capace un modello, come un'interrogazione. Lezione 7.

**Bias (pregiudizio)** — associazioni statistiche distorte, ereditate dai testi di addestramento, che un modello può riprodurre nelle proprie risposte senza che nessuno gliele abbia insegnate esplicitamente. Lezione 8.

**Campionamento (sampling)** — pescare la parola successiva da un "mazzo di probabilità" invece di scegliere sempre e solo la più probabile, un po' come estrarre da un'urna con palline pesate. Lezione 6.

**Capacità emergenti** — capacità che sembrano comparire all'improvviso oltre una certa dimensione del modello; spesso, misurando con un metro più fine, si scopre che il miglioramento era in realtà graduale fin dall'inizio. Lezione 7.

**Catena di pensiero (chain-of-thought)** — chiedere al modello di scrivere il proprio ragionamento passo passo prima della risposta finale, spesso migliorando l'accuratezza su problemi complessi. Lezione 7.

**Contaminazione del test** — quando le domande (e a volte le risposte) di un benchmark erano già presenti nel testo di addestramento del modello, rendendo il punteggio ottenuto poco significativo. Lezione 7.

**Distillazione** — allenare un modello piccolo (l'"allievo") a imitare il comportamento di un modello grande già addestrato (il "maestro"), invece di ripartire da zero. Lezione 10.

**DPO** — tecnica di post-training che spinge il modello a preferire direttamente le risposte già indicate come migliori in una coppia, senza passare da un modello-giudice separato. Lezione 5.

**Embedding** — il punto sulla "mappa dei significati" a cui viene associato ogni pezzetto di testo (token); parole con significato simile finiscono vicine sulla mappa. Lezione 2.

**Jailbreak** — una tecnica per convincere, con astuzia o inganno, un modello a produrre un contenuto che normalmente rifiuterebbe. Lezione 8.

**LLM (Large Language Model)** — un modello linguistico di grandi dimensioni: un sistema allenato a indovinare, una parola alla volta, la continuazione più plausibile di un testo. Lezione 1.

**LLM-as-judge** — usare un secondo modello come "giudice" per confrontare due risposte e dire quale preferisce, utile quando non esiste un'unica risposta giusta da controllare meccanicamente. Lezione 7.

**LoRA** — tecnica che specializza un modello già addestrato aggiungendo solo un piccolo numero di manopole nuove, lasciando congelate quelle originali, invece di riaddestrare tutto da capo. Lezione 10.

**Mascheramento causale** — la regola per cui, indovinando una parola, il modello può guardare solo alle parole precedenti, mai a quelle successive che ancora non esistono al momento della previsione. Lezione 3.

**Multi-head attention (attenzione multi-testa)** — far girare più meccanismi di attenzione in parallelo, ciascuno specializzato a cogliere un tipo diverso di relazione tra le parole. Lezione 3.

**Parametri (o pesi)** — le "manopole" numeriche interne di un modello, regolate durante l'addestramento; sono loro a determinare tutto il comportamento del modello. Lezione 4.

**Post-training** — la fase di addestramento successiva al pre-training, il cui scopo è insegnare al modello le buone maniere di un assistente (rispondere utilmente, rifiutare richieste pericolose), non nuove nozioni sul mondo. Lezione 5.

**Pre-training** — la prima e più lunga fase di addestramento, in cui il modello impara a indovinare parole cancellate leggendo enormi quantità di testo. Lezione 4.

**Prompt injection** — istruzioni nascoste in un contenuto esterno (una pagina web, un documento) scritte apposta per ingannare un agente e fargli fare qualcosa di diverso da ciò che l'utente legittimo ha chiesto. Lezione 9.

**Potatura (pruning)** — rimuovere dal modello le manopole interne che contribuiscono pochissimo al comportamento finale, per renderlo più piccolo e veloce. Lezione 10.

**Quantizzazione** — arrotondare i numeri interni di un modello a una precisione inferiore, per farlo occupare meno memoria e girare più velocemente con una perdita di qualità minima. Lezione 10.

**RAG (Retrieval-Augmented Generation)** — cercare automaticamente, prima di rispondere, i documenti più pertinenti a una domanda e inserirli nel contesto, per dare al modello accesso a informazioni che non ha mai "studiato" durante l'addestramento. Lezione 9.

**Red teaming** — il processo sistematico di provare a "rompere" un modello con richieste sempre più creative per scoprire e correggere le sue falle prima del rilascio pubblico. Lezione 8.

**Rendimenti decrescenti** — il fenomeno per cui ogni unità aggiuntiva di studio (o di addestramento) porta miglioramenti sempre più piccoli, man mano che si continua. Lezione 4.

**RLHF (Reinforcement Learning from Human Feedback)** — allenare un modello a produrre risposte sempre più gradite, usando le preferenze espresse da persone (o da un altro modello) per costruire un segnale di "premio". Lezione 5.

**Robustezza** — quanto un modello resta stabile e corretto anche di fronte a piccole modifiche, ininfluenti per un umano, nel modo in cui è posta una domanda. Lezione 8.

**SFT (Supervised Fine-Tuning)** — continuare l'addestramento di un modello già pre-addestrato su esempi scritti apposta di "domanda seguita da buona risposta", per insegnargli lo stile di un assistente. Lezione 5.

**Temperatura** — un parametro che regola quanto "rischia" il modello nello scegliere la parola successiva: bassa temperatura produce testo prevedibile, alta temperatura produce testo più vario e creativo. Lezione 6.

**Token** — il pezzetto elementare in cui viene spezzato un testo prima di essere trasformato in numeri; spesso non coincide con una parola intera. Lezione 2.

**Tokenizzazione** — il processo di spezzare un testo in token. Lezione 2.

**Transformer** — l'architettura, proposta nel 2017, costruita interamente attorno al meccanismo di attenzione; alla base della quasi totalità dei chatbot moderni. Lezione 1.

---

*Torna all'[indice — Prefazione](prefazione.md)*

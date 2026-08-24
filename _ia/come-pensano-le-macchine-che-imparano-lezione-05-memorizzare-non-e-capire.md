---
title: 'Lezione 05, Memorizzare non è capire'
date: '2026-08-24T00:00:00+02:00'
author: Fabio Mattei
layout: page
---

![Due curve d'errore che si separano nel tempo: quella di addestramento scende sempre, quella di validazione torna a salire](/images/ia/come-pensano-le-macchine-che-imparano-lezione-05-memorizzare-non-e-capire/come-pensano-le-macchine-che-imparano-lezione-05-memorizzare-non-e-capire.svg){:class="aside-image"}

### 5.1 Le tremila domande a memoria

Immagina uno studente che deve prepararsi per il quiz di teoria della patente. Trova online l'elenco completo delle tremila domande ufficiali con le relative risposte, e passa le settimane successive a memorizzarle una per una, parola per parola. Il giorno dell'esame, se le domande sono esattamente quelle studiate, risponde a tutto correttamente: sembra un genio del codice della strada.

Ma mettiamolo alla guida vera, su una strada reale, davanti a una situazione che nessuna delle tremila domande descriveva esattamente, un cartello coperto da un ramo, un incrocio con una segnaletica inconsueta. Se non ha mai davvero capito *perché* certe regole esistono, e si è limitato a far corrispondere domande a risposte memorizzate, andrà in difficoltà. Ha imparato le tremila domande, non il codice della strada.

Questo è precisamente il rischio più insidioso di ogni modello di machine learning, ed è il motivo per cui questa lezione arriva proprio ora, dopo aver visto tre algoritmi diversi (k-NN, alberi decisionali, regressione lineare) che condividono tutti lo stesso pericolo.

### 5.2 Overfitting: quando il modello memorizza invece di generalizzare

Un modello che si comporta come lo studente delle tremila domande, perfetto sugli esempi già visti, ma scadente su casi nuovi mai incontrati prima, soffre di **overfitting** (letteralmente, "sovra-adattamento"): si è adattato talmente bene ai dati di addestramento, comprese le loro particolarità casuali e il rumore, da aver smesso di catturare la regola generale sottostante, e aver iniziato invece a memorizzare i casi specifici.

Torniamo agli algoritmi delle lezioni precedenti per vedere come l'overfitting si manifesta concretamente in ciascuno.

In **k-NN** (Lezione 2), un valore di k troppo piccolo, in particolare k = 1, fa sì che ogni previsione dipenda da un singolo esempio noto: un'anguria etichettata per errore, o semplicemente anomala, "contagia" ogni punto nuovo che le capita vicino, anche se non rappresenta affatto la tendenza generale della zona.

In un **albero decisionale** (Lezione 3), un albero lasciato crescere senza limiti finisce per porre domande sempre più specifiche, fino a isolare un singolo esempio strano dentro una foglia tutta sua, una regola così particolare da valere solo per quell'unico caso, priva di qualunque valore predittivo su un esempio nuovo.

Nella **regressione lineare** (Lezione 4), l'overfitting è meno immediato con una sola retta, ma diventa evidente non appena si permettono curve più elaborate di una semplice linea retta: una curva abbastanza flessibile può essere piegata fino a passare esattamente su ogni singolo punto noto, ma nel farlo comincia a serpeggiare in modo assurdo fra un punto e l'altro, inseguendo il rumore casuale dei dati invece della tendenza reale.

### 5.3 Underfitting: l'altro estremo

Esiste anche l'errore opposto. Uno studente che, invece di studiare le tremila domande, si limitasse a leggere una sola pagina riassuntiva troppo generica ("guida con prudenza") non sarebbe pronto nemmeno per le domande più elementari dell'esame: non ha memorizzato troppo, ha semplicemente imparato troppo poco.

Un modello che si comporta così, k troppo grande in k-NN (che finisce per prevedere sempre la stessa etichetta più comune, ignorando ogni indizio locale), un albero fermato dopo una sola domanda, una retta forzata su dati che seguono in realtà un andamento curvo, soffre di **underfitting** (letteralmente, "sotto-adattamento"): è troppo semplice per catturare nemmeno i pattern più evidenti già presenti negli esempi di addestramento, figuriamoci quelli su dati nuovi.

Il segno distintivo che separa i due problemi è dove si manifesta l'errore. Nell'underfitting, il modello sbaglia parecchio *anche* sugli esempi che ha già visto durante l'addestramento, non ha imparato bene nemmeno quelli. Nell'overfitting, il modello va benissimo sugli esempi già visti (a volte alla perfezione) ma sbaglia parecchio su esempi nuovi, ha imparato quelli a memoria, non la regola generale che li collega.

### 5.4 Come scoprire quale dei due sta succedendo: dividere i dati

Il problema pratico è che, guardando solo l'errore sugli esempi di addestramento, overfitting e "modello perfetto" sono indistinguibili: entrambi mostrano un errore bassissimo su quei dati. Serve un modo per misurare, separatamente, quanto il modello si comporta bene su dati che non ha mai visto durante l'apprendimento, proprio come lo studente andrebbe davvero testato con domande mai incontrate prima, non con le stesse tremila già studiate a memoria.

La soluzione standard è dividere fin dall'inizio gli esempi disponibili in due gruppi separati, tenuti rigorosamente distinti per tutto il processo:

- L'**insieme di addestramento** (*training set*), su cui il modello viene effettivamente costruito, le domande che lo studente studia.
- L'**insieme di test**, tenuto completamente da parte, mai mostrato al modello durante l'addestramento, usato solo alla fine per misurare quanto il modello se la cava su casi davvero nuovi, l'esame vero, con domande mai viste.

Una regola pratica comune è usare circa l'80% degli esempi disponibili per l'addestramento e il restante 20% per il test, scelti a caso in modo che entrambi i gruppi siano rappresentativi dell'insieme originale. Il numero esatto non è sacro, dipende da quanti dati hai a disposizione in totale, ma il principio di tenerli separati sì, sempre.

Spesso si usa in realtà una terza suddivisione, l'**insieme di validazione**: un secondo gruppo, distinto sia dal training che dal test, usato durante lo sviluppo per confrontare più modelli o più impostazioni (per esempio, provare diversi valori di k) senza mai toccare l'insieme di test, che resta riservato a una verifica finale, una sola volta, quando ogni decisione è già stata presa. Usare l'insieme di test troppe volte per aggiustare le proprie scelte, infatti, rischia di farlo diventare, di fatto, un secondo insieme di addestramento, annullandone lo scopo.

### 5.5 La curva della complessità

Mettendo insieme quanto visto, emerge un quadro utile per qualunque algoritmo di questo libro: al crescere della **complessità** del modello, k sempre più piccolo, albero sempre più profondo, curva sempre più flessibile, l'errore sull'insieme di addestramento scende quasi sempre, con costanza, fino quasi ad annullarsi. L'errore sull'insieme di validazione, invece, scende all'inizio (il modello sta davvero imparando la regola generale), ma da un certo punto in poi torna a salire (il modello ha smesso di generalizzare e ha iniziato a memorizzare le particolarità dell'insieme di addestramento).

Il punto in cui l'errore di validazione smette di scendere e comincia a risalire indica, approssimativamente, il livello di complessità giusto per quel problema specifico: né così semplice da sotto-adattare, né così complesso da sovra-adattare. Trovare questo punto, spesso provando diverse impostazioni e confrontando l'errore di validazione, è una delle attività più concrete e ricorrenti di chi costruisce modelli di machine learning nella pratica.

---

> **Prova tu, Diagnostica due modelli**
>
> Due modelli sono stati addestrati sullo stesso problema di classificazione di angurie. Ecco il loro errore (in percentuale di angurie classificate male, quindi più basso è meglio) sull'insieme di addestramento e sull'insieme di test:
>
> | Modello | Errore addestramento | Errore test |
> |---|---|---|
> | Modello X | 1% | 34% |
> | Modello Y | 28% | 31% |
>
> 1. Il Modello X ha un errore bassissimo sui dati di addestramento, ma molto più alto sul test. Come si chiama questo problema, e cosa suggerisce sul comportamento del modello?
> 2. Il Modello Y ha un errore alto sia sull'addestramento sia sul test, con un divario molto più piccolo fra i due. Come si chiama questo secondo problema?
> 3. Nessuno dei due modelli è ideale. Descrivi con parole tue cosa dovrebbe mostrare l'errore di un terzo modello, "Modello Z", perché tu possa dire che ha trovato il giusto equilibrio.

---

*Continua con la [Lezione 06, Quanto è bravo, davvero?]({{ site.baseurl }}{% link _ia/come-pensano-le-macchine-che-imparano-lezione-06-quanto-e-bravo-davvero.md %}.html)*

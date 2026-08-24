---
title: 'Lezione 01 — Imparare dagli esempi, non dalle regole'
date: '2026-08-24T00:00:00+02:00'
author: Fabio Mattei
layout: page
---

![Regole scritte a mano contro un mucchio di esempi che, passati per un algoritmo, producono un modello](/images/ia/come-pensano-le-macchine-che-imparano-lezione-01-imparare-dagli-esempi-non-dalle-regole/come-pensano-le-macchine-che-imparano-lezione-01-imparare-dagli-esempi-non-dalle-regole.svg){:class="aside-image"}

### 1.1 L'apprendista del fruttivendolo

Immagina di fare l'apprendista da un fruttivendolo che ti deve insegnare a riconoscere un'anguria matura. Potrebbe provare a dettarti una regola precisa: "se, bussando con le nocche, il suono è cupo e profondo, e se il puntino giallo sulla scorza è bello acceso, allora è matura". Il problema è che una regola così, messa per iscritto, è più difficile da formulare bene di quanto sembri: quanto deve essere "cupo" il suono? Quanto "acceso" il giallo? E se il suono è cupo ma il puntino è pallido?

La strada che il fruttivendolo sceglie davvero, quasi sempre, è un'altra: ti mette davanti dieci angurie, ne apre alcune per farti vedere quali erano davvero mature e quali no, e ti lascia bussare, guardare, confrontare. Dopo qualche decina di angurie osservate — alcune giuste, alcune sbagliate, con lui che ti corregge ogni volta — inizi a "sentire" la differenza, senza che nessuno ti abbia mai scritto la regola esatta su un foglio. Hai imparato dagli esempi, non dalle istruzioni.

### 1.2 Programmazione classica contro machine learning

Questa differenza è precisamente ciò che separa la programmazione tradizionale dal **machine learning** (apprendimento automatico). Nella programmazione classica, il programmatore scrive a mano le regole — "se il totale del carrello supera 50 euro, applica lo sconto" — e il computer si limita a eseguirle alla lettera, sempre uguali, su ogni input che riceve.

Nel machine learning il programmatore non scrive la regola finale. Scrive invece un **algoritmo di apprendimento**: una procedura che, messa di fronte a un mucchio di esempi già risolti, trova da sola una regola che li spiega il più possibile bene. Il risultato di questo processo — la regola trovata — si chiama **modello**. Non è il programmatore a decidere se il puntino deve essere "abbastanza giallo": è il modello, guardando centinaia di angurie passate, a scoprire da solo quanto giallo è "abbastanza".

Questa è la ragione per cui il machine learning è utile proprio nei casi in cui scrivere la regola a mano è difficile o impossibile: riconoscere uno spam, stimare il prezzo giusto di una casa usata, prevedere se un cliente lascerà un abbonamento. Nessuno sa scrivere con precisione la regola "questa è un'email spam"; ma esistono milioni di email già etichettate come "spam" o "non spam", ed è da quelle che un modello può imparare.

### 1.3 Gli ingredienti: caratteristiche ed etichetta

Per far funzionare l'apprendimento del fruttivendolo servono due ingredienti, ed è utile dargli un nome preciso perché torneranno in ogni lezione di questo libro.

Il primo sono le **caratteristiche** (in inglese *feature*): gli indizi che osservi su ogni esempio. Per l'anguria: il tono del suono al tocco (cupo o acuto), l'intensità del colore del puntino giallo, magari il peso. Ogni anguria diventa così una piccola scheda di numeri o categorie — non più "un'anguria", ma "suono: cupo, giallo: intenso, peso: 6 kg".

Il secondo è l'**etichetta** (in inglese *label*): la risposta giusta che vuoi imparare a prevedere. Per l'anguria: "matura" oppure "non matura". Un esempio completo, nel machine learning, è sempre questa coppia: le caratteristiche osservabili subito, più l'etichetta che di solito si scopre solo dopo (aprendo l'anguria) e che nella vita reale, quando il modello dovrà essere usato su un frutto nuovo, non è ancora disponibile — è proprio quella che vogliamo indovinare.

L'insieme di tutti gli esempi già risolti, usato per far imparare il modello, si chiama **insieme di addestramento** (in inglese *training set*). Più è ampio e vario — angurie di stagioni diverse, di provenienze diverse, comprate in negozi diversi — più il modello avrà visto casi sufficienti per generalizzare bene, invece di imparare stranezze legate a un solo fornitore.

<figure style="margin: 2rem 0; text-align: center;">
<svg viewBox="0 0 520 260" xmlns="http://www.w3.org/2000/svg" role="img" aria-labelledby="tab-title tab-desc" style="width: 100%; max-width: 480px; height: auto; font-family: inherit;">
  <title id="tab-title">Una scheda di esempi: caratteristiche più etichetta</title>
  <desc id="tab-desc">Una piccola tabella con quattro angurie, ciascuna con due caratteristiche (suono e colore del puntino) e un'etichetta (matura o non matura).</desc>

  <text x="90" y="24" fill="#828282" font-size="12" text-anchor="middle">suono</text>
  <text x="220" y="24" fill="#828282" font-size="12" text-anchor="middle">puntino giallo</text>
  <text x="380" y="24" fill="#f66a0a" font-size="12" font-weight="bold" text-anchor="middle">etichetta</text>

  <g font-size="12" fill="#111">
    <rect x="20" y="36" width="140" height="40" fill="#fdfdfd" stroke="#e3e3e3" /><text x="90" y="60" text-anchor="middle">cupo</text>
    <rect x="160" y="36" width="140" height="40" fill="#fdfdfd" stroke="#e3e3e3" /><text x="230" y="60" text-anchor="middle">intenso</text>
    <rect x="300" y="36" width="160" height="40" fill="#fde8d6" stroke="#f66a0a" /><text x="380" y="60" text-anchor="middle" font-weight="bold">matura</text>

    <rect x="20" y="76" width="140" height="40" fill="#fdfdfd" stroke="#e3e3e3" /><text x="90" y="100" text-anchor="middle">acuto</text>
    <rect x="160" y="76" width="140" height="40" fill="#fdfdfd" stroke="#e3e3e3" /><text x="230" y="100" text-anchor="middle">pallido</text>
    <rect x="300" y="76" width="160" height="40" fill="#fde8d6" stroke="#f66a0a" /><text x="380" y="100" text-anchor="middle" font-weight="bold">non matura</text>

    <rect x="20" y="116" width="140" height="40" fill="#fdfdfd" stroke="#e3e3e3" /><text x="90" y="140" text-anchor="middle">cupo</text>
    <rect x="160" y="116" width="140" height="40" fill="#fdfdfd" stroke="#e3e3e3" /><text x="230" y="140" text-anchor="middle">pallido</text>
    <rect x="300" y="116" width="160" height="40" fill="#fde8d6" stroke="#f66a0a" /><text x="380" y="140" text-anchor="middle" font-weight="bold">non matura</text>

    <rect x="20" y="156" width="140" height="40" fill="#fdfdfd" stroke="#e3e3e3" /><text x="90" y="180" text-anchor="middle">acuto</text>
    <rect x="160" y="156" width="140" height="40" fill="#fdfdfd" stroke="#e3e3e3" /><text x="230" y="180" text-anchor="middle">intenso</text>
    <rect x="300" y="156" width="160" height="40" fill="#fde8d6" stroke="#f66a0a" /><text x="380" y="180" text-anchor="middle" font-weight="bold">non matura</text>
  </g>

  <text x="260" y="220" fill="#828282" font-size="11" text-anchor="middle">le colonne a sinistra sono le caratteristiche note subito;</text>
  <text x="260" y="236" fill="#828282" font-size="11" text-anchor="middle">l'ultima colonna è ciò che il modello dovrà imparare a indovinare</text>
</svg>
<figcaption style="color: #828282; font-size: 0.9rem; margin-top: 0.5rem;">Ogni riga è un esempio completo: caratteristiche osservabili più l'etichetta da imparare.</figcaption>
</figure>

### 1.4 Due famiglie: imparare con o senza risposte già date

Non tutti gli esempi arrivano con un'etichetta pronta. Questo divide il machine learning in due grandi famiglie, che percorreremo entrambe in questo libro.

Nell'**apprendimento supervisionato** ogni esempio ha già la sua risposta giusta — come le angurie aperte dal fruttivendolo, dove sai già se erano mature. Il compito del modello è imparare a prevedere quella risposta anche su frutti nuovi, mai visti, di cui non conosci ancora l'etichetta. La maggior parte di questo libro — dalla Lezione 2 alla Lezione 6 — riguarda questa famiglia, che a sua volta si divide in due casi: prevedere una categoria (matura o non matura: lo vedremo nelle Lezioni 2 e 3) si chiama **classificazione**; prevedere un numero (il prezzo giusto di una casa: lo vedremo nella Lezione 4) si chiama **regressione**.

Nell'**apprendimento non supervisionato**, invece, gli esempi non hanno alcuna etichetta: hai solo un mucchio di frutti, senza che nessuno ti abbia mai detto quali sono maturi. Quello che un modello può fare in questo caso non è "indovinare una risposta" — non esiste — ma trovare una **struttura nascosta**: per esempio, accorgersi che i frutti si raggruppano naturalmente in due mucchi ben distinti (probabilmente maturi e non maturi, anche se nessuno lo ha scritto da nessuna parte). Ne parleremo nella Lezione 7.

### 1.5 Cosa NON fa il machine learning

Vale la pena chiudere questa prima lezione con un chiarimento che eviterà confusioni più avanti. Un modello di machine learning non "capisce" un'anguria nel modo in cui la capisci tu: non sa cosa sia un frutto, non ha mai visto un'anguria vera, non collega il suono cupo a una nozione fisica di maturazione interna. Vede soltanto numeri — il tono del suono codificato come un valore, l'intensità del giallo come un altro — e trova, tra questi numeri e l'etichetta, delle regolarità statistiche che spesso funzionano sorprendentemente bene, ma che restano, nella sostanza, pattern trovati nei dati, non comprensione nel senso in cui la intendiamo noi.

Questo limite non è un difetto da correggere: è la natura stessa dello strumento, ed è importante tenerlo a mente proprio perché lo strumento funziona così bene da far dimenticare, a volte, che dietro non c'è nessuna vera comprensione — solo un pattern trovato in centinaia o migliaia di esempi passati.

---

> **Prova tu — Impara a riconoscere l'anguria matura**
>
> Guarda questa piccola tabella di angurie già aperte (l'insieme di addestramento):
>
> | Suono | Puntino giallo | Matura? |
> |---|---|---|
> | cupo | intenso | sì |
> | acuto | pallido | no |
> | cupo | pallido | no |
> | acuto | intenso | no |
> | cupo | intenso | sì |
>
> 1. Guardando solo questi cinque esempi, quale sembra essere la regola che separa le angurie mature da quelle non mature? Prova a scriverla con parole tue, come farebbe l'apprendista dopo aver osservato il fruttivendolo.
> 2. Arriva un'anguria nuova, mai vista prima: suono cupo, puntino giallo intenso. Applicando la regola che hai trovato al punto 1, la classificheresti come matura o no?
> 3. Arriva una seconda anguria nuova: suono acuto, puntino giallo intenso. Cosa prevede la tua regola? E quanto ti fidi di questa previsione, guardando quanti esempi nella tabella hanno esattamente questa combinazione?

---

*Continua con la [Lezione 02 — Il vicino più simile]({{ site.baseurl }}{% link _ia/come-pensano-le-macchine-che-imparano-lezione-02-il-vicino-piu-simile.md %}.html)*

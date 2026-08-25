---
title: "Ingegneria sociale: quando l'anello debole è una persona"
date: '2026-08-25T00:00:00+01:00'
author: Fabio Mattei
layout: page
---

Il firewall più aggiornato e la password più robusta non servono a nulla se qualcuno convince la persona giusta ad aprire la porta da sola. L'**ingegneria sociale** (*social engineering*) è l'arte di manipolare le persone per farle compiere azioni o rivelare informazioni che normalmente non condividerebbero — e resta, ancora oggi, il modo più efficace per violare un sistema, perché aggira le difese tecniche colpendo direttamente chi le usa. Il [phishing]({{ site.baseurl }}{% link _sicurezza/phishing.md %}.html) è la tecnica di ingegneria sociale più nota, ma è solo una delle tante.

### Perché funziona: le leve psicologiche

Chi pratica ingegneria sociale sfrutta scorciatoie mentali che usiamo tutti, ogni giorno, per decidere rapidamente come comportarci. Lo psicologo Robert Cialdini le ha catalogate in sei principi, oggi un punto di riferimento anche nella sicurezza informatica:

* **Autorità** — tendiamo a obbedire a chi sembra avere un ruolo superiore (un dirigente, un tecnico IT, un agente di polizia).
* **Urgenza e scarsità** — "solo per oggi", "ultimo avviso": la fretta impedisce di riflettere con calma.
* **Riprova sociale** — se "tutti gli altri" hanno già fatto una cosa, ci sentiamo spinti a farla anche noi.
* **Simpatia** — è più facile fidarsi di chi si mostra gentile, o di chi sembra avere qualcosa in comune con noi.
* **Reciprocità** — se qualcuno ci fa un favore (anche piccolo, anche finto), ci sentiamo in debito e più propensi a ricambiare.
* **Impegno e coerenza** — dopo aver detto "sì" a una piccola richiesta, tendiamo a restare coerenti e accettare richieste successive più grandi.

Nessuna di queste leve richiede competenze tecniche: richiede solo di conoscere il comportamento umano, ed è per questo che l'ingegneria sociale funziona anche contro persone molto attente dal punto di vista informatico.

### Le tecniche principali

* **Pretexting** — costruire un finto scenario credibile (un pretesto) per ottenere informazioni: un attaccante che si finge un collega dell'IT e chiede "solo per verifica" la password di un dipendente.
* **Baiting** — offrire un'esca (*bait*) per attirare la vittima: una chiavetta USB "smarrita" con un logo aziendale, lasciata apposta nel parcheggio, che se collegata a un computer installa un malware.
* **Tailgating / piggybacking** — seguire fisicamente una persona autorizzata attraverso una porta con badge, contando sulla cortesia di chi la tiene aperta, per entrare in un'area riservata senza credenziali proprie.
* **Quid pro quo** — offrire un servizio in cambio di un'informazione: un finto tecnico che telefona offrendo "assistenza gratuita" in cambio dell'accesso remoto al computer.
* **Vishing e smishing** — le varianti telefoniche e via SMS del phishing, già viste nel dettaglio nella pagina dedicata.
* **CEO fraud (o business email compromise)** — un attaccante si finge un dirigente e ordina con urgenza a un dipendente amministrativo un bonifico verso un conto controllato dall'attaccante, spesso sfruttando informazioni reali raccolte online sull'organigramma aziendale.

### Un esempio celebre

Uno dei casi più citati nella storia della sicurezza informatica è quello di **Kevin Mitnick**, che negli anni '80 e '90 ottenne l'accesso a sistemi di grandi aziende americane non tanto sfruttando falle tecniche, quanto telefonando a dipendenti fingendosi un collega o un tecnico, e chiedendo con sicurezza informazioni che nessuno gli avrebbe dovuto dare. Divenne, dopo l'arresto, uno dei consulenti di sicurezza più ricercati proprio per la sua capacità di individuare questo tipo di debolezza.

### Come difendersi

La difesa contro l'ingegneria sociale non è principalmente tecnica, ma culturale:

1. **Verificare sempre l'identità** di chi chiede informazioni sensibili o un'azione insolita, usando un canale diverso da quello con cui è arrivata la richiesta (richiamare un numero noto, non quello lasciato dal chiamante).
2. **Non farsi mettere fretta**: un attaccante conta proprio sul fatto che non ci si fermi a pensare. Una richiesta legittima e urgente resta legittima anche cinque minuti dopo una verifica.
3. **Diffidare di richieste fuori procedura**, soprattutto se riguardano soldi, credenziali o accessi, anche quando sembrano provenire da un superiore.
4. **Non collegare dispositivi sconosciuti** (chiavette USB trovate, caricabatterie non propri) a computer aziendali o personali.
5. **Formazione continua**: nelle aziende, le simulazioni periodiche di phishing e social engineering restano lo strumento più efficace per mantenere alta l'attenzione, molto più di un corso fatto una tantum.

In fondo, l'ingegneria sociale ricorda un principio già visto a proposito della [disinformazione]({{ site.baseurl }}{% link _sicurezza/disinformazione.md %}.html): la tecnologia da sola non basta a proteggerci, se non impariamo a riconoscere quando qualcuno sta cercando di manipolare la nostra fiducia.

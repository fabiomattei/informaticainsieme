---
title: Linux
layout: page
---

Linux non è un prodotto commerciale come Windows o macOS, ma nasce e cresce all'interno del movimento del **software libero**: per capirlo davvero occorre conoscerne la storia e la filosofia.

### Richard Stallman e il progetto GNU

Nel 1983 il programmatore **Richard Stallman**, insoddisfatto della crescente diffusione di software proprietario che impediva agli utenti di studiarne, modificarne e condividerne il codice, avvia il **progetto GNU** (acronimo ricorsivo per "GNU's Not Unix"), con l'obiettivo di creare un sistema operativo completo composto interamente da software libero. Nel 1985 fonda la **Free Software Foundation (FSF)** per sostenere economicamente e legalmente il progetto.

### Le quattro libertà del software libero

Stallman definisce "libero" un software che garantisce all'utente quattro libertà fondamentali:

* **Libertà 0** — eseguire il programma per qualsiasi scopo.
* **Libertà 1** — studiare come funziona il programma e modificarlo (richiede l'accesso al codice sorgente).
* **Libertà 2** — ridistribuire copie del programma.
* **Libertà 3** — distribuire copie delle proprie versioni modificate.

"Libero" qui si riferisce alla libertà (in inglese *free as in freedom*), non necessariamente alla gratuità (*free as in beer*): un software libero può anche essere venduto. Per garantire queste libertà nel tempo, Stallman crea la licenza **GPL** (General Public License), che obbliga chiunque distribuisca versioni modificate di un software GPL a rilasciarle a loro volta come software libero.

### Il kernel Linux

Negli anni '80 e primi anni '90 il progetto GNU aveva già realizzato gran parte di un sistema operativo libero (compilatore, editor, shell...), ma mancava ancora il **kernel**, il cuore del sistema. Nel 1991 lo studente finlandese **Linus Torvalds** rilascia come software libero un kernel scritto da lui, chiamato **Linux**. Combinato con gli strumenti GNU già esistenti, forma un sistema operativo completo: per questo motivo la Free Software Foundation preferisce chiamarlo **GNU/Linux**, anche se nell'uso comune il nome "Linux" indica ormai l'intero sistema operativo.

### Software libero e open source

Nel 1998 nasce un secondo movimento, l'**open source**, promosso tra gli altri da Eric Raymond con la fondazione della **Open Source Initiative (OSI)**. Dal punto di vista pratico, il software open source garantisce quasi sempre le stesse libertà del software libero (codice sorgente accessibile, modificabile, ridistribuibile). La differenza è soprattutto di enfasi: il movimento del software libero insiste sulla libertà dell'utente come valore etico, mentre il movimento open source promuove l'accesso al codice sorgente soprattutto per i suoi vantaggi pratici (qualità, sicurezza, collaborazione).

### Le distribuzioni Linux

Il kernel Linux da solo non basta a formare un sistema operativo utilizzabile: serve una **distribuzione**, cioè un pacchetto che unisce il kernel a librerie di sistema, strumenti GNU, un gestore di pacchetti e spesso un'interfaccia grafica. Esistono decine di distribuzioni diverse, tra le più diffuse:

* **Ubuntu** — orientata alla semplicità d'uso, molto diffusa sia su desktop sia su server.
* **Debian** — una delle distribuzioni più stabili e longeve, alla base di Ubuntu stessa.
* **Fedora** — sponsorizzata da Red Hat, spesso tra le prime a includere le tecnologie più recenti.
* **Arch Linux** — minimale e altamente personalizzabile, pensata per utenti esperti.
* **Linux Mint** — basata su Ubuntu, pensata per chi arriva da Windows e cerca un'interfaccia familiare.
* **openSUSE** — distribuzione europea, nota per lo strumento di amministrazione grafico YaST.

Nonostante le differenze, tutte condividono lo stesso kernel Linux e gli stessi strumenti GNU di base: imparare a usarne una rende relativamente semplice adattarsi alle altre.

### Un antenato comune: Unix

Sia Linux sia macOS derivano concettualmente da **Unix**, un sistema operativo sviluppato nei laboratori Bell a partire dal 1969 da Ken Thompson e Dennis Ritchie. Unix introdusse per primo molte idee che oggi diamo per scontate: il file system ad albero con un'unica radice, la filosofia "tutto è un file", la possibilità di collegare più programmi semplici tramite le pipe, e un linguaggio di sistema portabile (il C, scritto proprio per riscrivere Unix). Linux non contiene codice originale di Unix, ma ne reimplementa da zero le stesse idee ed è compatibile con i suoi standard (POSIX); per questo si dice "Unix-like".

### BSD: un'altra famiglia di sistemi liberi

Parallelamente a GNU/Linux esiste un'altra famiglia di sistemi operativi liberi derivati direttamente dal codice originale di Unix: i sistemi **BSD** (Berkeley Software Distribution), tra cui FreeBSD, OpenBSD e NetBSD. Anche macOS discende in parte da BSD. La differenza principale con Linux non è tanto tecnica quanto di licenza: BSD usa una licenza permissiva (chiunque può prendere il codice, modificarlo e distribuirlo anche in forma proprietaria, senza obbligo di ricondividerlo), mentre GPL è una licenza *copyleft*, che impone di mantenere libere anche le versioni modificate.

### Copyleft: la libertà che si protegge da sola

Il meccanismo del **copyleft**, inventato proprio da Stallman per la GPL, è forse l'idea più originale del movimento del software libero: gioca sul significato di *copyright* per descrivere una licenza che usa il diritto d'autore non per limitare la condivisione, ma per garantirla nel tempo. Un programma GPL può essere modificato e ridistribuito liberamente, ma chi lo ridistribuisce deve farlo mantenendo la stessa licenza GPL e includendo il codice sorgente: in questo modo la libertà iniziale "si trasmette" a ogni versione derivata, senza che nessuno possa privatizzarla lungo la catena.

### Alcuni progetti di software libero diventati fondamentali

L'idea di Stallman non è rimasta confinata a GNU: oggi gran parte dell'infrastruttura di Internet gira su software libero.

* **Apache** e **Nginx** — i server web più diffusi al mondo, entrambi open source.
* **MySQL/MariaDB** e **PostgreSQL** — database liberi usati da un'enorme quantità di applicazioni web.
* **Git** — il sistema di controllo versione creato da Linus Torvalds stesso, oggi standard de facto per la gestione del codice sorgente.
* **Firefox** — browser sviluppato dalla Mozilla Foundation, alternativa libera ai browser proprietari.
* **Python, PHP, Ruby** — molti dei linguaggi di programmazione più usati sono a loro volta software libero.

### Le sottopagine

- [Il terminale]({{ site.baseurl }}{% link _sistemioperativi/linux-terminale.md %}.html) — cos'è un terminale, la shell, la struttura di un comando, pipe, sudo, variabili d'ambiente e alias.
- [Editor di testo da terminale: nano e vim]({{ site.baseurl }}{% link _sistemioperativi/linux-editor.md %}.html) — le scorciatoie di nano e le modalità di vim.
- [Gestione dei file in Linux]({{ site.baseurl }}{% link _sistemioperativi/linux-gestione-file.md %}.html) — il file system unico, navigazione, copia/spostamento/eliminazione, permessi, link simbolici, archivi.
- [Gestione della rete in Linux]({{ site.baseurl }}{% link _sistemioperativi/linux-gestione-rete.md %}.html) — stato delle interfacce, indirizzo IP, connettività, firewall, SSH.
- [Gestione dei pacchetti in Linux]({{ site.baseurl }}{% link _sistemioperativi/linux-pacchetti.md %}.html) — APT, DNF, Pacman, Snap e Flatpak.
- [Gestione dei processi in Linux]({{ site.baseurl }}{% link _sistemioperativi/linux-processi.md %}.html) — ps, top, kill, foreground/background, systemd.
- [Utenti, gruppi e permessi]({{ site.baseurl }}{% link _sistemioperativi/linux-utenti.md %}.html) — root e sudo, creazione utenti e gruppi, permessi in dettaglio.
- [Shell scripting]({{ site.baseurl }}{% link _sistemioperativi/linux-shell-scripting.md %}.html) — variabili, argomenti, condizioni e cicli in un file .sh.
- [Gestione di dischi e mount]({{ site.baseurl }}{% link _sistemioperativi/linux-dischi.md %}.html) — mount/umount, /etc/fstab, spazio libero, UUID.

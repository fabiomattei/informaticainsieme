---
title: 'Installazione dei programmi in Windows'
date: '2026-08-23T00:00:00+01:00'
author: Fabio Mattei
layout: page
---

A differenza di Linux, dove il software si installa quasi sempre tramite un gestore di pacchetti centralizzato, su Windows la tradizione è installare ogni programma separatamente, scaricandolo dal sito del produttore. Negli ultimi anni si sono però affiancate anche soluzioni più centralizzate.

### Installer tradizionali: .exe e .msi

Il metodo storico consiste nello scaricare un file eseguibile (**.exe**) o un pacchetto di installazione Windows (**.msi**) dal sito ufficiale del programma, ed eseguirlo seguendo la procedura guidata di installazione. È il metodo più diffuso ma anche il più rischioso: bisogna assicurarsi di scaricare il file dal sito ufficiale, per evitare installer modificati con malware.

### Microsoft Store

Il **Microsoft Store** è il negozio di app integrato in Windows: le applicazioni scaricate da qui sono verificate da Microsoft, si aggiornano automaticamente e si disinstallano in modo pulito, senza lasciare file residui. Non tutti i programmi sono però disponibili in questa forma.

### winget: il gestore di pacchetti da riga di comando

Windows include ora **winget**, un gestore di pacchetti simile ad APT o Pacman su Linux, utilizzabile da Prompt dei comandi o PowerShell:

{% highlight shell %}
winget search firefox        # cerca un pacchetto
winget install firefox        # installa un pacchetto
winget upgrade --all          # aggiorna tutti i programmi installati tramite winget
winget uninstall firefox      # disinstalla un pacchetto
{% endhighlight %}

A differenza degli installer scaricati manualmente, winget scarica il software da un repository centralizzato e verificato, con il vantaggio di poter aggiornare più programmi con un solo comando.

### Disinstallare un programma

Dalle **Impostazioni → App → App installate** è possibile vedere l'elenco di tutti i programmi installati e disinstallarli. Lo stesso elenco è raggiungibile dal Pannello di controllo classico, alla voce **Programmi e funzionalità**.

### Aggiornamenti di sistema

Gli aggiornamenti del sistema operativo stesso (diversi da quelli dei singoli programmi) sono gestiti separatamente da **Windows Update**, in Impostazioni → Windows Update, che scarica e installa periodicamente patch di sicurezza e nuove funzionalità.

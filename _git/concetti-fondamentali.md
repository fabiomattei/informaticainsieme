---
title: 'I concetti fondamentali di Git'
date: '2026-08-17T09:15:00+02:00'
author: Fabio Mattei
layout: page
---

![Le tre aree di lavoro di Git: working directory, staging area e repository](/images/git/concetti-fondamentali/concetti-fondamentali.svg){:class="aside-image"}

Prima di usare i comandi è importante capire il **vocabolario** di Git: sono pochi concetti, ma vanno chiari fin da subito perché tutto il resto si basa su di essi.

## Repository

Un **repository** (spesso abbreviato "repo") è una cartella di progetto tenuta sotto controllo da Git. Al suo interno, oltre ai file veri e propri, Git mantiene una sottocartella nascosta chiamata `.git`, dove conserva tutta la cronologia delle modifiche, i branch, la configurazione e tutto ciò che gli serve per funzionare.

Un repository può essere:

* **locale**, cioè presente solo sul proprio computer;
* **remoto**, cioè ospitato su un server (ad esempio su GitHub) e condiviso con altre persone.

## Le tre aree di lavoro

Il funzionamento quotidiano di Git ruota attorno a tre "zone" in cui un file può trovarsi:

1. **Working directory** (cartella di lavoro): sono i file così come li vedi e li modifichi normalmente sul disco.
2. **Staging area** (o *index*): è una zona intermedia in cui si "mettono da parte" le modifiche che si vogliono includere nella prossima fotografia della cronologia. Permette di scegliere con precisione cosa salvare, anche se si sono modificati tanti file insieme.
3. **Repository** (cronologia): una volta confermate, le modifiche presenti nella staging area vengono registrate in modo permanente nella cronologia del progetto.

{% highlight text %}
Working directory  --(git add)-->  Staging area  --(git commit)-->  Repository (cronologia)
{% endhighlight %}

Questo flusso in due passaggi (prima `add`, poi `commit`) è una delle caratteristiche distintive di Git rispetto ad altri sistemi di versionamento, che spesso salvano direttamente tutte le modifiche presenti.

## Commit

Un **commit** è una "fotografia" dello stato del progetto in un preciso momento: rappresenta un insieme di modifiche, accompagnato da un messaggio che ne descrive lo scopo, dal nome dell'autore e dalla data.

Ogni commit è collegato al commit precedente, formando una catena che costituisce la cronologia del progetto. Ogni commit ha inoltre un identificativo univoco, uno **hash** (una sequenza di caratteri come `a3f5c9e`), calcolato in base al suo contenuto: due commit con contenuti diversi avranno sempre hash diversi.

## Branch

Un **branch** (ramo) è una linea di sviluppo indipendente. Il branch principale si chiama tradizionalmente `master` o, nelle convenzioni più recenti, `main`.

Creare un branch permette di lavorare su una nuova funzionalità o su una correzione senza toccare il codice principale, che continua a funzionare regolarmente. Quando il lavoro sul branch è completo e testato, lo si può **unire** (*merge*) al branch principale.

{% highlight text %}
main:      A---B---C-------F---G
                    \      /
feature:             D----E
{% endhighlight %}

Nell'esempio, dal branch `main` è stato creato un branch `feature` (dal commit `C`); il lavoro fatto sui commit `D` ed `E` viene poi riunito a `main` con un commit di merge (`F`).

## HEAD

**HEAD** è un puntatore che indica "dove ci si trova" in questo momento: di solito punta all'ultimo commit del branch attualmente attivo. Quando si cambia branch, o si torna a un commit precedente, è HEAD a spostarsi.

## Remote

Un **remote** è un riferimento a un repository che si trova altrove (tipicamente su un server come GitHub). Il nome convenzionale del remote principale è `origin`. È verso il remote che si inviano le proprie modifiche (`push`) e da cui si scaricano quelle degli altri (`pull` o `fetch`).

![Il repository locale collegato a un remote chiamato origin](/images/git/concetti-fondamentali/remote.svg){:class="half-image"}

## Riepilogo visivo

{% highlight text %}
              git add                 git commit               git push
Working dir -----------> Staging area -----------> Repository -----------> Remote (es. GitHub)
   (file)                  (index)                  (locale)                (origin)
{% endhighlight %}

Con questi concetti in mente, nella prossima pagina si parte con i comandi veri e propri.

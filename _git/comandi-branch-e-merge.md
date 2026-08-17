---
title: 'Branch e merge da riga di comando'
date: '2026-08-17T10:00:00+02:00'
author: Fabio Mattei
layout: page
---

![Un branch feature creato da main e poi unito con un merge, con un esempio di conflitto](/images/git/comandi-branch-e-merge/comandi-branch-e-merge.svg){:class="aside-image"}

I branch sono uno degli strumenti più potenti di Git: permettono di sviluppare più cose in parallelo senza interferenze.

## git branch - creare e vedere i branch

Per vedere l'elenco dei branch presenti nel repository (quello attivo è segnato con un asterisco `*`):

{% highlight shell %}
git branch
{% endhighlight %}

Per crearne uno nuovo, a partire dal branch su cui ci si trova in questo momento:

{% highlight shell %}
git branch nuova-funzionalita
{% endhighlight %}

Attenzione: `git branch` da solo **crea** il branch ma non ci si sposta sopra; resta attivo quello di partenza.

## git switch / git checkout - spostarsi tra branch

Per spostarsi su un branch già esistente:

{% highlight shell %}
git switch nuova-funzionalita
{% endhighlight %}

`git switch` è il comando moderno, introdotto per rendere più chiaro il cambio di branch. Nei tutorial più datati si trova ancora spesso il comando `git checkout`, che ha lo stesso effetto ma è meno specifico (serve anche per altre operazioni, come vedremo nella prossima pagina):

{% highlight shell %}
git checkout nuova-funzionalita
{% endhighlight %}

Per creare un branch e spostarcisi sopra in un unico comando:

{% highlight shell %}
git switch -c nuova-funzionalita
{% endhighlight %}

(equivalente a `git checkout -b nuova-funzionalita` con la sintassi più vecchia).

## git merge - unire un branch

Quando il lavoro su un branch è completo, lo si unisce al branch principale. Per prima cosa ci si sposta sul branch di destinazione (tipicamente `main`), poi si esegue il merge del branch da unire:

{% highlight shell %}
git switch main
git merge nuova-funzionalita
{% endhighlight %}

Se nel frattempo `main` non ha ricevuto altri commit, Git esegue un **fast-forward**: sposta semplicemente il puntatore di `main` in avanti, senza creare un commit apposito. Se invece entrambi i branch hanno proseguito in parallelo, Git crea un **commit di merge**, che ha due genitori e riunisce le due storie.

![Confronto tra un merge fast-forward e un merge che crea un commit con due genitori](/images/git/comandi-branch-e-merge/fast-forward.svg){:class="half-image"}

## I conflitti

Un **conflitto** si verifica quando lo stesso punto di uno stesso file è stato modificato in modo diverso su due branch diversi: Git non può decidere da solo quale versione tenere, e chiede di risolverlo a mano.

Quando capita, Git segnala nel file interessato le due versioni in conflitto:

{% highlight text %}
<<<<<<< HEAD
testo presente sul branch corrente
=======
testo presente sul branch che si sta unendo
>>>>>>> nuova-funzionalita
{% endhighlight %}

Per risolvere il conflitto si modifica il file a mano, decidendo quale contenuto tenere (o combinando i due), eliminando i marcatori `<<<<<<<`, `=======` e `>>>>>>>`. Fatto questo, si segna il file come risolto e si completa il merge con un commit:

{% highlight shell %}
git add nomefile.py
git commit -m "Risolto conflitto di merge"
{% endhighlight %}

## git branch -d - eliminare un branch

Una volta unito, un branch non serve più e si può eliminare:

{% highlight shell %}
git branch -d nuova-funzionalita
{% endhighlight %}

Git impedisce l'eliminazione se il branch contiene commit non ancora uniti da nessuna parte, come misura di sicurezza; se si è certi di volerlo eliminare comunque si può forzare con `-D` (maiuscola).

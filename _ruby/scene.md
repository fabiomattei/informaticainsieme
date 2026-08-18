---
title: 'Dragonruby: stato del gioco e scene'
date: '2020-11-16T08:47:01+01:00'
author: Fabio Mattei
layout: page
---

Un videogioco vero non è composto da una sola schermata: c'è tipicamente un menu iniziale,
la schermata di gioco, e una schermata di game over. Ognuna di queste si chiama **scena**
(in inglese **scene**).

Dato che la funzione **tick** viene chiamata sempre allo stesso modo 60 volte al secondo,
dobbiamo essere noi a ricordare in quale scena ci troviamo e a far eseguire codice diverso
in base alla scena corrente. Per farlo basta una variabile nello stato del gioco.

{% highlight ruby %}
def tick args
    args.state.scena ||= :menu

    case args.state.scena
    when :menu
        tick_menu args
    when :gioco
        tick_gioco args
    when :game_over
        tick_game_over args
    end
end
{% endhighlight %}

L'istruzione **case ... when** legge il contenuto della variabile **args.state.scena** ed
esegue soltanto il ramo corrispondente. In questo modo il codice di ciascuna scena resta
separato e più semplice da leggere.

## Il menu

Nel menu mostriamo semplicemente una label che invita a premere un tasto per iniziare, e
aspettiamo che il giocatore lo prema per passare alla scena di gioco.

{% highlight ruby %}
def tick_menu args
    args.outputs.labels << {
        x: 640,
        y: 400,
        text: "Premi SPAZIO per iniziare",
        anchor_x: 0.5,
        anchor_y: 0.5
    }

    if args.inputs.keyboard.key_down.space
        args.state.scena = :gioco
        args.state.punteggio = 0
    end
end
{% endhighlight %}

Da notare che è proprio nel momento del cambio di scena che inizializziamo il punteggio a
0: ogni volta che si ricomincia una partita lo stato del gioco va riportato alle condizioni
iniziali.

## Il gioco

Nella scena di gioco mettiamo tutto quello che abbiamo visto nelle sezioni precedenti:
input, sprites, collisioni. Quando succede l'evento che fa terminare la partita, ad esempio
il giocatore che perde tutte le vite, cambiamo scena.

{% highlight ruby %}
def tick_gioco args
    args.state.vite ||= 3

    # qui va la logica del gioco: movimento, collisioni, punteggio...

    args.outputs.labels << {
        x: 20,
        y: 700,
        text: "Punteggio: #{args.state.punteggio}   Vite: #{args.state.vite}"
    }

    if args.state.vite <= 0
        args.state.scena = :game_over
    end
end
{% endhighlight %}

## Il game over

Infine, nella schermata di game over mostriamo il punteggio finale e permettiamo di
tornare al menu per iniziare una nuova partita.

{% highlight ruby %}
def tick_game_over args
    args.outputs.labels << {
        x: 640,
        y: 400,
        text: "Game Over - punteggio: #{args.state.punteggio}",
        anchor_x: 0.5,
        anchor_y: 0.5
    }

    if args.inputs.keyboard.key_down.space
        args.state.scena = :menu
    end
end
{% endhighlight %}

Questo è anche il punto giusto in cui verificare se il punteggio della partita appena
conclusa è un nuovo record, e in caso affermativo salvarlo su file: vedi la pagina su
[come salvare e caricare i dati]({{ site.baseurl }}{% link _ruby/salvataggio.md %}.html).

## Perché usare i simboli

Le tre scene sono identificate dai simboli **:menu**, **:gioco** e **:game_over** invece
che da semplici stringhe come "menu". In ruby i simboli sono più efficienti da confrontare
tra loro rispetto alle stringhe, e dato che il confronto **args.state.scena == :gioco**
viene fatto potenzialmente 60 volte al secondo, questa scelta ha anche un piccolo beneficio
in termini di prestazioni.

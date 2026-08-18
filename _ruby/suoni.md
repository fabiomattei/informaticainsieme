---
title: 'Dragonruby: i suoni'
date: '2020-11-16T08:47:01+01:00'
author: Fabio Mattei
layout: page
---

Un videogioco senza audio perde molta della sua efficacia: i suoni danno un feedback
immediato al giocatore (una moneta raccolta, un colpo subito) e la musica crea l'atmosfera.
Dragonruby distingue tra effetti sonori brevi, da riprodurre una sola volta, e tracce audio
più lunghe, da mandare in loop.

## Effetti sonori

Per riprodurre un suono una singola volta basta aggiungere il percorso del file alla lista
**args.outputs.sounds**.

{% highlight ruby %}
def tick args
    if args.inputs.keyboard.key_down.space
        args.outputs.sounds << "sounds/coin.wav"
    end
end
{% endhighlight %}

E' importante notare che questa istruzione va eseguita soltanto nel momento in cui accade
l'evento (ad esempio la pressione di un tasto), altrimenti il suono verrebbe fatto partire
60 volte al secondo.

I file audio in formato **wav** devono avere una frequenza di campionamento massima di
44.1 kHz, altrimenti vanno prima convertiti (ad esempio con **ffmpeg**). E' supportato
anche il formato **ogg**.

## Musica e suoni in loop

Per un brano musicale che deve continuare a suonare, oppure per un suono che deve poter
essere fermato, aggiornato o sovrapposto ad altri, si utilizza il dizionario
**args.audio**.

{% highlight ruby %}
def tick args
    args.audio[:musica_di_sottofondo] ||= {
        input: "sounds/bg-music.ogg",
        looping: true
    }
end
{% endhighlight %}

Ogni voce di args.audio è identificata da una chiave a piacere (qui **:musica_di_sottofondo**)
e può contenere queste proprietà:

* input: il percorso del file audio
* gain: il volume, un valore float tra 0.0 e 1.0
* pitch: la velocità di riproduzione, 1.0 è la velocità normale
* looping: valore booleano, se true il suono riparte automaticamente dall'inizio
* paused: valore booleano, se true la riproduzione resta in pausa

Per interrompere definitivamente una traccia basta rimuoverla dal dizionario o assegnarle
il valore nil:

{% highlight ruby %}
def tick args
    if args.inputs.keyboard.key_down.m
        args.audio[:musica_di_sottofondo] = nil
    end
end
{% endhighlight %}

## Volume generale

Il volume di tutto l'audio del gioco si può regolare in un colpo solo attraverso
**args.audio.volume**, un valore float tra 0.0 e 1.0.

{% highlight ruby %}
def tick args
    if args.inputs.keyboard.key_down.minus
        args.audio.volume -= 0.1
    end
    if args.inputs.keyboard.key_down.plus
        args.audio.volume += 0.1
    end
end
{% endhighlight %}

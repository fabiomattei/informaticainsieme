---
title: 'Dragonruby: esempio, Snake'
date: '2020-11-16T08:47:01+01:00'
author: Fabio Mattei
layout: page
---

Gli esempi visti finora rappresentano ogni oggetto con la sua posizione in pixel. **Snake**
introduce un'idea diversa: il gioco si svolge su una **griglia**, cioè lo schermo è diviso
in caselle quadrate della stessa dimensione, e ogni oggetto occupa esattamente una casella.
Il serpente stesso è rappresentato come un **array di caselle**, una entry per ogni
segmento del suo corpo: la prima entry è la testa, tutte le altre sono la coda.

## La griglia

Scegliamo una dimensione per ogni casella, ad esempio 40 pixel, e calcoliamo quante caselle
ci stanno nello schermo largo 1280 e alto 720 pixel visto nell'[introduzione]({{ site.baseurl }}{% link _ruby/sprites.md %}.html).

{% highlight ruby %}
CASELLA = 40
COLONNE = 1280 / CASELLA  # 32
RIGHE = 720 / CASELLA     # 18
{% endhighlight %}

## Il corpo del serpente

Il corpo è un array di dizionari, ciascuno con le coordinate x e y espresse **in caselle**,
non in pixel: il segmento con coordinate x: 2, y: 5 occupa la terza casella da sinistra e la
sesta da sotto, indipendentemente da quanto grande sia una casella.

{% highlight ruby %}
def tick args
    args.state.corpo ||= [{ x: 16, y: 9 }, { x: 15, y: 9 }, { x: 14, y: 9 }]
    args.state.direzione ||= { x: 1, y: 0 }

    args.outputs.solids << args.state.corpo.map do |segmento|
        {
            x: segmento.x * CASELLA,
            y: segmento.y * CASELLA,
            w: CASELLA - 2,
            h: CASELLA - 2,
            r: 0, g: 200, b: 0
        }
    end
end
{% endhighlight %}

Solo al momento di disegnare moltiplichiamo le coordinate per la dimensione della casella,
per ottenere le coordinate in pixel richieste da **args.outputs.solids**.

## Cambiare direzione

La direzione è anch'essa una coppia x, y, ma con valori limitati a -1, 0 oppure 1: indica di
quante caselle si sposta la testa del serpente ad ogni passo. Impediamo di invertire
direttamente la direzione (ad esempio andare a sinistra mentre ci si muove verso destra),
altrimenti la testa andrebbe a sbattere immediatamente contro il primo segmento del corpo.

{% highlight ruby %}
def tick args
    direzione = args.state.direzione

    if args.inputs.keyboard.key_down.up && direzione.y == 0
        args.state.direzione = { x: 0, y: 1 }
    elsif args.inputs.keyboard.key_down.down && direzione.y == 0
        args.state.direzione = { x: 0, y: -1 }
    elsif args.inputs.keyboard.key_down.left && direzione.x == 0
        args.state.direzione = { x: -1, y: 0 }
    elsif args.inputs.keyboard.key_down.right && direzione.x == 0
        args.state.direzione = { x: 1, y: 0 }
    end
end
{% endhighlight %}

Il controllo **direzione.y == 0** quando si preme su o giù (e **direzione.x == 0** quando si
preme sinistra o destra) accetta il cambio di direzione solo se ci si stava muovendo lungo
l'asse perpendicolare: è il modo più semplice per impedire l'inversione a 180 gradi.

## Muoversi sulla griglia

Se il serpente si spostasse di una casella ad ogni tick, cioè 60 volte al secondo, sarebbe
troppo veloce per essere giocabile. Facciamo quindi avanzare il serpente soltanto una volta
ogni 8 tick, con la stessa tecnica del resto della divisione già vista nella pagina sullo
[spawn dei nemici]({{ site.baseurl }}{% link _ruby/spawn.md %}.html).

Muoversi significa aggiungere una nuova testa nella direzione corrente, con **unshift**
(che inserisce un elemento in cima all'array), e togliere l'ultimo segmento della coda con
**pop**, a meno che il serpente non debba crescere.

{% highlight ruby %}
def tick args
    if Kernel.tick_count % 8 == 0
        testa = args.state.corpo.first
        direzione = args.state.direzione

        nuova_testa = { x: testa.x + direzione.x, y: testa.y + direzione.y }
        args.state.corpo.unshift(nuova_testa)

        if args.state.crescere
            args.state.crescere = false
        else
            args.state.corpo.pop
        end
    end
end
{% endhighlight %}

## Il cibo

Il cibo è una singola casella, posizionata a caso, che va evitata di posizionare sopra il
corpo del serpente stesso.

{% highlight ruby %}
def posizione_libera_a_caso corpo
    loop do
        posizione = { x: rand(COLONNE), y: rand(RIGHE) }
        return posizione unless corpo.any? { |segmento| segmento.x == posizione.x && segmento.y == posizione.y }
    end
end

def tick args
    args.state.cibo ||= posizione_libera_a_caso(args.state.corpo)

    args.outputs.solids << {
        x: args.state.cibo.x * CASELLA,
        y: args.state.cibo.y * CASELLA,
        w: CASELLA - 2,
        h: CASELLA - 2,
        r: 220, g: 40, b: 40
    }
end
{% endhighlight %}

Quando la testa raggiunge il cibo, il serpente deve crescere e il cibo deve riapparire in
un'altra posizione casuale.

{% highlight ruby %}
testa = args.state.corpo.first
if testa.x == args.state.cibo.x && testa.y == args.state.cibo.y
    args.state.crescere = true
    args.state.punteggio += 1
    args.state.cibo = posizione_libera_a_caso(args.state.corpo)
    args.outputs.sounds << "sounds/mangia.wav"
end
{% endhighlight %}

## Game over

Il serpente muore se la testa esce dalla griglia, oppure se la testa collide con uno degli
altri segmenti del proprio corpo: usiamo **any?**, già incontrato poco sopra per il cibo,
per controllare tutti i segmenti tranne la testa stessa.

{% highlight ruby %}
testa = args.state.corpo.first
corpo_senza_testa = args.state.corpo[1..-1]

fuori_dalla_griglia = testa.x < 0 || testa.x >= COLONNE || testa.y < 0 || testa.y >= RIGHE
contro_se_stesso = corpo_senza_testa.any? { |segmento| segmento.x == testa.x && segmento.y == testa.y }

args.state.finita = fuori_dalla_griglia || contro_se_stesso
{% endhighlight %}

## Il gioco completo

{% highlight ruby %}
CASELLA = 40
COLONNE = 1280 / CASELLA
RIGHE = 720 / CASELLA

def posizione_libera_a_caso corpo
    loop do
        posizione = { x: rand(COLONNE), y: rand(RIGHE) }
        return posizione unless corpo.any? { |segmento| segmento.x == posizione.x && segmento.y == posizione.y }
    end
end

def tick args
    args.state.corpo ||= [{ x: 16, y: 9 }, { x: 15, y: 9 }, { x: 14, y: 9 }]
    args.state.direzione ||= { x: 1, y: 0 }
    args.state.cibo ||= posizione_libera_a_caso(args.state.corpo)
    args.state.punteggio ||= 0
    args.state.crescere ||= false
    args.state.finita ||= false

    if args.state.finita
        args.outputs.labels << { x: 640, y: 400, text: "Game Over", anchor_x: 0.5 }
        return
    end

    direzione = args.state.direzione

    # cambio di direzione
    if args.inputs.keyboard.key_down.up && direzione.y == 0
        args.state.direzione = { x: 0, y: 1 }
    elsif args.inputs.keyboard.key_down.down && direzione.y == 0
        args.state.direzione = { x: 0, y: -1 }
    elsif args.inputs.keyboard.key_down.left && direzione.x == 0
        args.state.direzione = { x: -1, y: 0 }
    elsif args.inputs.keyboard.key_down.right && direzione.x == 0
        args.state.direzione = { x: 1, y: 0 }
    end

    # movimento, una casella ogni 8 tick
    if Kernel.tick_count % 8 == 0
        testa = args.state.corpo.first
        direzione = args.state.direzione

        nuova_testa = { x: testa.x + direzione.x, y: testa.y + direzione.y }
        args.state.corpo.unshift(nuova_testa)

        if args.state.crescere
            args.state.crescere = false
        else
            args.state.corpo.pop
        end

        # cibo raggiunto
        if nuova_testa.x == args.state.cibo.x && nuova_testa.y == args.state.cibo.y
            args.state.crescere = true
            args.state.punteggio += 1
            args.state.cibo = posizione_libera_a_caso(args.state.corpo)
            args.outputs.sounds << "sounds/mangia.wav"
        end

        # game over
        corpo_senza_testa = args.state.corpo[1..-1]
        fuori_dalla_griglia = nuova_testa.x < 0 || nuova_testa.x >= COLONNE ||
                              nuova_testa.y < 0 || nuova_testa.y >= RIGHE
        contro_se_stesso = corpo_senza_testa.any? { |s| s.x == nuova_testa.x && s.y == nuova_testa.y }
        args.state.finita = fuori_dalla_griglia || contro_se_stesso
    end

    # disegno
    args.outputs.solids << args.state.corpo.map do |segmento|
        { x: segmento.x * CASELLA, y: segmento.y * CASELLA, w: CASELLA - 2, h: CASELLA - 2, r: 0, g: 200, b: 0 }
    end
    args.outputs.solids << {
        x: args.state.cibo.x * CASELLA, y: args.state.cibo.y * CASELLA,
        w: CASELLA - 2, h: CASELLA - 2, r: 220, g: 40, b: 40
    }
    args.outputs.labels << { x: 20, y: 700, text: "Punteggio: #{args.state.punteggio}" }
end
{% endhighlight %}

## Come continuare

* far aumentare la velocità del serpente (diminuendo il numero 8 usato nel modulo) ogni
  volta che mangia un frutto, per rendere il gioco via via più difficile
* salvare il [punteggio più alto]({{ site.baseurl }}{% link _ruby/salvataggio.md %}.html)
  ottenuto tra una partita e l'altra
* trasformare il messaggio di game over in una vera schermata da cui ricominciare, come
  visto nella pagina sullo [stato del gioco e le scene]({{ site.baseurl }}{% link _ruby/scene.md %}.html)
* disegnare il serpente con degli sprite invece di semplici solidi, come visto nella pagina
  sugli [sprites]({{ site.baseurl }}{% link _ruby/dragonsprites.md %}.html)

---
title: 'Dragonruby: esempio, uno space shooter'
date: '2020-11-16T08:47:01+01:00'
author: Fabio Mattei
layout: page
---

Come esempio conclusivo di questa sezione mettiamo insieme quasi tutte le tecniche viste
nelle pagine precedenti per costruire un piccolo **space shooter**: un'astronave che si
muove in basso sullo schermo, spara verso l'alto, e deve distruggere i nemici che scendono
dall'alto prima che la raggiungano.

## L'astronave del giocatore

Il giocatore si muove solo a sinistra e a destra, restando sempre alla stessa altezza,
esattamente come visto nella pagina sull'[input]({{ site.baseurl }}{% link _ruby/input.md %}.html).

{% highlight ruby %}
def tick args
    args.state.giocatore ||= { x: 610, y: 60, w: 60, h: 60 }

    if args.inputs.left
        args.state.giocatore.x -= 6
    elsif args.inputs.right
        args.state.giocatore.x += 6
    end

    args.outputs.sprites << args.state.giocatore.merge(path: 'sprites/giocatore.png')
end
{% endhighlight %}

## Sparare

Ogni volta che viene premuta la barra spaziatrice creiamo un nuovo proiettile nello stato
del gioco, esattamente sopra l'astronave, che si sposterà verso l'alto ad ogni tick. Usiamo
**key_down** e non **key_held**, altrimenti terrebbe la barra spaziatrice premuta
produrrebbe decine di proiettili in un solo secondo.

{% highlight ruby %}
def tick args
    args.state.proiettili ||= []

    if args.inputs.keyboard.key_down.space
        args.state.proiettili << {
            x: args.state.giocatore.x + args.state.giocatore.w / 2 - 4,
            y: args.state.giocatore.y + args.state.giocatore.h,
            w: 8,
            h: 20
        }
        args.outputs.sounds << "sounds/spara.wav"
    end

    args.state.proiettili.each { |proiettile| proiettile.y += 10 }
    args.state.proiettili.reject! { |proiettile| proiettile.y > 720 }

    args.outputs.solids << args.state.proiettili.map { |p| p.merge(r: 255, g: 255, b: 0) }
end
{% endhighlight %}

Come già visto nella pagina sui [nemici che appaiono nel tempo]({{ site.baseurl }}{% link _ruby/spawn.md %}.html),
è fondamentale togliere dall'array i proiettili usciti dallo schermo con **reject!**,
altrimenti l'array crescerebbe all'infinito.

## I nemici

I nemici compaiono ad intervalli regolari in alto, ad una posizione orizzontale casuale, e
scendono verso il basso, con la stessa tecnica vista nella pagina sullo
[spawn dei nemici]({{ site.baseurl }}{% link _ruby/spawn.md %}.html).

{% highlight ruby %}
def tick args
    args.state.nemici ||= []

    if Kernel.tick_count % 60 == 0
        args.state.nemici << {
            x: rand(1200),
            y: 700,
            w: 50,
            h: 50
        }
    end

    args.state.nemici.each { |nemico| nemico.y -= 2 }

    args.outputs.sprites << args.state.nemici.map { |n| n.merge(path: 'sprites/nemico.png') }
end
{% endhighlight %}

## Le collisioni tra proiettili e nemici

Per ogni proiettile controlliamo se ha colpito uno o più nemici usando
**find_all_intersect_rect**, visto nella pagina sulla [geometria]({{ site.baseurl }}{% link _ruby/geometria.md %}.html):
in questo modo un solo proiettile potente potrebbe anche colpire più nemici
contemporaneamente se sono molto vicini tra loro.

{% highlight ruby %}
def tick args
    args.state.punteggio ||= 0

    args.state.proiettili.each do |proiettile|
        nemici_colpiti = Geometry.find_all_intersect_rect(proiettile, args.state.nemici)

        next if nemici_colpiti.empty?

        proiettile.morto = true
        nemici_colpiti.each { |nemico| nemico.morto = true }
        args.state.punteggio += nemici_colpiti.length * 10
        args.outputs.sounds << "sounds/esplosione.wav"
    end

    args.state.proiettili.reject! { |proiettile| proiettile.morto }
    args.state.nemici.reject! { |nemico| nemico.morto }
end
{% endhighlight %}

Questo è lo stesso pattern "marca e poi rimuovi" visto nella pagina sulle
[collisioni]({{ site.baseurl }}{% link _ruby/collisioni.md %}.html): non si può eliminare un
elemento da un array mentre lo si sta ancora scorrendo con each, quindi prima si marcano gli
elementi da eliminare, e solo dopo si rimuovono tutti insieme con reject!.

## Un'esplosione quando un nemico viene distrutto

Aggiungiamo qualche particella, come visto nella pagina sulle
[particelle]({{ site.baseurl }}{% link _ruby/particelle.md %}.html), nel punto esatto in cui
un nemico viene distrutto, per dare più soddisfazione al colpo.

{% highlight ruby %}
def crea_particella x, y
    { x: x, y: y, dx: (rand - 0.5) * 6, dy: rand * 4, vita: 20 }
end

def tick args
    args.state.particelle ||= []

    args.state.nemici.each do |nemico|
        next unless nemico.morto

        8.times do
            args.state.particelle << crea_particella(nemico.x + nemico.w / 2, nemico.y + nemico.h / 2)
        end
    end

    args.state.particelle.each do |particella|
        particella.x += particella.dx
        particella.y += particella.dy
        particella.vita -= 1
    end
    args.state.particelle.reject! { |particella| particella.vita <= 0 }

    args.outputs.solids << args.state.particelle.map do |particella|
        { x: particella.x, y: particella.y, w: 4, h: 4, r: 255, g: 128, b: 0, a: particella.vita * 12 }
    end
end
{% endhighlight %}

## Il gioco completo

Ecco tutti i pezzi uniti in un'unica funzione tick:

{% highlight ruby %}
def crea_particella x, y
    { x: x, y: y, dx: (rand - 0.5) * 6, dy: rand * 4, vita: 20 }
end

def tick args
    args.state.giocatore ||= { x: 610, y: 60, w: 60, h: 60 }
    args.state.proiettili ||= []
    args.state.nemici ||= []
    args.state.particelle ||= []
    args.state.punteggio ||= 0

    # movimento del giocatore
    if args.inputs.left
        args.state.giocatore.x -= 6
    elsif args.inputs.right
        args.state.giocatore.x += 6
    end

    # sparare
    if args.inputs.keyboard.key_down.space
        args.state.proiettili << {
            x: args.state.giocatore.x + args.state.giocatore.w / 2 - 4,
            y: args.state.giocatore.y + args.state.giocatore.h,
            w: 8,
            h: 20
        }
        args.outputs.sounds << "sounds/spara.wav"
    end
    args.state.proiettili.each { |proiettile| proiettile.y += 10 }
    args.state.proiettili.reject! { |proiettile| proiettile.y > 720 }

    # far apparire i nemici
    if Kernel.tick_count % 60 == 0
        args.state.nemici << { x: rand(1200), y: 700, w: 50, h: 50 }
    end
    args.state.nemici.each { |nemico| nemico.y -= 2 }

    # collisioni tra proiettili e nemici
    args.state.proiettili.each do |proiettile|
        nemici_colpiti = Geometry.find_all_intersect_rect(proiettile, args.state.nemici)
        next if nemici_colpiti.empty?

        proiettile.morto = true
        nemici_colpiti.each { |nemico| nemico.morto = true }
        args.state.punteggio += nemici_colpiti.length * 10
        args.outputs.sounds << "sounds/esplosione.wav"

        nemici_colpiti.each do |nemico|
            8.times { args.state.particelle << crea_particella(nemico.x + nemico.w / 2, nemico.y + nemico.h / 2) }
        end
    end
    args.state.proiettili.reject! { |proiettile| proiettile.morto }
    args.state.nemici.reject! { |nemico| nemico.morto }

    # particelle delle esplosioni
    args.state.particelle.each do |particella|
        particella.x += particella.dx
        particella.y += particella.dy
        particella.vita -= 1
    end
    args.state.particelle.reject! { |particella| particella.vita <= 0 }

    # disegno
    args.outputs.sprites << args.state.giocatore.merge(path: 'sprites/giocatore.png')
    args.outputs.sprites << args.state.nemici.map { |n| n.merge(path: 'sprites/nemico.png') }
    args.outputs.solids << args.state.proiettili.map { |p| p.merge(r: 255, g: 255, b: 0) }
    args.outputs.solids << args.state.particelle.map do |particella|
        { x: particella.x, y: particella.y, w: 4, h: 4, r: 255, g: 128, b: 0, a: particella.vita * 12 }
    end
    args.outputs.labels << { x: 20, y: 700, text: "Punteggio: #{args.state.punteggio}" }
end
{% endhighlight %}

## Come continuare

Questo esempio riassume gran parte della sezione, ma si può ancora migliorare molto,
riprendendo le pagine già viste:

* far terminare la partita, con una vera schermata di [game over]({{ site.baseurl }}{% link _ruby/scene.md %}.html),
  quando un nemico raggiunge l'altezza del giocatore
* salvare il [punteggio più alto]({{ site.baseurl }}{% link _ruby/salvataggio.md %}.html) ottenuto tra una partita e l'altra
* far diminuire l'intervallo tra un nemico e l'altro nel tempo, come visto nella pagina sullo
  [spawn dei nemici]({{ site.baseurl }}{% link _ruby/spawn.md %}.html), per aumentare la difficoltà
* aggiungere un pulsante "Gioca" cliccabile nel menu iniziale, come visto nella pagina sui
  [pulsanti cliccabili]({{ site.baseurl }}{% link _ruby/pulsanti.md %}.html)

---
title: 'Dragonruby: esempio, il tris'
date: '2020-11-16T08:47:01+01:00'
author: Fabio Mattei
layout: page
---

Tutti gli esempi visti finora sono giochi in tempo reale, dove qualcosa si muove ad ogni
tick anche se il giocatore non fa nulla. Il **tris** (conosciuto anche come
**tic-tac-toe**) è un gioco completamente diverso: **a turni**, dove non succede nulla
finché uno dei due giocatori non clicca su una casella, esattamente come i
[pulsanti cliccabili]({{ site.baseurl }}{% link _ruby/pulsanti.md %}.html) già visti.

## Il tabellone

Il tabellone è una griglia 3x3, la stessa idea di griglia vista in
[Snake]({{ site.baseurl }}{% link _ruby/snake.md %}.html), ma qui rappresentiamo le nove
celle con un semplice array di 9 elementi invece che con le coordinate di un array bidimensionale:
la cella alla riga **r** e colonna **c** si trova all'indice **r * 3 + c**.

{% highlight ruby %}
DIMENSIONE_CELLA = 200
ORIGINE_X = 340
ORIGINE_Y = 60

def tick args
    args.state.celle ||= Array.new(9)

    # le linee della griglia
    (1..2).each do |i|
        args.outputs.lines << { x: ORIGINE_X + i * DIMENSIONE_CELLA, y: ORIGINE_Y, x2: ORIGINE_X + i * DIMENSIONE_CELLA, y2: ORIGINE_Y + 600 }
        args.outputs.lines << { x: ORIGINE_X, y: ORIGINE_Y + i * DIMENSIONE_CELLA, x2: ORIGINE_X + 600, y2: ORIGINE_Y + i * DIMENSIONE_CELLA }
    end
end
{% endhighlight %}

**Array.new(9)** crea un array di 9 elementi, tutti inizialmente **nil**, cioè nessuna
cella è stata ancora giocata. Le linee, come viste nella pagina sulle
[forme e primitive grafiche]({{ site.baseurl }}{% link _ruby/forme.md %}.html), disegnano
soltanto le due righe e le due colonne interne, quelle che dividono le nove celle.

## Capire su quale cella si è cliccato

Per ogni cella controlliamo, con **inside_rect?** già visto nella pagina
sull'[input]({{ site.baseurl }}{% link _ruby/input.md %}.html), se il click del mouse è
avvenuto al suo interno.

{% highlight ruby %}
def tick args
    if args.inputs.mouse.key_down.left
        (0..2).each do |riga|
            (0..2).each do |colonna|
                cella_x = ORIGINE_X + colonna * DIMENSIONE_CELLA
                cella_y = ORIGINE_Y + riga * DIMENSIONE_CELLA

                if args.inputs.mouse.inside_rect?(cella_x, cella_y, DIMENSIONE_CELLA, DIMENSIONE_CELLA)
                    indice = riga * 3 + colonna
                    # qui andremo a registrare la mossa
                end
            end
        end
    end
end
{% endhighlight %}

## Registrare una mossa e cambiare turno

Una mossa è valida solo se la cella cliccata è ancora libera (**nil**). Dopo una mossa
valida tocca all'altro giocatore.

{% highlight ruby %}
def tick args
    args.state.turno ||= :x

    # ... dentro al controllo del click visto sopra:
    if args.state.celle[indice].nil?
        args.state.celle[indice] = args.state.turno
        args.state.turno = (args.state.turno == :x ? :o : :x)
    end
end
{% endhighlight %}

## Disegnare le X e le O

Per ogni cella non vuota disegniamo una label con la scritta X oppure O, centrata al centro
della cella grazie ad **anchor_x** e **anchor_y**, già visti nella pagina sulle
[labels]({{ site.baseurl }}{% link _ruby/labels.md %}.html).

{% highlight ruby %}
def tick args
    (0..2).each do |riga|
        (0..2).each do |colonna|
            indice = riga * 3 + colonna
            simbolo = args.state.celle[indice]
            next unless simbolo

            args.outputs.labels << {
                x: ORIGINE_X + colonna * DIMENSIONE_CELLA + DIMENSIONE_CELLA / 2,
                y: ORIGINE_Y + riga * DIMENSIONE_CELLA + DIMENSIONE_CELLA / 2,
                text: simbolo.to_s.upcase,
                anchor_x: 0.5,
                anchor_y: 0.5,
                size_px: 80
            }
        end
    end
end
{% endhighlight %}

## Controllare se qualcuno ha vinto

Ci sono otto modi per vincere: tre righe, tre colonne e due diagonali. Ognuno di questi si
rappresenta con i tre indici dell'array che devono contenere lo stesso simbolo.

{% highlight ruby %}
COMBINAZIONI_VINCENTI = [
    [0, 1, 2], [3, 4, 5], [6, 7, 8], # righe
    [0, 3, 6], [1, 4, 7], [2, 5, 8], # colonne
    [0, 4, 8], [2, 4, 6]             # diagonali
]

def trova_vincitore celle
    COMBINAZIONI_VINCENTI.each do |a, b, c|
        if celle[a] && celle[a] == celle[b] && celle[b] == celle[c]
            return celle[a]
        end
    end
    nil
end
{% endhighlight %}

**COMBINAZIONI_VINCENTI.each do |a, b, c|** scompone automaticamente ogni array di tre
indici nelle tre variabili a, b e c: è la stessa idea della destrutturazione già usata altrove
per estrarre più valori in un colpo solo da un array.

## Pareggio e come ricominciare

Se nessuno ha vinto e tutte le celle sono piene, la partita finisce in pareggio. In ogni
caso, quando la partita è finita, un click permette di ricominciare da capo.

{% highlight ruby %}
def tick args
    args.state.vincitore = trova_vincitore(args.state.celle)
    finita = args.state.vincitore || args.state.celle.all? { |cella| !cella.nil? }

    if finita
        testo = args.state.vincitore ? "Ha vinto #{args.state.vincitore.to_s.upcase}!" : "Pareggio!"
        args.outputs.labels << { x: 640, y: 660, text: "#{testo} Clicca per ricominciare", anchor_x: 0.5 }

        if args.inputs.mouse.key_down.left
            args.state.celle = Array.new(9)
            args.state.turno = :x
            args.state.vincitore = nil
        end
    end
end
{% endhighlight %}

## Il gioco completo

{% highlight ruby %}
DIMENSIONE_CELLA = 200
ORIGINE_X = 340
ORIGINE_Y = 60

COMBINAZIONI_VINCENTI = [
    [0, 1, 2], [3, 4, 5], [6, 7, 8],
    [0, 3, 6], [1, 4, 7], [2, 5, 8],
    [0, 4, 8], [2, 4, 6]
]

def trova_vincitore celle
    COMBINAZIONI_VINCENTI.each do |a, b, c|
        return celle[a] if celle[a] && celle[a] == celle[b] && celle[b] == celle[c]
    end
    nil
end

def tick args
    args.state.celle ||= Array.new(9)
    args.state.turno ||= :x
    args.state.vincitore = trova_vincitore(args.state.celle)
    finita = args.state.vincitore || args.state.celle.all? { |cella| !cella.nil? }

    if args.inputs.mouse.key_down.left
        if finita
            args.state.celle = Array.new(9)
            args.state.turno = :x
        else
            (0..2).each do |riga|
                (0..2).each do |colonna|
                    cella_x = ORIGINE_X + colonna * DIMENSIONE_CELLA
                    cella_y = ORIGINE_Y + riga * DIMENSIONE_CELLA
                    indice = riga * 3 + colonna

                    if args.inputs.mouse.inside_rect?(cella_x, cella_y, DIMENSIONE_CELLA, DIMENSIONE_CELLA) &&
                       args.state.celle[indice].nil?
                        args.state.celle[indice] = args.state.turno
                        args.state.turno = (args.state.turno == :x ? :o : :x)
                    end
                end
            end
        end
    end

    # griglia
    (1..2).each do |i|
        args.outputs.lines << { x: ORIGINE_X + i * DIMENSIONE_CELLA, y: ORIGINE_Y, x2: ORIGINE_X + i * DIMENSIONE_CELLA, y2: ORIGINE_Y + 600 }
        args.outputs.lines << { x: ORIGINE_X, y: ORIGINE_Y + i * DIMENSIONE_CELLA, x2: ORIGINE_X + 600, y2: ORIGINE_Y + i * DIMENSIONE_CELLA }
    end

    # simboli
    (0..2).each do |riga|
        (0..2).each do |colonna|
            simbolo = args.state.celle[riga * 3 + colonna]
            next unless simbolo

            args.outputs.labels << {
                x: ORIGINE_X + colonna * DIMENSIONE_CELLA + DIMENSIONE_CELLA / 2,
                y: ORIGINE_Y + riga * DIMENSIONE_CELLA + DIMENSIONE_CELLA / 2,
                text: simbolo.to_s.upcase,
                anchor_x: 0.5,
                anchor_y: 0.5,
                size_px: 80
            }
        end
    end

    # messaggio di fine partita
    if finita
        testo = args.state.vincitore ? "Ha vinto #{args.state.vincitore.to_s.upcase}!" : "Pareggio!"
        args.outputs.labels << { x: 640, y: 660, text: "#{testo} Clicca per ricominciare", anchor_x: 0.5 }
    else
        args.outputs.labels << { x: 640, y: 660, text: "Turno di #{args.state.turno.to_s.upcase}", anchor_x: 0.5 }
    end
end
{% endhighlight %}

## Come continuare

* evidenziare con un colore diverso la casella su cui si trova il mouse, come visto per i
  [pulsanti cliccabili]({{ site.baseurl }}{% link _ruby/pulsanti.md %}.html)
* far scegliere il nome dei due giocatori con la tecnica vista nella pagina su
  [come raccogliere il testo digitato]({{ site.baseurl }}{% link _ruby/testo.md %}.html)
* tenere il conteggio delle vittorie di ciascun giocatore, salvandolo su file come visto
  nella pagina su [come salvare e caricare i dati]({{ site.baseurl }}{% link _ruby/salvataggio.md %}.html)
* trasformare il messaggio di inizio e fine partita in vere schermate, come visto nella
  pagina sullo [stato del gioco e le scene]({{ site.baseurl }}{% link _ruby/scene.md %}.html)

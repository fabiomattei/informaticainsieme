---
title: 'Dragonruby'
date: '2020-11-15T08:47:01+01:00'
author: Fabio Mattei
layout: page
---

## Area di gioco

![Area di gioco](/images/ruby/dragonruby/schermo.png)

La cosa più importante in un videogioco è la gestione della grafica, dobbiamo 
essere in grado di poter decidere cosa disegnare e dove.
A questo proposito abbiamo bisogno di definire un **sistema di riferimento**.
L'area in cui si svolge il nostro gioco è un piano cartesiano che ha larghezza di
1280 pixel e altezza di 720 pixel. L'origine è nell'angolo in basso a destra 
dello schermo.

## Tick

La funzione tick è il cuore di ogni videogioco.
Questa funzione viene chiamata 60 volte al secondo e precede il disegnarsi di ciascun  
frame sullo schermo per questo si guarda al suo contenuto definendolo **game loop**. 
Il game loop si divide in quattro fasi: lettura dell'input, aggiornamento dello stato 
del gioco, aggiornamento dei suoni del gioco, aggiornamento della grafica.

![Il loop](/images/ruby/dragonruby/loop.png)

Questo piccolo loop la frase "Ciao 4C!" nelle coordinate 
x=120, y=120 del piano cartesiano che abbiamo indicato in precedenza.

La scrive 60 volte al secondo, sempre uguale, sempre nello stesso punto. 
Questo dà all'utente l'idea di vedere una scritta immobile.

{% highlight ruby %}
def tick args
    args.outputs.labels << { x:120, y:120, text: "Ciao 4C!"}
end
{% endhighlight %}

La definizione della funzione tick è contiene un parametro args.
Questo parametro è una struttura dati basata su dizionari e consente di
leggere l'input, tenere memoria dello stato e scrivere l'output.
E' una struttura dati molto importante!

L'esempio seguente disegna un drago in una posizione fissa dello schermo.
Anche in questo caso l'immagine sebbene sembri fissa all'utente, viene aggiornata
60 volte al secondo.

{% highlight ruby %}
def tick args
    args.state.player_x ||= 120
    args.state.player_y ||= 280
    args.outputs.sprites << {
		x:args.state.player_x, 
		y:args.state.player_y, 
		w:100, 
		h:80, 
		path:'sprites/dragon-0.png' 
	}
end
{% endhighlight %}

## Lo stato del gioco

Il dizionario args.state permette di memorizzare informazioni allo sviluppatore
che vengono mantenute in memoria tra una esecuzione di tick e la successiva.
Questo viene normalmente definito **lo stato del gioco**.
E' dunque una **variabile globale** che viene mantenuta sempre in RAM fino 
allo spegnimento del gioco.

{% highlight ruby %}
def tick args
    args.state.x_giocatore ||= 120
    args.state.y_giocatore ||= 280
end
{% endhighlight %}

### Operatore ||=

Questo operatore che si chiama **or equal** è un operatore di assegnamento, ma 
a differenza dell'operatore **=** che assegna alla variabile sinistra il valore
contenuto a destra in qualsiasi caso, l'operatore **or equal** assegna il valore
soltanto se in precedenza quella variabile non è stata mai assegnata.

## Input: facciamo muovere il drago

L'esempio seguente disegna un drago in coordinate che l'utente può aggiornare.
Questo significa raccogliere l'input che l'utente immette attraverso la pressione
dei tasti freccia giù, freccia giù, freccia sinistra, freccia destra.

{% highlight ruby %}
def tick args
    # inizializzazione (eseguita soltanto prima di disegnare il primo frame)
    args.state.player_x ||= 120
    args.state.player_y ||= 280

    # lettura dell'input e aggiornamento dello stato
    if args.inputs.left
       args.state.player_x -= 10
    elsif args.inputs.right
       args.state.player_x += 10
    end
    if args.inputs.up
       args.state.player_y += 10
    elsif args.inputs.down
       args.state.player_y -= 10
    end

    # scrittura dell'output
    args.outputs.sprites << {
        x:args.state.player_x, 
        y:args.state.player_y, 
        w:100, 
        h:80, 
        path:'sprites/dragon-0.png'}
end
{% endhighlight %}

In questo esempio durante la prima esecuzione della funzione tick vengono assegnate
la variabili, di tipo numero intero, **player_x e player_y** che sono situate
nel dizionario **state** che è contenuto nel dizionario **args**.

Il dizionario **args** contiene anche un dizionario chiamato **inputs**.
Questo dizionario contiene una variabile booleana per ciasun possibile input
l'utente possa immettere. 
Quando si va a valutare l'istruzione **if args.inputs.left** quello che si va 
a valutare è lo stato True o False della variabile booleana **left** contenuta
nel dizionario **inputs** contenuto a sua volta nel dizionario **args**.

Se questa risulta vera si va ad decrementare di 10 il contenuto della variabile
**player_x** che è contenuta nel dizionario **state** che a sua volta è contenuto
nel dizionario **args**.

## Output

L'output in dragonruby consiste nell'aggiungere dizionari alle liste:

* args.outputs.sprites
* args.outputs.labels

In ruby l'operatore **<<** permette di aggiungere un elemento ad una lista

{% highlight ruby %}
def tick args
    args.outputs.labels << {
        x:120, 
        y:120, 
        text:"Hello Dragon!"}
    args.outputs.sprites << {
        x:120, 
        y:180, 
        w:100, 
        h:80, 
        path:'sprites/misc/dragon-0.png'}
end
{% endhighlight %}

In questo modo definamo ad ogni tick cosa l'utente andrà a visualizzare in un
singolo frame. 

Le liste vengono svuotate subito dopo aver disegnato il frame.


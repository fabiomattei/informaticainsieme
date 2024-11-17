---
title: 'Dragonruby input'
date: '2020-11-16T08:47:01+01:00'
author: Fabio Mattei
layout: page
---

Per permettere all'utente di interagire con il nostro gioco dobbiamo raccogliere il suo input.
Dragonruby mette a disposizione il dizionario **inputs** contenuto nel dizionario **args**
a questo proposito.

## Tastiera

Abbiamo due modalità di lettura, possiamo leggere la pressione di un tasto oppure il suo rilascio.

Per leggere la pressione di un tasto utilizziamo **args.inputs.keyboard**, abbiamo diverse modalità di lettura:

{% highlight ruby %}
 # Per capire se il tasto G è stato appena premuto
 # oppure è stato premuto in precedenza e continua 
 # a restare premuto
 if args.inputs.keyboard.key_down.g
     # ....  
 end
{% endhighlight %}

{% highlight ruby %}
 # Per capire se il tasto G è stato premuto 
 # in precedenza e continua a restare premuto
  if args.inputs.keyboard.key_held.g
    args.state.g_key.key_held_at = args.inputs.keyboard.key_held.g
  end
{% endhighlight %}

{% highlight ruby %}
 # Per capire se il tasto G è stato appena premuto
 # oppure è stato premuto in precedenza e continua 
 # a restare premuto
  if args.inputs.keyboard.g
    args.state.g_key.key_down_or_held_at = args.inputs.keyboard.g
  end
{% endhighlight %}

{% highlight ruby %}
  # Per capire se il tasto G è stato rilasciato
  if args.inputs.keyboard.key_up.g
    args.state.g_key.key_up_at = args.inputs.keyboard.key_up.g
  end
{% endhighlight %}

{% highlight ruby %}
  # Per capire se è stata premuta la combinazione
  # Ctrl + G
  if args.inputs.keyboard.ctrl_g
    args.state.g_key.ctrl_at = args.inputs.keyboard.ctrl_g
  end
{% endhighlight %}

Per una descrizione più esaustiva rimando alla documentazione ufficiale: https://docs.dragonruby.org/static/docs.html#-----up-


## Mouse 

Il mouse ci dà diversi tipi di input, il tutto si trova all'interno del dizionario **args.inputs.mouse**.

1. posizione corrente del mouse: si trova nelle variabili int x e y

{% highlight ruby %}
def tick args
    xcorrente = args.inputs.mouse.x
    ycorrente = args.inputs.mouse.y
end
{% endhighlight %}

2. posizione del mouse nel frame precedente: si trova nelle variabili inte previous_x e previous_y

{% highlight ruby %}
def tick args
    xprecedente = args.inputs.mouse.previous_x
    yprecedente = args.inputs.mouse.previous_y
end
{% endhighlight %}

3. differenza tra la posizione corrente e la posizione precedente: si trova nelle variabili inte relative_x e relative_y

{% highlight ruby %}
def tick args
    dx = args.inputs.mouse.relative_x
    dy = args.inputs.mouse.relative_y
end
{% endhighlight %}

4. controllo se il mouse si trova all'interno di un rettangolo: inside_rect?

metodo che restituisce un booleano e prende come parametri to x, y, w, h.

{% highlight ruby %}
def tick args
    isinside = args.inputs.mouse.inside_rect? 100, 120, 80, 60
end
{% endhighlight %}

5. controlla la pressione dei bottoni del mouse, variabili booleane: button_left, button_middle, button_right

{% highlight ruby %}
def tick args
    premutosinistra = args.inputs.mouse.button_left
    premutocentrale = args.inputs.mouse.button_middle
    premutodestra = args.inputs.mouse.button_right
end
{% endhighlight %}

## Controller

Dragonruby può gestire fino a 4 controller: args.inputs.controller_one, args.inputs.controller_two, args.inputs.controller_three, args.inputs.controller_four

Per ciasun controller può controllare la pressione dei tasti per le quattro direzioni: up, down, left, right

{% highlight ruby %}
def tick args
    args.state.giocatore.x , , = 600
    args.state.giocatore.y , , = 400
	
    if args.inputs.controller_one.left
	   args.state.giocatore.x -= 10
	elsif args.inputs.controller_one.right
	   args.state.giocatore.x += 10
	end 
	
    if args.inputs.controller_one.up
	   args.state.giocatore.x += 10
	elsif args.inputs.controller_one.down
	   args.state.giocatore.x -= 10
	end
end
{% endhighlight %}

In modo analogo il sistema gestisce la pressione dei bottoni: a, b, x, y, l1, r1, l2, r2, l3, r3, start, select


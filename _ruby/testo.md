---
title: 'Dragonruby: raccogliere il testo digitato'
date: '2020-11-16T08:47:01+01:00'
author: Fabio Mattei
layout: page
---

Nella pagina sull'[input]({{ site.baseurl }}{% link _ruby/input.md %}.html) abbiamo visto
come leggere la pressione di un singolo tasto, utile per muovere un personaggio. A volte
però serve raccogliere del testo vero e proprio digitato dal giocatore, ad esempio per
chiedere il suo nome prima di iniziare la partita, da mostrare poi nella classifica dei
record.

## Leggere un carattere alla volta

**args.inputs.keyboard.key_down.char** restituisce, nel tick in cui viene premuto un tasto,
il carattere corrispondente sotto forma di stringa (già considerando maiuscole, minuscole e
tastiera nazionale), oppure **nil** se in quel tick non è stato digitato nessun carattere.

{% highlight ruby %}
def tick args
    args.state.nome ||= ""

    if args.inputs.keyboard.key_down.char
        args.state.nome += args.inputs.keyboard.key_down.char
    end

    if args.inputs.keyboard.key_down.backspace
        args.state.nome = args.state.nome[0...-1]
    end

    args.outputs.labels << {
        x: 640,
        y: 400,
        text: "Nome: #{args.state.nome}",
        anchor_x: 0.5
    }
end
{% endhighlight %}

**backspace** va gestito a parte, perché non è un carattere da aggiungere ma un comando per
togliere l'ultimo carattere digitato. **nome[0...-1]** restituisce la stringa senza il suo
ultimo carattere: se nome è vuota, il risultato resta semplicemente una stringa vuota.

## Una schermata di inserimento nome

Metttendo insieme questa tecnica con lo [stato del gioco e le scene]({{ site.baseurl }}{% link _ruby/scene.md %}.html)
possiamo costruire una vera schermata iniziale, che chiede il nome e passa al gioco solo
quando il giocatore preme invio.

{% highlight ruby %}
def tick_inserisci_nome args
    args.state.nome ||= ""

    args.outputs.labels << {
        x: 640,
        y: 400,
        text: "Come ti chiami? #{args.state.nome}",
        anchor_x: 0.5
    }

    if args.inputs.keyboard.key_down.char
        args.state.nome += args.inputs.keyboard.key_down.char
    end

    if args.inputs.keyboard.key_down.backspace
        args.state.nome = args.state.nome[0...-1]
    end

    if args.inputs.keyboard.key_down.enter && args.state.nome.length > 0
        args.state.scena = :gioco
    end
end
{% endhighlight %}

## Limitare la lunghezza del nome

Per evitare che il giocatore scriva un nome lunghissimo che poi non entra nella schermata
della classifica, basta smettere di accettare nuovi caratteri superata una certa lunghezza.

{% highlight ruby %}
if args.inputs.keyboard.key_down.char && args.state.nome.length < 10
    args.state.nome += args.inputs.keyboard.key_down.char
end
{% endhighlight %}

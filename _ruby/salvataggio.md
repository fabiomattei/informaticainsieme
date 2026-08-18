---
title: 'Dragonruby: salvare e caricare i dati'
date: '2020-11-16T08:47:01+01:00'
author: Fabio Mattei
layout: page
---

Lo [stato del gioco]({{ site.baseurl }}{% link _ruby/scene.md %}.html), come **args.state**,
esiste soltanto mentre il gioco è in esecuzione: appena il giocatore chiude il programma,
tutto quello che contiene, punteggio compreso, viene perso. Per ricordare informazioni da
una partita all'altra, ad esempio il record migliore, dobbiamo scrivere quei dati su un file.

## Scrivere un file

**args.gtk.write_file** accetta il percorso del file e il contenuto da scrivere, sotto
forma di stringa di testo.

{% highlight ruby %}
def tick args
    if args.inputs.keyboard.key_down.s
        args.gtk.write_file("dati/record.txt", args.state.punteggio.to_s)
    end
end
{% endhighlight %}

Notiamo l'uso di **to_s**: il file può contenere solo testo, quindi un numero come il
punteggio va prima convertito in stringa.

## Leggere un file

**args.gtk.read_file** restituisce il contenuto del file come stringa, oppure **nil** se il
file non esiste ancora (ad esempio la primissima volta che il gioco viene avviato).

{% highlight ruby %}
def tick args
    args.state.record ||= (args.gtk.read_file("dati/record.txt") || "0").to_i
end
{% endhighlight %}

Qui usiamo **|| "0"** per gestire il caso in cui il file non esista ancora: se
**read_file** restituisce nil, al suo posto usiamo la stringa "0", che convertita con
**to_i** diventa il numero 0.

## Un record che si aggiorna da solo

Mettendo insieme lettura e scrittura possiamo tenere un file sempre aggiornato con il
punteggio più alto ottenuto, aggiornandolo solo quando serve davvero, cioè quando il
punteggio della partita corrente supera il record salvato.

{% highlight ruby %}
def tick args
    args.state.record ||= (args.gtk.read_file("dati/record.txt") || "0").to_i

    args.outputs.labels << {
        x: 20,
        y: 700,
        text: "Punteggio: #{args.state.punteggio}   Record: #{args.state.record}"
    }

    if args.state.punteggio > args.state.record
        args.state.record = args.state.punteggio
        args.gtk.write_file("dati/record.txt", args.state.record.to_s)
    end
end
{% endhighlight %}

## Salvare dati più complessi

Un singolo numero è facile da salvare, ma un gioco più completo ha bisogno di salvare più
informazioni insieme: il record, il livello raggiunto, le impostazioni audio. In questo
caso conviene salvare i dati in formato **JSON**, che dragonruby sa leggere direttamente con
**args.gtk.parse_json_file**.

{% highlight ruby %}
def tick args
    if args.inputs.keyboard.key_down.s
        dati = { record: args.state.record, livello: args.state.livello }
        args.gtk.write_file("dati/salvataggio.json", dati.to_json)
    end

    if args.inputs.keyboard.key_down.l
        dati = args.gtk.parse_json_file("dati/salvataggio.json")
        args.state.record = dati["record"]
        args.state.livello = dati["livello"]
    end
end
{% endhighlight %}

I file scritti con **write_file** vengono salvati in una cartella dedicata al gioco,
diversa dai file che si trovano nella cartella del progetto: in questo modo il
salvataggio funziona anche una volta che il gioco viene esportato e installato sul
computer di un giocatore.

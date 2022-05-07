---
id: 20
title: 'Template vuoto'
date: '2020-01-20T17:37:45+01:00'
author: fabio
layout: page
guid: 'https://www.esercizidiinformatica.it/?page_id=20'
---

Il seguente codice è un template vuoto, un modello da riempire nella costruzione di un videogioco. Rimedia alla fase tediosa di cercare nella libreria le basi per partire e toglie il programmatore dal empasse della pagine bianca

{% endhighlight %}
<pre class="wp-block-code">{% endhighlight %}
"""
Starting Template

Questo template si avvale delle classi e della programmazione ad oggetti

Se Python e Arcade sono installati questo programma può essere lanciato così:
python -m starting_template.py
"""

import arcade

SCREEN_WIDTH = 800
SCREEN_HEIGHT = 600
SCREEN_TITLE = "Starting Template"

class MyGame(arcade.Window):
    """
    Classe principale per l'applicazione

    NOTE: Usa quello di cui hai bisogno, cancella quello che non ti serve.
    Se hai bisogno di uno dei seguenti metodi cancella il 'pass' e rimpiazzalo
    con il tuo codice. Non lasciare 'pass' in questo programma.
    """

    def __init__(self, width, height, title):
        # Chiama il costruttore della classe padre (non toccare)
        super().__init__(width, height, title)

        arcade.set_background_color(arcade.color.AMAZON)

        # Se hai una lista di sprite dovresti crearli qui e settarli a None

    def setup(self):
        # Crea qui i tuoi sprite e le tue sprite lists
        pass

    def on_draw(self):
        """
        Disegna (Render) la schermata grafica
        """

        # Questo metodo va chiamato come prima cosa, prima di iniziare a disegnare. It will clear
        # Cancellerà lo scehrmo al colore di sfondo (background) e cancellerà quanto disegnato nel 
        # frame (schermata) precedente
        arcade.start_render()

        # Chiama il metodo draw() su tutte le tue liste di sprite.

    def on_update(self, delta_time):
        """
        Tutta la logica per spostare gli sprite e calcolare i punteggi va qui
        Normalmente chiamerai update() sulle liste di sprite che ne hanno bisogno
        """
        pass

    def on_key_press(self, key, key_modifiers):
        """
        Chiamata ogni volta che un tasto viene premuto sulla tastiera.
        Per vedere la lista di tutti i tasti: http://arcade.academy/arcade.key.html
        """
        pass

    def on_key_release(self, key, key_modifiers):
        """
        Chiamato quando l'utente rilascia un tasto premuto in precedenza
        """
        pass

    def on_mouse_motion(self, x, y, delta_x, delta_y):
        """
        Chiamato quando si muove il mouse
        """
        pass

    def on_mouse_press(self, x, y, button, key_modifiers):
        """
        Chiamato quando viene premuto un bottone sul mouse
        """
        pass

    def on_mouse_release(self, x, y, button, key_modifiers):
        """
        Chiamato quanto viene rilasciato un bottone sul mouse
        """
        pass


def main():
    """ Metodo principale """
    game = MyGame(SCREEN_WIDTH, SCREEN_HEIGHT, SCREEN_TITLE)
    game.setup()
    arcade.run()


if __name__ == "__main__":
    main()
    
{% endhighlight %}
{% endhighlight %}
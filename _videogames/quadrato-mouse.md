---
id: 15
title: 'Palla controllata dal mouse'
date: '2020-01-20T13:03:52+01:00'
author: fabio
layout: page
guid: 'https://www.esercizidiinformatica.it/?page_id=15'
---

{% endhighlight %}
<pre class="wp-block-code">{% endhighlight %}
"""
Questa semplice animazione mostra come muovere uno sprite utilizzando il mouse e gestire il click del mouse.
"""

import arcade

SCREEN_WIDTH = 640
SCREEN_HEIGHT = 480
SCREEN_TITLE = "Move Mouse Example"



class Ball:
    """
    Questa classe gestisce lo sprite Ball.
    E' bene creare una classe per ogni tipo di sprite che si vuole includere nel videogioco
    """
    def __init__(self, position_x, position_y, radius, color):
        # Costruttore prende i parametri che gli sono passati e crea una istanza della classe
        self.position_x = position_x
        self.position_y = position_y
        self.radius = radius
        self.color = color

    def draw(self):
        """ Disegna lo sprite utilizzando le informazioni passate al costruttore. """
        arcade.draw_circle_filled(self.position_x, self.position_y, self.radius, self.color)


class MyGame(arcade.Window):

    def __init__(self, width, height, title):
        # Chiama il costruttore della classe padre
        super().__init__(width, height, title)

        # Fa sparire il mouse quando è dentro la finestra del videogioco.
        # Così vediamo solo il nostro sprite e non il puntatore.
        self.set_mouse_visible(False)

        arcade.set_background_color(arcade.color.ASH_GREY)

        # Crea una istanza della classe Ball
        self.ball = Ball(50, 50, 15, arcade.color.AUBURN)

    def on_draw(self):
        """ Disegna (Render) la schermata grafica """
        arcade.start_render()
        # viene chiamato il metodo draw di tutti gli sprite
        self.ball.draw()

    def on_mouse_motion(self, x, y, dx, dy):
        """ Chiamato per aggiornare le nostre istanze. Chiamato approssimativamente 60 volte al secondo."""
        self.ball.position_x = x
        self.ball.position_y = y

    def on_mouse_press(self, x, y, button, modifiers):
        """
        Chiamato quando viene premuto un bottone sul mouse
        """
        print(f"You clicked button number: {button}")
        if button == arcade.MOUSE_BUTTON_LEFT:
            self.ball.color = arcade.color.BLACK

    def on_mouse_release(self, x, y, button, modifiers):
        """
        Chiamato quanto viene rilasciato un bottone sul mouse
        """
        if button == arcade.MOUSE_BUTTON_LEFT:
            self.ball.color = arcade.color.AUBURN


def main():
    MyGame(SCREEN_WIDTH, SCREEN_HEIGHT, SCREEN_TITLE)
    arcade.run()


if __name__ == "__main__":
    main()
    
    
{% endhighlight %}
{% endhighlight %}
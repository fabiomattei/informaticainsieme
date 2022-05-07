---
id: 17
title: 'Palla contrllata dalla tastiera'
date: '2020-01-20T13:14:25+01:00'
author: fabio
layout: page
guid: 'https://www.esercizidiinformatica.it/?page_id=17'
---

```
<pre class="wp-block-code">```
"""
Questa semplice animazione mostra come muovere uno sprite utilizzando la tastiera
"""

import arcade

SCREEN_WIDTH = 640
SCREEN_HEIGHT = 480
SCREEN_TITLE = "Move Keyboard Example"
MOVEMENT_SPEED = 3


class Ball:
    """
    Questa classe gestisce lo sprite Ball.
    E' bene creare una classe per ogni tipo di sprite che si vuole includere nel videogioco
    """
    def __init__(self, position_x, position_y, change_x, change_y, radius, color):
        # Costruttore prende i parametri che gli sono passati e crea una istanza della classe
        self.position_x = position_x
        self.position_y = position_y
        self.change_x = change_x
        self.change_y = change_y
        self.radius = radius
        self.color = color

    def draw(self):
        """ Disegna lo sprite utilizzando le informazioni passate al costruttore. """
        arcade.draw_circle_filled(self.position_x, self.position_y, self.radius, self.color)

    def update(self):
        # Muove la palla
        self.position_y += self.change_y
        self.position_x += self.change_x

        # Controlla se la palla tocca i margini dello schermo. Se è così la ferma.
        if self.position_x < self.radius:
            self.position_x = self.radius

        if self.position_x > SCREEN_WIDTH - self.radius:
            self.position_x = SCREEN_WIDTH - self.radius

        if self.position_y < self.radius:
            self.position_y = self.radius

        if self.position_y > SCREEN_HEIGHT - self.radius:
            self.position_y = SCREEN_HEIGHT - self.radius


class MyGame(arcade.Window):

    def __init__(self, width, height, title):
        # Chiama il costruttore della classe padre
        super().__init__(width, height, title)

        # Fa sparire il mouse quando è dentro la finestra del videogioco.
        # Così vediamo solo il nostro sprite e non il puntatore.
        self.set_mouse_visible(False)

        arcade.set_background_color(arcade.color.ASH_GREY)

        # Crea una istanza della classe Ball
        self.ball = Ball(50, 50, 0, 0, 15, arcade.color.AUBURN)

    def on_draw(self):
        """ Disegna (Render) la schermata grafica """
        arcade.start_render()
        self.ball.draw()

    def on_update(self, delta_time):
        # chiama il metodo update() della classe Ball
        self.ball.update()

    def on_key_press(self, key, modifiers):
        """ Chiamata ogni volta che un tasto viene premuto sulla tastiera """
        if key == arcade.key.LEFT:
            self.ball.change_x = -MOVEMENT_SPEED
        elif key == arcade.key.RIGHT:
            self.ball.change_x = MOVEMENT_SPEED
        elif key == arcade.key.UP:
            self.ball.change_y = MOVEMENT_SPEED
        elif key == arcade.key.DOWN:
            self.ball.change_y = -MOVEMENT_SPEED

    def on_key_release(self, key, modifiers):
        """ Chiamato quando l'utente rilascia un tasto premuto in precedenza """
        if key == arcade.key.LEFT or key == arcade.key.RIGHT:
            self.ball.change_x = 0
        elif key == arcade.key.UP or key == arcade.key.DOWN:
            self.ball.change_y = 0


def main():
    MyGame(SCREEN_WIDTH, SCREEN_HEIGHT, SCREEN_TITLE)
    arcade.run()


if __name__ == "__main__":
    main()
```
```
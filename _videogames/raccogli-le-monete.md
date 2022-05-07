---
id: 31
title: 'Raccogli le monete'
date: '2020-01-20T17:56:15+01:00'
author: fabio
layout: page
guid: 'https://www.esercizidiinformatica.it/?page_id=31'
---

Il seguente script presenta uno sprite, controllato dal mouse che deve raccogliere tutte le monete.

{% highlight shell %}

"""
Lo sprite raccoglie le monete
"""

import random
import arcade
import os

# --- Constants ---
SPRITE_SCALING_PLAYER = 0.5
SPRITE_SCALING_COIN = .25
COIN_COUNT = 50

SCREEN_WIDTH = 800
SCREEN_HEIGHT = 600
SCREEN_TITLE = "Sprite Collect Coins Example"

class Giocatore(arcade.Sprite):
    """
    Questa classe gestisce lo sprite Giocatore.
    Per la documentazione della classe Sprite: http://arcade.academy/arcade.html?highlight=sprite#arcade.Sprite
    """
    def __init__(self, image, scale, center_x, center_y):
        super().__init__(image, scale)
        self.center_x = center_x
        self.center_y = center_y
        
        
class Moneta(arcade.Sprite):
    """
    Questa classe gestisce lo sprite Moneta.
    Per la documentazione della classe Sprite: http://arcade.academy/arcade.html?highlight=sprite#arcade.Sprite
    """
    def __init__(self, image, scale, center_x, center_y):
        super().__init__(image, scale)
        self.center_x = center_x
        self.center_y = center_y
        

class MyGame(arcade.Window):
    """ Our custom Window Class"""

    def __init__(self):
        # Chiama il costruttore della classe padre
        super().__init__(SCREEN_WIDTH, SCREEN_HEIGHT, SCREEN_TITLE)

        # Pone la directory di lavoro (working directory) (ci serve per trovare i file delle immagini)
        # alla stessa directory in cui si trova questo file .py 
        file_path = os.path.dirname(os.path.abspath(__file__))
        os.chdir(file_path)

        # Le due liste di sprite
        self.player_list = None
        self.coin_list = None

        # Sprite giocatore
        self.player_sprite = None
        
        # Punteggio    
        self.score = 0

        # Fa sparire il mouse quando è dentro la finestra del videogioco.
        self.set_mouse_visible(False)

        arcade.set_background_color(arcade.color.AMAZON)

    def setup(self):
        # Crea qui i tuoi sprite e le tue sprite lists

        # Liste di sprite
        self.player_list = arcade.SpriteList()
        self.coin_list = arcade.SpriteList()

        # Score
        self.score = 0

        # Crea una istanza di Giocatore
        self.player_sprite = Giocatore(":resources:images/animated_characters/female_person/femalePerson_idle.png", SPRITE_SCALING_PLAYER, 50, 50)
        # Aggiungi il giocatore alla lista player_list
        self.player_list.append(self.player_sprite)

        # Crea le istanze di tutte le monete
        # ripeti 50 volte
        for i in range(COIN_COUNT): 
            
            # scegli una posizione a caso
            x_pos = random.randrange(SCREEN_WIDTH)
            y_pos = random.randrange(SCREEN_HEIGHT)
            
            # Crea una istanza di moneta utilizzando la posizione a caso
            coin = Moneta(":resources:images/items/coinGold_ul.png", SPRITE_SCALING_COIN, x_pos, y_pos)

            # Add the coin to the lists
            self.coin_list.append(coin)

    def on_draw(self):
        """ Disegna (Render) la schermata grafica """
        
        arcade.start_render()
        
        # chiama il metodo draw per le due liste di sprites
        self.coin_list.draw()
        self.player_list.draw()

        # Mette il punteggio sullo schermo
        output = f"Score: {self.score}"
        arcade.draw_text(output, 10, 20, arcade.color.WHITE, 14)

    def on_mouse_motion(self, x, y, dx, dy):
        """ Chiamato quando si muove il mouse """

        # Muove il centro dello sprite Giocatore alle coordinate x, y del mouse
        self.player_sprite.center_x = x
        self.player_sprite.center_y = y

    def on_update(self, delta_time):
        """ Movimenti e logica di gioco """

        # Chiama il metodo update() su tutti gli sprite
        self.player_list.update()
        self.coin_list.update()

        # Genera una lista contenente tutti gli sprite che toccano il giocadore (individuo le collisioni)
        coins_hit_list = arcade.check_for_collision_with_list(self.player_sprite, self.coin_list)

        # Con una iterazione sulla lista appena creata rimuoviamo gli sprite colpiti e incrementiamo il punteggio
        for coin in coins_hit_list:
            coin.remove_from_sprite_lists()
            self.score += 1


def main():
    """ Metodo principale """
    window = MyGame()
    window.setup()
    arcade.run()


if __name__ == "__main__":
    main()

{% endhighlight %}
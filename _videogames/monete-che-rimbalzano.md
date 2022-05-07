---
id: 39
title: 'Monete che rimbalzano'
date: '2020-01-20T18:01:06+01:00'
author: fabio
layout: page
guid: 'https://www.esercizidiinformatica.it/?page_id=39'
---

Le monete si muovono nel campo di gioco rimblazando sullo schermo. Fai presto a raccoglierle tutte.

{% endhighlight %}
<pre class="wp-block-code">{% endhighlight %}
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
MOVEMENT_SPEED = 5
TEXTURE_LEFT = 0
TEXTURE_RIGHT = 1

SCREEN_WIDTH = 800
SCREEN_HEIGHT = 600
SCREEN_TITLE = "Sprite Collect Coins Example"

class Giocatore(arcade.Sprite):
    """
    Questa classe gestisce lo sprite Giocatore.
    Per la documentazione della classe Sprite: http://arcade.academy/arcade.html?highlight=sprite#arcade.Sprite
    """
    def __init__(self):
        super().__init__()

        # Carica le immagini (textures) che guardano a destra e a sinistra
        # mirrored=True fa l'immainge a specchio di quella caricata
        texture = arcade.load_texture(":resources:images/animated_characters/female_person/femalePerson_walk0.png", mirrored=True, scale=SPRITE_SCALING_PLAYER)
        self.textures.append(texture)
        texture = arcade.load_texture(":resources:images/animated_characters/female_person/femalePerson_walk0.png", scale=SPRITE_SCALING_PLAYER)
        self.textures.append(texture)

        # By default, face right.
        self.set_texture(TEXTURE_RIGHT)
        
    def update(self):
        self.center_x += self.change_x
        self.center_y += self.change_y
        
        # Capisce se devo usare la texture rivolta a destra o quella rivolta a sinistra
        if self.change_x < 0:
            self.set_texture(TEXTURE_LEFT)
        if self.change_x > 0:
            self.set_texture(TEXTURE_RIGHT)

        if self.left < 0:
            self.left = 0
        elif self.right > SCREEN_WIDTH - 1:
            self.right = SCREEN_WIDTH - 1

        if self.bottom < 0:
            self.bottom = 0
        elif self.top > SCREEN_HEIGHT - 1:
            self.top = SCREEN_HEIGHT - 1
        
        
class Moneta(arcade.Sprite):
    """
    Questa classe gestisce lo sprite Moneta.
    Per la documentazione della classe Sprite: http://arcade.academy/arcade.html?highlight=sprite#arcade.Sprite
    """
    def __init__(self, image, scale, center_x, center_y, change_x, change_y):
        super().__init__(image, scale)
        self.center_x = center_x
        self.center_y = center_y
        self.change_x = change_x
        self.change_y = change_y
        
    def update(self):
        # Muovi la moneta
        self.center_x += self.change_x
        self.center_y += self.change_y

        # Se la moneta arriva ai margini dello schermo allora rimbalza
        if self.left < 0:
            self.change_x *= -1

        if self.right > SCREEN_WIDTH:
            self.change_x *= -1

        if self.bottom < 0:
            self.change_y *= -1

        if self.top > SCREEN_HEIGHT:
            self.change_y *= -1
        

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
        self.player_sprite = Giocatore()
        self.player_sprite.center_x = 50
        self.player_sprite.center_y = 50
        # Aggiungi il giocatore alla lista player_list
        self.player_list.append(self.player_sprite)

        # Crea le istanze di tutte le monete
        # ripeti 50 volte
        for i in range(COIN_COUNT): 
            
            # scegli una posizione a caso
            x_pos = random.randrange(SCREEN_WIDTH)
            y_pos = random.randrange(SCREEN_HEIGHT)
            cambia_x = random.randrange(-3, 4)
            cambia_y = random.randrange(-3, 4)
            
            # Crea una istanza di moneta utilizzando la posizione a caso
            coin = Moneta(":resources:images/items/coinGold_ul.png", SPRITE_SCALING_COIN, x_pos, y_pos, cambia_x, cambia_y)

            # Add the coin to the lists
            self.coin_list.append(coin)

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

    def on_draw(self):
        """ Disegna (Render) la schermata grafica """
        
        arcade.start_render()
        
        # chiama il metodo draw per le due liste di sprites
        self.coin_list.draw()
        self.player_list.draw()

        # Mette il punteggio sullo schermo
        output = f"Score: {self.score}"
        arcade.draw_text(output, 10, 20, arcade.color.WHITE, 14)


    def on_key_press(self, key, modifiers):
        """
        Chiamata ogni volta che un tasto viene premuto sulla tastiera.
        Per vedere la lista di tutti i tasti: http://arcade.academy/arcade.key.html
        """
        if key == arcade.key.UP:
            self.player_sprite.change_y = MOVEMENT_SPEED
        elif key == arcade.key.DOWN:
            self.player_sprite.change_y = -MOVEMENT_SPEED
        elif key == arcade.key.LEFT:
            self.player_sprite.change_x = -MOVEMENT_SPEED
        elif key == arcade.key.RIGHT:
            self.player_sprite.change_x = MOVEMENT_SPEED


    def on_key_release(self, key, modifiers):
        """
        Chiamato quando l'utente rilascia un tasto premuto in precedenza
        """
        if key == arcade.key.UP or key == arcade.key.DOWN:
            self.player_sprite.change_y = 0
        elif key == arcade.key.LEFT or key == arcade.key.RIGHT:
            self.player_sprite.change_x = 0


def main():
    """ Metodo principale """
    window = MyGame()
    window.setup()
    arcade.run()


if __name__ == "__main__":
    main()

{% endhighlight %}
{% endhighlight %}
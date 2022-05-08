---
id: 138
title: 'I frattali con turtle'
date: '2020-02-04T15:57:56+01:00'
author: Fabio Mattei
layout: page
guid: 'https://www.esercizidiinformatica.it/?page_id=138'
---

<figure class="wp-block-image size-large">![](https://www.esercizidiinformatica.it/wp-content/uploads/2020/02/frattalistella-1024x992.png)</figure>{% highlight python %}
import turtle
turtle.hideturtle()
turtle.pencolor('red')
turtle.speed(0)
turtle.penup()
turtle.setx(-260)
turtle.sety(-150)
turtle.left(60)
turtle.pendown()
def koch(n,d):
   if(n == 1):
      turtle.forward(d)
   elif(n > 1):
      d3=d/3
      koch(n-1,d3); turtle.left(60)
      koch(n-1,d3); turtle.right(120)
      koch(n-1,d3); turtle.left(60)
      koch(n-1,d3)
RICORSIONE=4
DISTANZA=550
for i in range(1,RICORSIONE+1):
   turtle.pensize(i)
   koch(i,DISTANZA); turtle.right(120)
   koch(i,DISTANZA); turtle.right(120)
   koch(i,DISTANZA); turtle.right(120)
turtle.done()
{% endhighlight %}

</div><figure class="wp-block-image size-large">![](https://www.esercizidiinformatica.it/wp-content/uploads/2020/02/frattalialbero-1024x975.png)</figure>{% highlight python %}
import turtle
#--------------------------
turtle.shape('turtle')
turtle.pencolor('red')
turtle.pensize(3)
turtle.speed(0)
turtle.penup()
turtle.sety(-280)
turtle.pendown()
turtle.left(90)
#--------------------------
def albero(n,d):
    if(n > 0):
        d2=d/RATIO
        turtle.forward(d)
        turtle.left(45)
        albero(n-1,d2)
        turtle.right(90)
        albero(n-1,d2)
        turtle.left(45)
        turtle.backward(d)
#--------------------------
RICORSIONE=9
DISTANZA=250
RATIO=1.65
albero(RICORSIONE,DISTANZA)
turtle.done()
{% endhighlight %}

</div>
---
title: 'Dragonruby: labels'
date: '2020-11-15T08:47:01+01:00'
author: Fabio Mattei
layout: page
---



{% highlight ruby %}
mylabel = {
	x: 320,
    y: 640,
    text: "Text",
    font: "fonts/font.ttf",
    anchor_x: 0.5, # or alignment_enum: 0, 1, or 2
    anchor_y: 0.5, # or vertical_alignment_enum: 0, 1, or 2
    r: 0,
    g: 0,
    b: 0,
    a: 255,
    size_px: 20,   # or size_enum: -10 to 10 (0 means "ledgible on small devices" ie: 20px)
    blendmode_enum: 1 
}
{% endhighlight %}
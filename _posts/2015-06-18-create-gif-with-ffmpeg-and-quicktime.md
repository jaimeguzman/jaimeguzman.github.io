---
layout: post
title: Create gif with ffmpeg and quicktime player 
date: "2015-06-18 18:55 -0300"
comments: true
published: true
---

Today I learn how to create gif with ffmpeg and quicktime player. Well I'm researching and looking issue on drupal.org issue queue and look this awesome comment from  [Rachel Lawson](https://github.com/rachellawson) . And she response my message on d.o.
It's a really cool script for people that love gif =) 

----------


``` bash
#!/bin/sh
 
# Create an animated GIF of a screen recording to upload to d.o
# Usage: creategif input_file.mp4 output_file.gif
# As described at http://blog.pkh.me/p/21-high-quality-gif-with-ffmpeg.html
 
palette="/tmp/palette.png"
 
filters="fps=15,scale=1280:-1:flags=lanczos"
 
# GIF supports a 256 colour palette. Determine the best palette and store as temp file
ffmpeg -v warning -i $1 -vf "$filters,palettegen" -y $palette
 
# Use the stored palette to encode the GIF
ffmpeg -v warning -i $1 -i $palette -lavfi "$filters [x]; [x][1:v] paletteuse" -y $2

```
[Gist Link ](https://gist.github.com/rachellawson/3168e8abeca9b46ff30b)

> More info : [High quality gif with ffmpeg ](http://blog.pkh.me/p/21-high-quality-gif-with-ffmpeg.html)

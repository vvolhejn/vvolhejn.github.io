---
layout: single
title:  "Video: I can prove I’ve solved this Sudoku without revealing it"
date:   2025-03-16 00:00:00 +0100
categories:
header:
  teaser: /assets/images/polylog/zero-knowledge-proofs-thumbnail.jpg
---

A Polylog video about zero-knowledge proofs, a mathematical magic trick.

<iframe width="560" height="315" src="https://www.youtube.com/embed/Otvcbw6k4eo?si=yKebkrOQC-7vuoxl" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

This time, we joined forces with Tom Sláma who has [his own YouTube
channel](https://www.youtube.com/@YTomS) dedicated to similar topics to
Polylog's. He helped out a lot. One thing I'm especially happy about is that he
introduced us to the animation software [Motion
Canvas](https://motioncanvas.io/), which is the
[Manim](https://www.manim.community/) alternative we've been missing [for so long]({% post_url 2024-08-19-p-vs-np-video %}).
Tom also wrote an excellent [tutorial](https://slama.dev/motion-canvas/introduction/) about Motion Canvas for Manim users.

Motion Canvas' killer feature is the live preview – the animations get autoreloaded in a web GUI when you change something in your code, and you don't need to wait for the whole thing to re-render.
From a more technical standpoint, it has better thought-through semantics of how things are animated. Objects have properties such as scale, position, opacity, and you animate these in a uniform manner.
You can also control them declaratively by assigning callbacks as values. For example, I can say that an arrow is always supposed to be pointing at some target object, and then I don't have to remember to manually update the arrow.
It's also much clearer what happens if you declare a bunch of animations in advance and then play them. In Manim, the semantics of animations scheduled for the future are a mystery to me.

I'm thinking about making a whole video comparing Motion Canvas and Manim because it's a great case study of software design. There are still some rough edges, but it's an improvement on many axes.

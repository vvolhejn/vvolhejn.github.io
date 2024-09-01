---
layout: single
title:  "Video: What P vs NP is actually about"
date:   2024-08-19 00:00:00 +0100
categories:
header:
  teaser: /assets/images/p-vs-np-thumbnail.png
---

What if we could run algorithms backwards? We discuss how we could do this by turning algorithms into circuits, and how it all connects to P vs NP. 

<iframe width="1280" height="720" src="https://www.youtube.com/embed/6OPsH8PK7xM" title="What P vs NP is actually about" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

For this video, we had the help of Gabor Hollbeck, who did the video recording
and editing. It's definitely the most professional-looking Polylog video to
date!

I'm still very frustrated with [Manim](https://www.manim.community/), the Python
animation library we're using, but I do have to admit that it has one big
advantage: since it's animation-as-code, you get all the benefits of a modern
programming ecosystem, like code reusability and composability, version control,
and procedural generation.

For this video, I wrote a tool (code [here](https://github.com/polylog-cs/np-completeness))
for working with logical circuits: evaluating, visualizing, and reverting them.
By separating the visual and the logical parts, I could even write unit tests,
and these indeed discovered some bugs I wasn't aware of before!

This advantage of Manim is very particular to Polylog because we often work with
and visualize algorithms, and for this, the procedural generation would be
difficult to replace. Animating all of the circuits by hand would be a
nightmare. I still have high hopes for
[Cavalry](https://cavalry.scenegroup.co/). It's a very different animation tool
that is built primarily for animators rather than for coders, but they do have
strong procedural generation capabilities. You can write JavaScript to generate
Cavalry objects. I'd really like to try this for the next video!
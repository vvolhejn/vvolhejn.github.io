---
layout: single
title:  "Video: I made maps that show time instead of space"
date:   2024-03-24 00:00:00 +0100
categories:
header:
  teaser: /assets/images/spacetime-maps/video-still.jpg
---

The first video on my new [personal YouTube channel](https://www.youtube.com/@vvolhejn), explaining how spacetime maps work.

<iframe width="1280" height="720" src="https://www.youtube.com/embed/rC2VQ-oyDG0" title="I made maps that show time instead of space" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

See also [the post](/2023/11/25/spacetime-maps.html) about the spacetime maps themselves. 

This was not an easy project because I was learning several new tools at once:

- Davinci Resolve for video editing. I still did the recording and basic editing (like selecting which take to use) in Descript, but Resolve is significantly more mature and stable than Descript when it comes to video editing.
- [Cavalry](https://cavalry.scenegroup.co/) for animations. I was also looking for alternatives to Manim for [Polylog](https://www.youtube.com/@PolylogCS), our other YouTube chanel. People (us included) often use Manim for maths animations because they see 3Blue1Brown using it. But that doesn't mean it's always the right tool for the job, so I was trying out Cavalry to see if it could work. It's super interesting to see two tools that have very different approaches to the same problem of creating 2D animations. I might make a separate video or a blog post about that later.
- A custom webapp that I made for creating the freehand writing animations in the video. The code is not public at the moment because a lot of things I need are hardcoded and it wouldn't be useful to other people at this point. It's built on top of the [Perfect Freehand](https://perfect-freehand-example.vercel.app/) JS library and it uses FFMPEG running in WebAssembly to assemble single frames into a video file. I think this approach was recommended to me by ChatGPT – the fact that you can run FFMPEG directly in the browser is amazing. Thank you, robot!
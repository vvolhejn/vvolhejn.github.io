---
layout: single
title: 'Sine Wave Speech in Python'
date: 2023-05-03 00:00:00 +0100
categories:
header:
  teaser: /assets/images/sws-sentence-sine-wave.png
---

A [Python package](https://github.com/vvolhejn/sine_wave_speech) for sine wave speech.
What's that? Let's listen to an example.
Can you tell what the voice is saying? I bet not:

<audio controls src="/assets/audio/sws-sentence-sine-wave.wav"></audio>

![](/assets/images/sws-sentence-sine-wave.png)

This sound has been turned into sine wave speech. Now listen to the original:

<audio controls src="/assets/audio/sws-sentence-original.wav"></audio>

![](/assets/images/sws-sentence-original.png)

If you go back and listen to the sine wave speech again, you'll probably be able to understand it.
This is a really surprising effect: after you hear a few before-and-after examples,
the originally unintelligible sine wave speech becomes easy to follow.
Check out [this video](https://www.youtube.com/watch?v=GCtTtKKAhyE) for a longer example.

I stumbled upon sine wave speech accidentally when researching something for my [Master's thesis](/2022/09/21/msc-thesis.html)
and thought it's really cool.
But to my surprise, no Python implementations were around! The only implementation I found was one by Dan Ellis from 1996, from before I was born!
I wanted to provide a modern implementation for people who'd want to play around with this cool effect.

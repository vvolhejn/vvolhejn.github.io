---
layout: single
# <wbr> allows word breaking - othewise the title it doesn't fit onto one line
# on desktop and we get an ellipsis
title:  "sinewavespeech<wbr>.com: now LIVE!"
date:   2024-11-17 00:00:00 +0100
categories:
header:
  teaser: /assets/images/sine-wave-speech/sinewavespeech-com-live-thumbnail.png
---

Turn any sound into music, in real time, in your browser.

[![](/assets/images/sine-wave-speech/sws-live-screenshot.png)](https://sinewavespeech.com)

[sinewavespeech.com](https://sinewavespeech.com) is an interactive website for exploring how human speech can be reduced *extremely* and still be intelligible.
You can find the source code [on my GitHub](https://github.com/vvolhejn/sine_wave_speech).

About a year before this project, I made [sinewavespeech.com/explanation/](https://sinewavespeech.com/explanation/),
which demonstrates the effect on a pre-sinewaved recording.
You can read more about that in [this post]({% post_url 2023-08-21-sinewavespeech-com %}).

# UX design

> I can already read the criticism, that 'there's no active EQ, no pickup this, it's not versatile'. \*You're\* not versatile.

That's Vulfpeck band leader Jack Stratton talking about their single-knob [Joe Dart bass guitar](https://youtu.be/9eOF7t4HgjE?t=49).
The musician having less control might seem like a bad thing, but it also means that the maker can ensure the bass sounds good no matter the settings.
This is also why I love the [Sausage Fattener](https://youtu.be/jHFzfZl6NQQ?t=63) VST.
It has two knobs. It makes the sound fat. That's it.

I tried to apply the same less-is-more philosophy to the sine wave speech effect.
I ended up with five parameters:
- Step size: how short or long the notes are.
- Number of waves: decompose into this many sine waves.
- Scale: whether the frequencies get snapped to musical notes, and how restrictive the scale is. More about that later.
- Gain: adjust the loudness.
- Depth: decrease the frequency of the lower sine waves to cover more of the frequency spectrum.

Additionally, there some features under the hood to prevent you from making something truly awful-sounding.
I compress the dynamic range of the resulting audio is compressed so that there aren't huge differences in loudness.
I also adjust the loudness based on the frequency of the sine wave to compensate for the fact that humans
[perceive certain frequencies to be louder](https://en.wikipedia.org/wiki/A-weighting).

Admittedly, my five knobs are a far cry from the one-knob ideal of the Joe Dart bass.
Dear reader, in my defense, I did resist the temptation of adding any of the following:
- Using other waves than just sines, like square or triangle waves
- Allowing to record for longer periods of time
- Uploading audio files
- Download the processed audio
- Trimming the recording to a smaller passage

If enough people complain about one of these things missing, I might still add it.
Maybe the way to keep things simple would be adding a simple/advanced mode trigger,
and hide the more complicated things under advanced mode.

# Code

In [the first iteration](https://sinewavespeech.com/explanation/) of the project, I implemented the effect in Python by translating [Matlab code from the 90s](https://github.com/vvolhejn/sine_wave_speech/tree/main/matlab_code_archive) that I found on the internet.
This time, I wanted everybody to be able to transform their voice without needing to write any code, straight from the browser.
Sadly, it's not easy to run Python in the browser and that meant that I had to translate the code once again.

The plan was: Use the [Web Audio API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Audio_API) to get detailed control over sound in the browser,
using an [AudioWorklet](https://developer.mozilla.org/en-US/docs/Web/API/Web_Audio_API/Using_AudioWorklet) that would run the sine wave speech effect in a separate thread.
The audio effect itself would be written in Rust and [compiled into WebAssembly](https://rustwasm.github.io/book/) so that it can run in the browser.

# Going real-time with Rust

<!-- https://www.toptal.com/webassembly/webassembly-rust-tutorial-web-audio -->

I chose to use Rust for a few reasons.
- Efficiency: I wasn't sure how computationally expensive the effect would be, and using JavaScript would mean risking that it ends up being too slow on phones etc.
- Libraries: JavaScript's scientific computing ecosystem is much less mature
  than Python's, so I'd have to write some functions which I'd get for free in
  Python. That's not really an argument in favor of Rust – and we'll see the
  situation isn't much better there – but it's an argument against JavaScript.
- Learning: I had never worked with Rust before this so I wanted to see what the hype is about. I also hadn't used WebAssembly.

_More coming soon._

<!-- ## The Amerustacean Dream

Remember how I said that JavaScript is way behind Python when it comes to scientific computing?
As it turns out, so is Rust (a language that is 20 years younger). Who knew!
 -->

<!-- the iphone silence issue -->
---
layout: single
title:  "Master Thesis: Accelerating Neural Audio Synthesis"
date:   2022-09-21 00:00:00 +0100
categories:
header:
  teaser: /assets/images/msc-thesis-thumbnail.webp
---

My ETH Zürich Master Thesis.
Making AI-generated instrument sounds fast enough to comfortably run in real-time.

The text is available [here](https://www.research-collection.ethz.ch/bitstream/handle/20.500.11850/571861/2/Volhejn_Vaclav.pdf)
and the code is [here](https://github.com/vvolhejn/thesis/).

![Thumbnail](/assets/images/msc-thesis-thumbnail.webp)

_Image adapted from [the DDSP paper](https://arxiv.org/abs/2001.04643)_

In short, I started with [DDSP](https://arxiv.org/abs/2001.04643) (see also [blog](https://magenta.tensorflow.org/ddsp)),
[RAVE](https://arxiv.org/abs/2111.05011),
and [NEWT](https://arxiv.org/abs/2107.05050)
as three baseline models for synthesising sounds of musical instruments.
The main application of these models is timbre transfer: you put in a melody performed in one instrument (such as your voice)
and you get back the same melody played on a violin, a trumpet or so, depending on what the model was trained on.

The dream was to be able to run one of these models in a [DAW](https://en.wikipedia.org/wiki/Digital_audio_workstation)
so that electronic music artists can use the model seamlessly.
The issue is that the models are very CPU intensive compared to regular synthesizers – when I started working on the thesis, they were _barely_ real-time.
I used techniques such as [neural network quantization](https://arxiv.org/abs/2103.13630) and [pruning](https://arxiv.org/pdf/2102.00554.pdf) to speed the models up.
A summary of the findings is:
- The original DDSP paper uses a model with about 6M parameters, but I found that a tiny model with 7k parameters works just as well. This means that the DDSP architecture itself has a really strong _inductive bias_ that determines what the model can do.
- It matters a lot what framework you run the model in. ONNX Runtime and PyTorch's Torchscript were generally the best.
- Quantizing to 8 bits helps, although as I learned, this is not because of saved CPU cycles but rather reduced memory bandwidth.
- Pruning does nothing for speed in most libraries. [DeepSparse](https://github.com/neuralmagic/deepsparse/) is the exception, but the library was less mature than the alternatives (at the time that I wrote the thesis). It didn't support acceleration for the dilated convolutional layers that made up the bulk of the networks I was working with.
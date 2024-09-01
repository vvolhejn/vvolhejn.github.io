---
layout: single
title:  "Autoguitar"
date:   2024-07-01 00:00:00 +0100
categories:
header:
  teaser: /assets/images/autoguitar/thumbnail.jpg
---

A robotic stringed instrument: one motor tunes the string, and another one
plucks it. Fretless, since the pitch is controlled by tuning. Controllable via MIDI.

I decided to make this because I have plenty of experience with making software,
but hardware is completely unfamiliar territory for me: my first step in this
project was to google what voltage is.

Shoutouts to **Elio Fistarol** who helped me
massively with the hardware aspect and showed me a dozen of different machines
for cutting particular things in particular ways. And shoutouts to the [**ETH
Student Project House**](https://sph.ethz.ch/) whose Makerspace gave me access
to said machines.

**[Here](https://vm.tiktok.com/ZGevqMYG7/) is a demo of the guitar in action.**
At the moment, it's just one string, but you could extend this to several strings to be able to play chords.
Let's take a closer look at how it works:

{% include figure image_path="/assets/images/autoguitar/guitar-overview.jpg" caption="" %}

Or, in diagram form:

{% include figure image_path="/assets/images/autoguitar/guitar-diagram.png" caption="" %}

Here is what happens when you press a key on the MIDI controller:

1. The Raspberry Pi deceives a signal saying that a key was pressed.
2. It turns the strummer motor to pluck the string. In the demo video linked
  above, I'm using a tremolo mode where the guitar is automatically strummed all
  the time, but let's imagine that's off.
1. Simultaneously, it turns the tuner motor to tune the string to the note that you pressed on the MIDI controller.
  - It continually gets audio from the pickup and computes the current pitch of the string.
  - If the pitch is lower than it's supposed to be, it winds the string up to increase the pitch, and vice versa.

# Hardware

{% include figure image_path="/assets/images/autoguitar/tuner.jpg" caption="The tuner motor." %}

Both the tuner and the strummer use NEMA 17 Stepper motors.
The tuner motor is connected using a coupling to a repurposed [classical guitar tuning peg](https://www.musikhug.ch/de/tgi-gitarrenmechaniken-vernickelt-zubehoer-zu-klas-1817190.html)
on which the string is wound.

I also tried winding the string directly onto the stepper motor's axis, but then
the motor needs a much higher torque to wind the string.

{% include figure image_path="/assets/images/autoguitar/strummer.jpg" caption="The strummer motor." %}

The strummer motor is directly attached to a 3D-printed strummer that Elio
designed. The diamond-shaped pick is detachable from the holder so that it can
be easily exchanged. That proved to be handy because I've already broken a few
picks – the resin is not as tough as the regular picks' plastic.


# Fancy model-based tuning

The tuning strategy described above is a simple [proportional
controller](https://en.wikipedia.org/wiki/Proportional_control): in each
time-step, I turn the motor by a number of steps that's proportional to the
difference between the target and actual frequencies.

But this has disadvantages: pitch detection is computationally expensive for the
poor Raspberry Pi and you need to take a fairly large chunk of audio (~100 ms)
for the measurements to be stable. This leads to a slow feedback loop, meaning
delays and overshooting.

An alternative is to use a model-based approach: we know how strings work, so if
we're told "go from 150 Hz to 200 Hz", we could compute how much to turn the
tuner without

# Fancy model-based tuning



# What's next?

This is the state of the guitar from July 2024.
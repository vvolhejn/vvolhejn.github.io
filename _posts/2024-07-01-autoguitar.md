---
layout: single
title:  "Autoguitar"
date:   2024-07-01 00:00:00 +0100
categories:
header:
  teaser: /assets/images/autoguitar/thumbnail.jpg
katex: true
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

An alternative is to use a model-based approach: we know the physics of strings,
so if we receive an instruction like "go from 150 Hz to 200 Hz", we could
compute precisely how much to turn the motor to get to that frequency, without
having to use the feedback loop.

Here is a plot of the string's frequency as a function of how many steps the
motor has been turned, relative to an arbitrary zero-point.

{% include figure image_path="/assets/images/autoguitar/240512-steps-vs-hz-raw.png" caption="" %}

Looks like a simple linear relationship. This is what we get when we fit a
linear regression model to the data:

{% include figure image_path="/assets/images/autoguitar/240512-steps-vs-hz-linear.png" caption="" %}

Not bad, but you can see how the estimatated frequency is too low on the sides,
and too high in the center. And that's not due to noise: linear regression is
actually not the correct model!

That's because [according to
physics](https://en.wikipedia.org/wiki/Mersenne%27s_laws), the frequency of the
string is actually proportional to the _square root_ of the stretching force,
and our model assumes it's proportional to the force itself (=the number of
steps the motor was turned).

We can still model this using linear regression. Our original linear model is
$$f = as + b$$, where $$f$$ is the frequency, $$s$$ is the number of steps and
$$a$$ and $$b$$ are the parameters we fit to the data.

Now we want $$f$$ to be a function of $$\sqrt{s}$$, which makes the model
non-linear in $$s$$. But what we can do is instead take the square and predict
$$f^2$$ as a function of $$s$$. We can then simply take the square root of the
prediction to get $$f$$. This is the fit we get:

{% include figure image_path="/assets/images/autoguitar/240512-steps-vs-hz-correct.png" caption="" %}

Much better! We can now use this model for tuning. By inverting the model we get
a function from frequencies $$f$$ to motor positions $$\hat{s}(f)$$.
Essentially, this means "if you want to get frequency $$f$$, you should turn the
motor to $$\hat{s}(f)$$". So now when we press a key on the MIDI controller, we
can just look up $$\hat{s}(f)$$" and turn the motor to that rotation.

Notice that since there is no feedback loop any more, we no longer need the
pickup! We only need it at startup, because we don't know what frequency the
guitar started at – or, equivalently, what is the initial rotation of the motor.

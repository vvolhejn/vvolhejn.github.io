---
layout: single
title: "Game: Ant Colony Tycoon Deluxe"
date: 2024-10-10 00:00:00 +0100
categories:
header:
  teaser: /assets/images/ant-colony-tycoon-deluxe.jpg
katex: true
---

Control an ant colony by placing pheromone trails on the ground that ants follow. Made for Ludum Dare 56.

<img src="/assets/2024-10-10/gameplay.webp" alt="Ant Colony Tycoon Deluxe" style="width: 100%;"/>

**[Play it online on Itch.](https://allemansratten.itch.io/ant-colony-tycoon-deluxe)**

You can also check out [the game's Ludum Dare page](https://ldjam.com/events/ludum-dare/56/ant-colony-tycoon-deluxe),
or the [source code](https://github.com/allemansratten/AntColonyTycoonDeluxe).

When [we](https://github.com/allemansratten/) design games for game jams,
we always discuss whether we should prefer the game to be [experimental and innovative]({% post_url 2020-04-21-overly-fragile-relationships %}) or stick to tried-and-true genres and tropes.
The latter approach gave us [Tower Tower Defence Defence]({% post_url 2021-04-24-ttdd %}), our most successful game to date,
but I generally enjoy working on the weirder games more.

In that regard, I'm happy with how this game turned out. It strikes a good balance between being innovative and actually playable.
The pheromone mechanism is satisfying and the ants shuffling around do seem realistic, but it still draws on familiar mechanics
of collecting resources and infinite survival with increasing difficulty.

Initially, I was pushing for us to make a coding game inspired by cells and breaking symmetry in which you'd have to program
identical units to coordinate and fulfill different roles – like having the units choose a "leader", or form themselves into a particular shape.
We discarded that idea in the end because my teammates weren't excited about it and we thought the programming would be too complex;
in hindsight it was indeed the right decision.

That left us with the vague idea of "control an ant colony using pheromones".
On Saturday, we implemented the basic mechanics but we didn't really know how to turn the sandbox into an actual game.
Then on Saturday the idea crystallized into what we ended up with.
We borrowed the idea of the difficulty being controlled by zooming from [Mini Motorways](https://store.steampowered.com/app/1127500/Mini_Motorways/)
and later realized that there are a lot of parallels between the two games.
They're both about managing a transport network on which many small units move and perform tasks.

# The ant movement algorithm

The part I found technically most interesting was designing the ant movement algorithm.
Here's the high-level idea.

The movement is divided into steps. In each step, the ant picks a target point and moves
towards it. Once it reaches it, it picks a new target.

To pick a target, we first randomly pick a distance and then consider 36 possible targets
that are all at the same distance and their angles are evenly spaced (0°, 10°, 20°, ...).
For illustration, we'll pretend there are 8 possible targets instead:

<img src="/assets/2024-10-10/ant-algorithm-explanation-1.png"
  alt="Choosing a direction"
  style="width: 60%; display: block; margin-left: auto; margin-right: auto;"
/>

We assign a score to each of these targets and then randomly choose one, with
the probability being higher for targets with a higher score.
There are two components to the score: a turning penalty and a pheromone reward.

## Turning penalty

First, we penalize targets based on how different their angle is from the
direction the ant is currently facing. That way, the ant will tend to keep on
moving in the direction it's facing instead of doing a fully random walk.
We subtract $$\alpha \cdot \text{angle}$$ from each score, where $$\text{angle}$$
is the angular distance in radians between the angle the ant is currently facing
and the angle of the target. The $$\alpha$$ factor is a parameter we can modify
to make the turning penalty less or more important relative to the pheromone reward.

<img src="/assets/2024-10-10/ant-algorithm-explanation-2.png"
  alt="Turning penalty"
  style="width: 60%; display: block; margin-left: auto; margin-right: auto;"
/>

## Pheromone reward

To nudge the ants to follow the pheromone trails, we add to each score the pheromone
value at the target position.

<img src="/assets/2024-10-10/ant-algorithm-explanation-3.png"
  alt="Turning penalty"
  style="width: 60%; display: block; margin-left: auto; margin-right: auto;"
/>

Simple as that!
But there are two technicalities here: one, the way the pheromone is represented
internally doesn't fully match what the player sees.
The pheromones shown to the player are spatially smooth and their values are discretized to five possible values.
But under the hood, the grid is much coarser than pixel-level and the values can be any float from 0 to 1.
We apply a shader to make this raw data pretty and then show it to the player.

The second technicality is that we actually add $$\sqrt{\text{pheromone}}$$ to the score.
That way, lower values are emphasized and brought closer to the maximum value of 1.
We added this to fix an issue where the ants would get stuck in a high-pheromone
pool and had trouble getting out.
Because ants carrying food produce pheromone, they were stuck in a feedback loop
that kept refilling the pool.
This way, it's easier for ants to decide to go to a place even if its pheromone value
is not _as_ high.

## Putting it together

This way, we get a score for each target.
These scores can be negative so we cannot directly use them sampling weights.
To fix this, we use common trick from machine learning:
we apply [softmax](https://en.wikipedia.org/wiki/Softmax_function)
to convert the scores into probabilities.

An advantage of softmax is that we can use a _temperature_ parameter.
A high temperature levels out the probabilities, meaning the ants are more likely to choose targets with lower scores.
With a temperature of 0, they would always choose the target with the highest score.

By varying the temperature and the turning penalty $$\alpha$$,
we get good control of the behavior of the ants and we can easily change it to balance the game.
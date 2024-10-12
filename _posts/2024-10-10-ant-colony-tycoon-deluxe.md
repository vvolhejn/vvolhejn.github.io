---
layout: single
title:  "Game: Ant Colony Tycoon Deluxe"
date:   2024-10-10 00:00:00 +0100
categories:
header:
  teaser: /assets/images/ant-colony-tycoon-deluxe.jpg
---

Control an ant colony by placing pheromone trails on the ground that ants follow. Made for Ludum Dare 56.

<img src="/assets/2024-10-10/gameplay.webp" alt="Ant Colony Tycoon Deluxe" style="width: 100%;"/>

Play it [here](https://allemansratten.itch.io/ant-colony-tycoon-deluxe), the source code is [here](https://github.com/allemansratten/AntColonyTycoonDeluxe).

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

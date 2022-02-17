---
layout: single
title:  "Game: The Skewery"
date:   2019-10-08 00:00:00 +0100
categories:
header:
  teaser: /assets/images/the-skewery-thumbnail.png
---

<figure>
	<a href="https://allemansratten.github.io/the-skewery/dist/index.html"><img src="/assets/images/the-skewery-thumbnail.png"></a>
	<!-- <figcaption></figcaption> -->
</figure>

**Play it [here](https://allemansratten.github.io/the-skewery/dist/index.html).**

Begin your skewer journey and satisfy every customer’s specific request.
A puzzle game about learning to create the best skewers.

We made it for [Ludum Dare 45](https://ldjam.com/events/ludum-dare/45),
whose theme was "You start with nothing".
As you can to tell, we only followed the theme _very_ loosely.

[Here's](https://ldjam.com/events/ludum-dare/45/the-skewery) the game's LD page.
I'm happy with how it turned out, I think the scope was modest enough for us to be able
to make it a short but fun game.

We came up with the mechanics before we coded and tried anything, and we designed a few puzzles
on paper that seemed interesting.
Only later did we realize that we "got lucky" when we tried it out on paper and later on had trouble
creating more interesting mechanics within the game's constraints.
The game had a high surface-to-volume ratio, so to speak: for the puzzle to be non-trivial, we needed
to write long and convoluted rules so it was a challenge for the player to even understand
what the game wants from them. Lesson learned!

Later someone found a game with a similar concept but the lore was that you are trying to
create a strong password that conforms to absurd requirements. Why didn't we think of that??
I think it was [this one](https://www.theverge.com/c/22526949/normal-password-system-generator-game),
but I'm not sure.

For encoding the rules of the skewers, we used a (perhaps unnecessarily) expressive regex-based system,
see [source code](https://github.com/allemansratten/the-skewery/blob/master/src/misc/levels.ts).

Fun fact: the [music](https://www.youtube.com/watch?v=4vItK3-Shyw) is just the first YouTube search result for "kebab music".

## Credits

- Alžběta Volhejnová - graphics
- Jan Petr - level design
- Jiří Balhar - programming, sound, level design
- Václav Volhejn - programming, level design
- Vilém Zouhar - programming, level design, graphics

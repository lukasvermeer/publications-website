---
layout: post
title: "Snakes on a Two-dimensional Plane"
date: 2010-12-17
tags: ["Artificial Intelligence","Javascript","Oracle","RTD"]
original:
  source_name: Wordpress
  source_url: https://lukasvermeer.wordpress.com/2010/12/17/snakes-on-a-two-dimensional-plane/
---

<span style="color:#bbb;">[This post is based around a very old pet project of mine. I thought it worthwhile to describe this golden oldie some more detail and relate it to my current work.]</span>

Do you remember those old [Nokia 3210](http://en.wikipedia.org/wiki/Nokia_3210) phones? Remember what an awesome game [Snake](http://en.wikipedia.org/wiki/Snake_(video_game)#Snake_on_Nokia_phones) used to be?

**Ah, good times!**

Back then, I was a freshman at [Utrecht University](http://www.uu.nl/), where I studied [Computing Science](http://www.cs.uu.nl/) with a minor in [Artificial Intelligence](http://www.uu.nl/university/minors/NL/technischeki/Pages/default.aspx). I must admit that I arguably wasn't a very good student in those days. I was far to young, naively enthusiastic, impulsively inquisitive and stubborn to take any wisdom being presented by teachers at face value. I would call anything, everything and anyone into question. I must have missed many opportunities to stop, listen and learn from those around me.

I've grown older, bolder and balder, and perhaps even a little wiser. And some of the traits that made me a difficult student are the ones I value most in my work as consultant. Listening still requires some effort, but one is never to old to learn.

One recurring thesis posed in those first semesters seemed to be that Artificial Intelligence is immensely difficult to achieve. More than once, it was stressed in class that a lot of expert knowledge and hard work (and also hardware) was required to make a piece of software seem even remotely intelligent.

I didn't agree.

I thought, and still think, that 'intelligence' is something that is always defined and evaluated within a domain of possibilities. While it may be extremely difficult to create a piece of software that makes decisions perceived to be intelligent in a large domain, this can be almost trivial if the domain is sufficiently small.

[caption id="attachment_442" align="alignright" width="195" caption="Three Snakes Converging on the Target"][![Detail of a screenshot of the Snake Project](http://lukasvermeer.files.wordpress.com/2010/12/snakes-detail.png "Snakes")]({{site.baseurl}}{% link assets/2010-12-17-snakes-on-a-two-dimensional-plane-snakes-detail.png %})[/caption]

**Small, like the realm of a game of Snake.**

In Snake, the world consists of a two-dimensional plane divided into a limited number of squares (or pixels). Some of these are occupied by the snake itself and only one is occupied by the target (or food pellet). At any given point in the game, a player has exactly four possible actions. Should he go op, down, left or right? When the snake steps on the target he grows in length and the target moves to a new location on the plane. When a snake steps off the plane (hits a wall) or on itself the game is over. Simple.

So, to prove my point (mostly to myself), I built [a small website that 'learns' to play snake](http://www.xs4all.nl/~destack/projects/snake/). Starting with a random set of priorities, the group of twenty snakes evolves (using a [Genetic Algorithm](http://en.wikipedia.org/wiki/Genetic_algorithm)) to the point where they can be seen to actively compete over the target. Their success seems only limited by the fact that there is only one such target and the snakes will starve when they do not capture it often enough. Also, as snakes become longer, pixels will be at a premium and the risk of collisions will increase.

Because the snakes learn from a random set of priorities, I do not know what the optimal priorities are (if such an optimum even exist). I also have no idea what decisions each snake is making every step during the game.

And I couldn't care less. I care about the end result.

Of all the people I have shown this to over the years, not many have voiced any doubts whether these snakes are 'intelligent' or not. Most people seem convinced that learning is happening and that the snakes play with sufficient skill; they conclude that some form of intelligence must be involved.

**Lesson learned.**

In my work as a consultant for [Oracle Real-Time Decisions](http://www.oracle.com/technetwork/middleware/real-time-decisions/overview/index.html) (RTD) my experience with Snake proves to be a very valuable lesson. One of the key features of RTD is its ability to learn to predict consumer behavior. The use of relatively simple (when compared to other data mining techniques) Predictive Analytics methods in this confined domain enables intelligent personalization that is both technically and functionally scalable and relatively easy to implement. The process is in some ways quite similar to the way the snakes learn to play. It is the result that counts, not the complexity of the solution.

Implementing intelligence like this is not difficult, but does require that you relinquish some control and focus on what is really important.

**Intelligence requires autonomy.**

* * *

**Update (april 2nd 2011):** Code for this project is now available on [Github](https://github.com/lukasvermeer/snake).
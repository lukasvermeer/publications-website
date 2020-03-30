---
layout: post
title: "Connect Four and Minimax"
date: 2011-09-24
tags: ["Artificial Intelligence","Javascript"]
original:
  source_name: Wordpress
  source_url: https://lukasvermeer.wordpress.com/2011/09/24/connect-four-and-minimax/
---

Computers are incredibly fast calculators. That makes them good at maths, but it does not make them smart. People that program computers have to invent clever ways to exploit that arithmetical ability to achieve intelligent behavior.

**Using the force.**

Often the simplest way to allow a computer to make the 'right' decision is to calculate all possible outcomes and select a path that leads to the best end result. When planning a route from A to B, we consider every possible sequence of turns for every intersection between A and B. This so-called [brute-force](http://en.wikipedia.org/wiki/Brute-force_search) approach does not require a lot of thinking, it requires a lot of calculating; and computers are pretty good at the latter, less so at the former.

Sometimes it is possible to trim certain paths of exploration because we are certain they will lead to sub-optimal results. For example, when a sequence of turns leads us back to an intersection we have visited before along this route. But even then, this method quickly results in long computation times as the number of possible outcomes grows exponentially with the number of possible decisions the computer has to consider.

This is one of the reasons why we need a [super-computer](http://en.wikipedia.org/wiki/Deep_Blue_(chess_computer)) to beat a [chess grandmaster](http://en.wikipedia.org/wiki/Garry_Kasparov). There are a lot of possible moves to consider in a game of chess.

**[Winning](http://knowyourmeme.com/memes/winning).**

Another problem when programming computers to play a two-player [zero-sum game](http://en.wikipedia.org/wiki/Zero%E2%80%93sum_game) like chess, is that half of the decisions in the game are made by the opponent. The computer can decide which moves to make, but it has no control over what the opposing player does in response.

Even worse, assuming they both want to win, the two players have completely diametric goals. When plotting a potential path through solution space, we have to consider that our opponent probably wants to move towards a different solution altogether. The other player is actively trying to stop us from getting to where we're going.

**[Mini and Maxi](http://www.youtube.com/watch?v=ePxdZ_qIQ4g).**

One easy way to deal with the uncertainty of the opponent's decisions when looking ahead is to assume the other player will always make the move that leads to the worst possible result for the computer. The opponent is not trying to win, he is trying to stop the computer from winning. When plotting a path (succession of possible moves) we alternate between making the best (maximum) move when it's our turn and the worst (minimum) when the opponent is up to make a move.

[caption id="attachment_659" align="alignright" width="309"][![A game of connect four]({{site.baseurl}}{% link assets/2011-09-24-connect-four-and-minimax-connectfour.jpg %} "Connect Four")](http://destack.home.xs4all.nl/projects/minimax/) Connect Four[/caption]

This approach is called [minimax](http://en.wikipedia.org/wiki/Minimax). It is not particularly clever, but it does the trick and is relatively easy to implement.

**Connect Four.**

Last year, a [Microsoft SharePoint](http://en.wikipedia.org/wiki/Microsoft_SharePoint) consultant I'd met at a customer site asked me to explain how to implement a computer opponent for a [Connect Four](http://en.wikipedia.org/wiki/Connect_Four) game he was creating for [Microsoft Windows Phone Mobile XP 7 Vista Home Edition](http://en.wikipedia.org/wiki/Windows_Phone) (or whatever they call it nowadays). Instead of explaining how, I decided to build [a simple example](http://destack.home.xs4all.nl/projects/minimax/) myself in JavaScript using minimax. I don't think he ever finished his version of the game.

You can play my version [here](http://destack.home.xs4all.nl/projects/minimax/). Somewhat ironically, it is excruciatingly slow when using [Microsoft Internet Explorer](http://en.wikipedia.org/wiki/Internet_Explorer); so please consider using [any](http://www.apple.com/safari/download/) [other](http://www.mozilla.org/en-US/firefox/new/) [browser](http://www.google.com/chrome?hl=en).

* * *

**Update (June 29th 2012):** The code for this example is available [on Github](https://github.com/lukasvermeer/minimax).

**Update (July 5th 2012):** This is now available as a web service. Read [this post](http://lukasvermeer.wordpress.com/2012/07/05/connecting-four-in-the-cloud/).
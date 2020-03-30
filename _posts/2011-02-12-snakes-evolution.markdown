---
layout: post
title: "Snakes: Evolution"
date: 2011-02-12
tags: ["Artificial Intelligence","Javascript","Standard","Psychology"]
original:
  source_name: Wordpress
  source_url: https://lukasvermeer.wordpress.com/2011/02/12/snakes-evolution/
---

After [one of my previous posts](http://lukasvermeer.wordpress.com/2010/12/17/snakes-on-a-two-dimensional-plane/) about the [snake project](http://www.xs4all.nl/~destack/projects/snake/) Casper [had some questions](http://www.facebook.com/lukas.vermeer/posts/1768491377794).
> Are they retaining their knowledge beyond death? Sometimes it doesn't seem so. Also, you say they learn to keep themselves alive and how to get to the pellet, is the need to feed programmed (say, as an instinct) or are they learning that feeding = more length? :)
These are good questions, and I understand the confusion. But it seems to me that Casper is looking at this the wrong way around. The snakes as a group can be said to learn to play the game, but individual snakes do not change their behavior during their lifetime. Individuals do not learn. Snakes die and new ones appear, but these are really new snakes; mutations and clones of the previous generation, not the same specimens.

The questions are understandable, but in light of the technique used they do not make any sense. Allow me to try and explain.

**Evolution, baby!**

[Bobo the poodle](http://www.mypoodleclub.com/owner.php) did not learn how to look as silly as he (or she) does. His appearance is the result of generations of selective breeding. Likewise, the snakes do not learn to play the game; the program selects and breeds those that have more potential.

The type of algorithm I've used for this project is called a [Genetic Algorithm](http://en.wikipedia.org/wiki/Genetic_algorithm). This technique is based on the [Darwinian idea of evolution](http://en.wikipedia.org/wiki/Evolution). A set of digital genes determine the behavior of each snake, its length at death defines its fitness and the next generation is based on the previous gene pool. Snakes that were long at death get more descendants. There is no knowledge to retain (e.g. everything depends on genes that are set from birth) and death is permanent (e.g. each snake lives only once).

Because length defines fitness the population will evolve towards snakes that eat more. There is no 'need to feed' or instinct pre-programmed (just as, presumably, poodles do not 'want' to look as daft as they do). The direction in which the food is located is simply one of the inputs to the decision each snake has to make. I'm not telling them how important that input is. Evolution takes care of that.

**It's all in the genes.**

Each snake has a digital set of genes each represented by a number between minus and plus one hundred.

[sourcecode language="javascript"]
basic_need	= genes[1]; // starting score for each direction.
wall_need	= genes[2]; // added when the snake is adjacent to a wall.
snake_need	= genes[3]; // added when the snake is adjacent to another snake.
bitex_need	= genes[4]; // added when the pellet is up.
bitex2_need	= genes[5]; // added when the pellet is down.
bitey_need	= genes[6]; // added when the pellet is left.
bitey2_need	= genes[7]; // added when the pellet is right.
last_need	= genes[8]; // added when the snake moved this direction in previous step.
[/sourcecode]

These values are combined with limited boolean knowledge of the current environment to assign a score to each direction (up, down, left and right). Each step, the snake will move in the direction with the highest score (above zero that is, the snakes will commit [sepuku](http://nl.wikipedia.org/wiki/Seppuku) if all scores are negative).

Initially these values are assigned at random. This is why initially most snakes seem to wobble around without purpose until they starve from hunger, while others bang head-first into walls and some even disappear right away (this last group of 'pessimists' conclude life is pointless from the get-go and give up at their first move).

But eventually (by accident or chance or fate; or whatever you want to call it) a snake will get the pellet. This first feeder might not make it very far after this initial triumph, but the seeds of succes are sown. Now all the algorithm (or evolution) needs to do is nurture it to its full potential.

**The dating game.**

To ensure that the population is somewhat stable (mostly for esthetic reasons), the algorithm will introduce four new snakes as soon as four old ones have ended their game. To keep things simple I used a deterministic version of [tournament selection](http://en.wikipedia.org/wiki/Tournament_selection) and implemented only [mutation](http://en.wikipedia.org/wiki/Mutation_(genetic_algorithm)) without [crossover](http://en.wikipedia.org/wiki/Crossover_(genetic_algorithm)).

[sourcecode language="javascript"]
function compare_and_breed(p) { // This code has been slightly altered for clarity.
	score = new Array;
	genes = new Array;
	var best = -1;
	var bestman = null;

	for (z=0;p[z]!=null;z++) {
		score[z]=getScore(p[z]);
		genes[z]=cloneGenes(p[z]);
		if (score[z]>=best) {
			best = score[z];
			bestman = z;
		}
	}

	AddSnake(genes[bestman]);
	AddSnake(mutateGenes(p[bestman]));
	AddSnake(mutateGenes(p[bestman]));
	AddSnake(mutateGenes(p[bestman]));
}
[/sourcecode]

Of the four old snakes, the one who is deemed the most fit (the longest) will be selected for procreation. The next generation will consist of one clone of the champion and three mutations. In the mutated snakes, each gene value has a 1 in 8 chance of being raised or lowered slightly and the same odds of being completely randomized.

[sourcecode language="javascript"]
function mutateGenes(p) { // This code has been slightly altered for clarity.
	oldgenes = historyL[p]
	newgenes = new Array();

	for (i=1; i<=8; i++) {
		if (mutationInhibitor()) {newgenes[i] = mutateGene(oldgenes[i]);}
		else if (unstableMutationInhibitor()) {newgenes[i] = randomGene();}
		else {newgenes[i] = oldgenes[i];}
	}

	return newgenes;
}

function mutationInhibitor()			{ return (Math.round(Math.random()*8) == 1); }
function unstableMutationInhibitor()	{ return (Math.round(Math.random()*8) == 1); }
function mutateGene(x)					{ return x + ((Math.round(Math.random()*10)/2) - 5); }
function randomGene()					{ return (Math.round(Math.random()*200)/2-50); }
[/sourcecode]

And that is really all there is too it. The rest is just repetition of tweaking and testing; recombining and re-evaluating; mutation and measuring fitness.

**Pay attention to that [man behind the curtain](http://www.imdb.com/title/tt0032138/quotes?qt0409920).**

[In my previous post about this project](http://lukasvermeer.wordpress.com/2010/12/17/snakes-on-a-two-dimensional-plane/) I had the audacity of calling these snakes intelligent. Judging by the questions Casper and others raised they are certainly perceived that way. Now that you (hopefully) understand how this magic works, I wonder if you would still consider what you see to be intelligence. Perhaps lifting the curtain of uncertainty has broken the spell; revealing it to be a cheap trick, nothing but smoke and mirrors.

And if you think that the latter is the case, what does this imply for your _own_ intellect?

**You might not want to _think_ about that for too long, because you might not like the answer.**

<span style="color:#bbbbbb;">[I am deeply sorry if I've offended any poodle fanatics with the examples used in this article. You have to admit, they do look rather silly. If evolution were ever to blame for causing something awful it would be those poor poodles.]</span>

* * *

**Update (april 2nd 2011):** Code for this project is now available on [Github](https://github.com/lukasvermeer/snake).
---
layout: post
title: "Estimating Pi"
date: 2010-05-06
tags: ["Code","Mathematics","Perl","Pseudo","Statistics"]
original:
  source_name: Wordpress
  source_url: https://lukasvermeer.wordpress.com/2010/05/06/estimating-pi/
---

<span style="color:#bbb;">[I've [tweeted ](http://twitter.com/lukasvermeer/status/9197883776)about [this](http://twitter.com/lukasvermeer/status/9197886895) before, but I feel the need to explain.]</span>

I want to share with you my favorite algorithm. It may not be pretty, efficient or even practical, but I love it for it's misleading simplicity and hidden complexity. It is a [Probabilistic (Randomized) Algorithm](http://en.wikipedia.org/wiki/Randomized_algorithm) that combines [statistics](http://en.wikipedia.org/wiki/Statistics) and [Euclidean geometry](http://en.wikipedia.org/wiki/Euclidean_geometry) by using the [Pythagorean theorem](http://en.wikipedia.org/wiki/Pythagoras#Pythagorean_theorem) and the [Law of Large Numbers](http://en.wikipedia.org/wiki/Law_of_large_numbers) to estimate [Pi](http://en.wikipedia.org/wiki/Pi). I first learned about this algorithm when I was at [Utrecht University](http://www.uu.nl/) studying [Computing Science](http://www.cs.uu.nl/).

Don't worry, this is not some complicated computer-gizmo-thing that only super-nerds can translate. I'll take you through it nice and slow. You will need to recall some basic arithmetic you learned in high-school.

To _estimate _Pi, we rely on the following truths.

1.  The surface area of a circle is equal to πr² (Pi times the radius of the circle squared, definition of Pi).
2.  We can make a square in which the circle fits with sides that are of length 2r (easy, just try it).
3.  The surface of that square (that just encompasses the circle) is (2r)² (the width of the square squared).
4.  The ratio between the surface of the circle and the surface of the square is πr² / (2r)²; which equals π / 4.
5.  This ratio reflects the percentage of the square that is occupied by the circle (something close to 78,5%).
6.  The odds that a random point in the square is also in the circle is equal to the ratio of the two surface areas (statistics).
7.  Any point in the square is also in the circle when it is less than r distant from the center of the circle.
8.  The distance between two points squared is a² + b² (Pythagorean theorem).
9.  If you select enough random points in the square, the ratio between those points that are also in the circle and those that are not in the circle will approximate the ratio between the two surface areas (law of large numbers).

![Estimating Pi]({{site.baseurl}}{% link assets/2010-05-06-estimating-pi-pi1.png %} "Estimating Pi")

Based on all this, all we have to do is the following.

1.  Pick a a bunch of random points in the square (say N number of points where N is a _pretty big_ number).
2.  Determine how many of those points are in the circle using the Pythagorean theorem (say C of them are in the circle).
3.  Calculate the ratio between C and N and multiply by 4.
4.  Presto! You have now estimated Pi.
Written down in Perl and compressed to fit into a [single tweet](http://twitter.com/lukasvermeer/status/9197883776) this [1] can be expressed as:

`perl -e 'for(;$i<999999;++$i){rand()**2+rand()**2>1?0:++$c}print 4*$c/$i." is (probably) pretty damn close to Pi!\n"'`

Which probably looks daunting, but does nothing more than what I've just described.

*   `for (;$i<999999:++$i)` just means "do the following 999999 times".
*   `rand()` is a random number between 0 and 1.
*   `N**2` is `N` squared.
*   `something ? this : that` translates to "if something is true, do this, else, do that".
*   `0` does nothing.
*   `++$c` adds one to the variable `C`.
*   `print` does exactly what you expect.
*   `\n` prints a new line (solid return).
When executed, the Perl script outputs something like the following.

`3.14152714152714 is (probably) pretty damn close to Pi!`

Which is obviously wrong, but still pretty close to the actual number. Also note, that since we select random points the estimated number will be different every time we execute the code. To increase the accuracy of the number, simply use more random points. As the professor who explained this to an astonished class commented: 

> For maximum accuracy, you should simply select enough points so that the odds of a technical computational error are higher then the odds of an estimation error.

Isn't computing science cool!?

[1] As my friend Daan [pointed out before](http://twitter.com/daanbroekhuizen/status/9218973798) my original tweet was not quite optimal; hence the different code in this post.
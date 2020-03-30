---
layout: post
title: "Solving Some Sudoku Puzzles Faster (I Stand Corrected)"
date: 2011-02-05
tags: ["Mathematics"]
original:
  source_name: Wordpress
  source_url: https://lukasvermeer.wordpress.com/2011/02/05/solving-some-sudoku-puzzles-faster-i-stand-corrected/
---

Okay. So maybe I was jumping to conclusions a little early in the game.

After I [posted my own modified version](http://lukasvermeer.wordpress.com/2011/01/29/solving-every-sudoku-puzzle-faster/) of [Peter Norvig](http://norvig.com/)'s [Sudoku Solver](http://norvig.com/sudoku.html), Peter and I had some brief discussion about my results and conclusions. I had only compared the performance of the two algorithms based on the [ninety-five difficult puzzles](http://www.xs4all.nl/~destack/projects/sudo.py/top95.txt) provided by Peter, but he was not convinced. So I set out to make a more rigorous analysis of the relative strengths of our approaches using the method for generating random puzzles included in the original code. The results were a little unexpected, but not very surprising.

**Fair game.**

To make this a fair competition, I made some slight modifications. 

Firstly, I had to ensure that both algorithms were solving the exact same random puzzles. Otherwise one algorithm might, simply by virtue of some serious bad luck, end up with more difficult puzzles. This is especially important since Peter's experiments showed that long execution times (difficult puzzles) are rare but have an extreme impact; a few puzzles taking more than a minute to solve. 

Secondly, I modified the code so that it uses the [timeit](http://docs.python.org/library/timeit.html) module for measuring execution times. This allowed me to measure execution times more precisely.

**More pudding.**

Overnight, one million random puzzles were attempted by the two competing algorithms (999.925 were actually solved). Peter's original algorithm required an average of 0.007 seconds per puzzle (142 puzzles per second) with a maximum execution time of 91.06 seconds. For the same puzzles, my modified version required an average of 0.008 seconds per puzzle (124 puzzles per second) with a maximum execution time of merely 5.75 seconds. 

Both these averages are far lower than those for the ninety-five difficult puzzles attempted earlier, but the maxima are orders of magnitude higher. This seems to indicate that most of these random puzzles are easier to solve; save for a few extreme puzzles. This is in line with Peter's earlier findings.

The original algorithm solves these random puzzles about fourteen percent quicker on average, but the distribution of execution times is more spread out; as is evident from the much larger standard deviation and higher maximum.

**Mea culpa. But not for maxima.**

Based on these findings I have to admit I was wrong. My modified algorithm does not solve _every_ Sudoku puzzle faster, as the title of my previous post implied. When solving many, randomly generated puzzles the original is slightly faster on average. 

But when the task at hand involves more difficult Sudoku puzzles, or when you are operating under strict time constraints (i.e. you want more certainty about the maximum time required to solve a single puzzle), my money is still on the modified version. The exact puzzle that required the original algorithm 91.06 seconds to solve took the modified version a mere 0.0079870224 seconds. I'd say that is an improvement.

**There's a tool for every job.**

When we were discussing how Peter had generated the graphs he used in his article, I remarked (tongue-in-cheek) that copying and pasting execution times into a spreadsheet sounded a lot like brute-force manual labour to me. I think his response summarizes this whole exercise for me quite well.
> Yes, but sometimes a little brute force is easier than figuring out how to do an elegant solution.
It seems to me now that sometimes a little brute force is not only easier, but that it can be faster than a more elegant solution as well. Many thanks to Peter for posing questions and pushing me to dig a little deeper.

**Sir, I stand corrected!**
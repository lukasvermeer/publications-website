---
layout: post
title: "the Same Old Song"
date: 2010-07-16
tags: ["Datamining"]
original:
  source_name: Wordpress
  source_url: https://lukasvermeer.wordpress.com/2010/07/16/the-same-old-song/
---

<span style="color:#bbb;">[I've [tweeted ](http://twitter.com/lukasvermeer/status/6962168925)[about ](http://twitter.com/lukasvermeer/status/7029674362)[this ](http://twitter.com/lukasvermeer/status/7029711378)[before](http://twitter.com/lukasvermeer/status/7029745604).]</span>

A few months ago my friend and neighbor [Olav](http://laudy.net/) was fiddling around with a dataset of movie plot descriptions he downloaded from the [Internet Movie Database](http://www.imdb.com/) (IMDb). If I recall correctly, he was taking a stab at the [Netflix Prize](http://www.netflixprize.com/). We discussed this for a while over coffee, but (as usual) our conversations were all over the place; and somewhere along the line we wondered what songs are used most often in movies.

[caption id="" align="alignright" width="240" caption="Play!"][![Play]({{site.baseurl}}{% link assets/2010-07-16-the-same-old-song-48997287_678eb48f77_m.jpg %} "Play")](http://www.flickr.com/photos/lukasvermeer/48997287)[/caption]

**What is that song they always play? The one that goes like '#****_dun dun dun dun dudun dun dun duuuuun#_'. You know?**

The IMDb site offers [lots of different datasets for download](http://www.imdb.com/interfaces#plain), and we quickly found that one of them contains soundtrack listings (the aptly named file [soundtracks.list.gz](ftp://ftp.fu-berlin.de/pub/misc/movies/database/soundtracks.list.gz)). Now it was just a matter of filtering out the unnecessary contextual data and counting songs. Quickly Olav, who does datamining for a living, managed to get all this done using spiffy point-and-click tools. I proceeded to [ask twitter what people thought the answer would be](http://twitter.com/lukasvermeer/status/6962168925).

The top five results turned out to be a collection of classics. The songs played in movies (according to the IMDb data) is as follows.

1.  ["Jingle Bells"](http://en.wikipedia.org/wiki/Jingle_Bells) (220x)
2.  ["William Tell Overture"](http://en.wikipedia.org/wiki/William_Tell_Overture) (204x)
3.  ["Home Sweet Home"](http://en.wikipedia.org/wiki/Home!_Sweet_Home!) (160x)
4.  ["Auld Lang Syne"](http://en.wikipedia.org/wiki/Auld_Lang_Syne) (149x)
5.  ["Rock-a-Bye Baby"](http://en.wikipedia.org/wiki/Rock-a-bye_Baby) (140x)
Not at all what we were expecting, but quite obvious when you think about how many Christmas movies are out there. Data mining is very often like that. You find answers that were unexpected, but also unsurprisingly obvious.

**It's the same song, but it never gets old.**

<span style="color:#bbb;">[Much later, a friend (can't remember exactly who) noted that the song that is played most often in theaters is probably not listed in the data set the IMDb provides. It's the [20th Century Fox intro](http://www.youtube.com/watch?v=LTgRm6Qgscc).]</span>
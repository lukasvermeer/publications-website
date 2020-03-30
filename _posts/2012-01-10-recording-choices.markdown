---
layout: post
title: "Recording Events"
date: 2012-01-10
tags: ["choices","Code","implementation","Java","Oracle","performance","recording","RTD","rtd"]
original:
  source_name: Wordpress
  source_url: https://lukasvermeer.wordpress.com/2012/01/10/recording-choices/
---

<span style="color:#bbb;">[ [Crossposting](http://en.wikipedia.org/wiki/Crossposting) here [from](http://blogs.oracle.com/rtd/en/entry/recording_choices) the [Oracle Real-Time Decisions Blog ](http://blogs.oracle.com/rtd/)where I have been [invited](http://blogs.oracle.com/rtd/en/entry/introducing_lukas_vermeer) to contribute. ]</span>

There is always more than one way to skin a cat; it's just that some ways to excoriate a feline are more efficient than others. This is certainly true for the way in which we can record events against choices in RTD, and the differences in performance can be striking.

**getChoice**

A common approach to record events against static choices is to use the getChoice API.

[sourcecode language="java"]
Choice ch = MyChoiceGroup.getChoice("MyChoiceId");
ch.recordEvent("Clicked");
[/sourcecode]

On the first line, we are asking RTD to go through the list of all choices in MyChoiceGroup and retrieve one particular choice keyed MyChoiceId. On the second, we record a Clicked event against the returned choice.

Because we are asking RTD to go through the list of all choices, we require that RTD has a full list of choices available; even if we only end up using a single choice. If we use the same code for dynamic choices, RTD will thus have to fetch and instantiate the full list of dynamic choices in order to find the single one we are interested in. This may be an expensive operation, as it may require accessing an external database or web service; execution of complex custom code; and/or instantiating a large number of dynamic choices.

<span style="color:#bbb;">[ Note that dynamic choices do not cache at the choice level. They cache at the entity level. These entities hold the data for the dynamic choices but not the choices themselves. The RTD API could perhaps be optimized differently, but the current implementation will instantiate all the dynamic choices and then find the one the API call is looking for. Consider also that the actual source data used for the dynamic choices could come from anywhere and need not come from cached entities at all; it might even be generated on the fly through custom functions. This flexibility in sourcing dynamic choices makes retrieving a single choice non-trivial from the RTD API perspective. ]</span>

This approach will certainly work, but may waste precious time and resources retrieving and instantiating dynamic choices that are never used.

**getPrototype**

For recording an event, it is sufficient that we have a choice that has the desired SDOId; all other choice attributes are irrelevant for this purpose. A more efficient way to record events against dynamic choices is therefore to create a new empty dynamic choice; assign it an SDOId; and record the event against that, rather than retrieving the 'actual' dynamic choice. The result in terms of statistics and learning are the same, but in this approach there is no need for RTD to retrieve any choices at all.

We can instantiate an empty dynamic choice using the getPrototype API.

[sourcecode language="java"]
Choice ch = new MyDynamicGroupChoice(MyDynamicGroup.getPrototype());
ch.setSDOId("MyDynamicGroup" + "$" + "MyChoiceId");
ch.recordEvent("Clicked");
[/sourcecode]

This approach can provide significant performance improvements in implementations with a large number of dynamic choices; or in implementations where retrieving dynamic choices is a non-trivial complex operation and entity caching proves insufficient.

Being able to record events on choices that are not retrievable through the dynamic list is an additional advantage that has several interesting applications which we will explore in future posts.

<span style="color:#bbb;">[ Special thanks to Michel Adar for bringing this to my attention and providing an initial draft of this article. ]</span>
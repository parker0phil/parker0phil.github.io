---
layout: post
title: Architecture 101 + Domain Alignment
---
> This is the outline of a "Lighting talk of two parts" delivered at [Agile on the Beach 2014 (#agileotb)](http://agileonthebeach.com/2014-2/)

> UPDATE: [Video is now online](http://echo.falmouth.ac.uk:8080/ess/echo/presentation/76f54723-8e47-4739-83e2-b091930bb4da)
> This talk starts at 23:30 (but please watch them all!)

# Part 1 -  Agile 101

Everyone knows that architecture is just "box drawing" so today I'll just talk about boxes (forget contexts, components, containers, modules, classes, packages etc).
This lightning talk is intended to help explore some of the terms used and (controversially!) suggest 5 rules...

## Abstraction

This is a term I heard used in maybe 4 out of 5 of the talks I attended today.

Abstraction has two (relatively subtle) different meanings:

### (first abstraction subtlety) Elevation == different 'levels of abstraction'

* At what leve are you looking at a problem from? (a) "a 1000 miles high" vs (b) "in the trenches"
* An example is "What are you doing ...?". "What are you doing *now*" vs "What are you doing *next year*". You pray for a different level of granularity in the answer.
* [Uncle Bob says Don't mix different levels of abstraction in code](http://java.dzone.com/articles/clean-code-dont-mix-different)

> RULE 1: *LEVELS* When communicating about Architecture, converse at a single level of abstraction (elevation), and ergo, an appropriate level of granularity.

### (second abstraction subtlety) Pattern Identification == something "non-concrete"

* This is the Picasso flavour of abstract.

> RULE 2: *EXAMPLES* Abstract by example, not speculation.

* e.g. (a) Requirement for 'a messaging system' (b) we need a system to manage "Groups"
* In code why have an *iWhatsit* and a *WhatsitImpl*?

## Encapsulation

• (Not one for the Snapchat generation) but we used to say in Java, don't expose your *privates*.

> RULE 3:: *BOUNDARY* Understand your component boundaries (and therefore contracts) at every "level of elevation".

* Manage your coupling. Defend your right to react/adapt to change. Maintain options.
* [good Component Teams are Responsible Providers and Sceptical Consumers](https://twitter.com/parker0phil/status/507571838709694464)

All of the above (in box terms) defines how to build your boxes but misses an important question....

# Part 2 - What's in the box?

The easiest way to look at this is Rule 2. *Q.* What sort of examples? *A.* Business examples.

So when determining what's in the box think, verticles, verticles, verticles.

A typical architecture diagram shows technological concepts in horizontal layers (we're really good at this... think front-end and back-end teams or gui, service and dao packages...) and (sometimes) business concepts in vertical "slices".

* Single Responsibility Principle (SRP) -  "Gather together those things that change for the same reason, and separate those things that change for different reasons"
* Conway's Law - "organizations which design systems ... are constrained to produce designs which are copies of the communication structures of these organizations"

> RULE 4: ALIGNMENT ??but to what??

I said I'd deliver 5 rules to let's "right size" RULE 4...

> RULE 4 (a): ALIGN your Architecture (portfolio/components) with your Delivery (people)

AND MOST IMPORTANTLY!!!

> RULE 4 (b): ALIGN both with your Domain (the reasons why your business changes)

So let's summarise!

#### RULE 1: *LEVELS* When communicating about Architecture, converse at a single level of abstraction (elevation), and ergo, an appropriate level of granularity.

#### RULE 2: *EXAMPLES* Abstract by example, not speculation.

#### RULE 3:: *BOUNDARY* Understand your component boundaries (and therefore contracts) at every "level of elevation".

#### RULE 4 (a): *ALIGN* your Architecture (portfolio/components) with your Delivery (people)

#### RULE 4 (b): *ALIGN* both with your Domain (the reasons why your business changes)




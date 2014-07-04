---
layout: post
title: GaP Stories
---
## GaP Stories {{ title }}
> I've talked about this technique informally at a few conferences but never got round to writing it up (thanks [@anthonycgreen](https://twitter.com/anthonycgreen) for giving me the final push!)


### What are "GaP Stories"?

'GaP' stands for "Gap and Problem" (the geek in me can't resist a good [recursive acronym](http://en.wikipedia.org/wiki/Recursive_acronym)!) and it's a format for writing [User Stories](http://www.mountaingoatsoftware.com/agile/user-stories) which intends to focus on problems rather than solutions.

User Stories are great (I particularly like the concept that they are a placeholder for a conversation) - and they're a massive improvement on huge specification documents outlining the way everything "will work" - but they still have a habit of degrading into a list of demands.

    As a: Product Owner
    I Want: All my requirements to be met
    So That: I don't fire you

This can turn the whole story conception process into one of order-taking rather than problem-solving. It's the "I want" that really hammers that home (especially if, as above, the "User" ends up being a person of authority). I want, I want, I want.

The GaP Story idea is partly based on the above frustration with classical User Stories and partly on the idea I heard about years ago of capturing all software requirements (including missing features) as defects
(I can't remember where I first read about this but [@darrenhobbs](https://twitter.com/darrenhobbs) documents [a conversation](http://darrenhobbs.com/2004/01/16/defect-driven-development/) with [@jbrains](https://twitter.com/jbrains) back in 2004).



### A GaP Story takes the format:

    As a: <Persona>
    I Can't: <Gap/Problem>
    Therefore: <Impact>
    [Could We: <Suggestions>]


#### Let's break this down:

    As a: <Persona>

My rule is that the only "thing" you can use in this section is a published [Persona](http://www.measuringusability.com/blog/personas-ux.php) (if you don't have time to document a few bullet points about their motivation then you probably don't have time to write solutions for them!). It's ok to have "internal" personas but they should be the actual recipient (e.g. user) of the solution. If the reason for doing some piece of development is not to benefit a User of the system then perhaps a "User story" is not the best way of documenting it!



    I Can't: <Gap/Problem>

What is the need? What do we think we are unable to achieve with the system in it's current state. What's missing or what could we be doing better.  This is the clause that discourages feature requests and encourages describing a need.

    Therefore: <Impact>

Why does the above matter? Why are we even discussing this? Dollar Cost is ideal (but not always realistic). What is the impact of leaving this need unadressed. This clause turns every story into a mini business case.

    [Could We: <Suggestions>]

Everyone involved in product development should be able to contribute to solutions and this section allows for solution hypotheses. "Could We" underlines that this is a suggestion not an order. A very useful word when completing this section is "or" - sometimes just the process of suggesting and discussing different, competing, solutions leads to a better eventual outcome.

#### So that's it.

    As a: <Persona>
    I Can't: <Gap/Problem>
    Therefore: <Impact>
    [Could We: <Suggestions>]

Next time you are writing stories why not try the GaP approach (and let me know how you get on)?
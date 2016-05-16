---
layout: post
title: The Kano Method and MVPs
---

The Kano Method
----

In one of my last sessions at Agile Manchester, [Dave Grant](https://twitter.com/seize_the_dave) introduced The Kano Method (I'm reliably informed that's _car-no_ not _cane-no_) and we collectively used the model to understand the satisfaction of the conference itself.

The model works by classifying features into the following categories:

- **Basic** - (also Must-be) these are features that people expect. Any investment can offset negative feeling but you won't create positive feeling from basic features.
- **Exciter** - (sometimes Attractive/Delighters) features that cause unexpected excitement. When Exciter features reach a certain level then satisfaction goes off-the-scale... exponentially)
- **Performance** - (also Linear/One-dimensional) features that behave as you might expect - adding to the feature leads to linear growth of satisfaction.
- **Indifferent** - features that no matter how much you add to them, don't affect satisfaction one way or the other (remember that different people/demographics may feel differently about different features)

(There are also two more feature types but they are likely to indicate problems in your methodology **Reverse** and **Questionable**).

In order to understand how your demographic feel about these features a questionaire is used. Respondents are asked to rate their response to a set of statements in the following way (similar to [The Likert Scale](https://en.wikipedia.org/wiki/Likert_scale)):

- **Like**, **Expect**, **Neutral**, **Tolerate**, **Dislike**

The twist is that they are asked to answer both a  **Functional** and a **Dysfunctional** form of the question (I wonder if this is intended to prevent skew from [The Endowment Effect/Loss Aversion](https://en.wikipedia.org/wiki/Endowment_effect)).

e.g.

- How did you feel about having Workshops at Agile Manchester, as well as Talks/Experience Reports?
- How would you have felt if there had not been any Workshops at Agile Manchester?


Then the answers to the questions are fed through the following matrix (image stolen from the brilliant [Folding Burritos - Kano Model reference](http://foldingburritos.com/kano-model/)) to classify which category a feature belongs to, for that individual. In this version of the table some of the alternate terms are used so the key is **Q** - Questionable, **A** - Exciter, **P** - Perfomance, **R** - Reverse, **I** - Indifferent and **M** - Basic.


![](/img/posts/kano/EvalTable-Std.png)

So. If I had answered 'I liked having Workshops at Agile Manchester' and 'I would tolerate _not_ having workshops at Agile Manchester' then it would be classed as an **A - Exciter**.

(It's on my TODO list to write a mini app for this... we will see... Kano Model as a Service (KMaaS) anyone?)

I think the most challenging part was deciding an appropriate level of granularity for the features that we wanted to ask about. For example should I ask about "The Venue", "The location of the venue", "the locality of the venue for transport" etc... I'll have to try using the model in delivery to decide how to be guided in this respect.

MVPs (Minimum Viable Products)
----

So what does this all have to do with Minimum Viable Products?

*MVP* is a much over-used and mis-used (and therefore debated) term to help form a discussion over scoping initial releases of products. One of the most debated aspects is this diagram:


![](/img/posts/kano/mvphk.png)

With all sorts of [there is something wrong on the internet](https://xkcd.com/386/) type of outrage about how _"a skateboard won't do you any good if you want to travel to Scotland"_. My response:

> This is not a Product Owners guide on how to deliver a car to market.

It is simply using transportation as a _**metaphor**_ for trying to deliver end-user value incrementally (and in my experience many people - particularly the type of people that criticise this diagram - still significantly struggle to achieve this).

So onwards....

A more universally appreciated image about MVPs is this one:

![](/img/posts/kano/mvpthisnotthis.png)


I think it's spot on in terms of trying to fix the common misconception that an MVP is automatically low-fidelity. It shouldn't be low-fi, as something too "basic" will NOT likely be "viable".


The link (...finally!)
----

It was this model that triggered my neural connection with the Kano Method.

It's my hypotheses that a product should increment (let's not worry if it's the first, minimum, increment or not) relatively equally across **Basic**, **Performance** and **Exciter** features for your target demographic.

![](/img/posts/kano/kanomvp.png)

The aim should be to increment in the minimum number of **Basic** features that are required to form a consistent experience, with some new **Excite** and **Performance** features also being tested (as it's these that will allow you to match, or exceed, satisfaction benefits from your investment).

The Kano Method seems to be a pretty good way of understanding which features might fit the bill.










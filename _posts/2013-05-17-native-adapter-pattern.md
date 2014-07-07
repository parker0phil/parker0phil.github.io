---
layout: post
title: Integration Patterns for Native Mobile App Development

---

>  This  was an article written on behalf of [EqualExperts](http://www.equalexperts.com) for publication in [Software Developer's Journal May 2013](http://sdjournal.org/new-issue-iphone-development-all-you-have-to-know/)


Many native mobile apps Live or Die based on the APIs that serve them. Whether consuming third-party APIs or those delivered within your own enterprise, what’s the best way to get A talking to B?


## ‘Mashup’ Apps vs Native Adapter APIs
In this article I’ll mainly be comparing, and contrasting, two architectural patterns for integrating Native mobile applications with back-end services; the “Mashup” App and the Native Adapter API.

### ‘Mashup’ Apps
A ‘Mashup’ App is, perhaps, the most common way of integrating Native Apps with service providers. In this pattern the app integrates independently, and directly, with various backend services and APIs (these may be large scale third-party APIs, bespoke APIs delivered by partners, or APIs delivered within your own organisation).

![](/img/posts/native-adapter/mashup.png)

In the last 12 months I’ve noticed a steady rise in the amount of clients where architects are assuming this pattern and therefore that Native Apps (delivered internally) will directly consume “Strategic APIs”, also delivered within the enterprise.
In this pattern the App is responsible for meeting the contracts of each of the APIs, filtering data to meet the requirements of the application, and deciding how to present the data for the particular audience.

### Native Adapter APIs
A Native Adapter API is a dedicated component that provides services tailored towards one or more individual native apps (in architectural terms this can actually provide any, or all, of the functions of an Adapter, Bridge, Facade or Proxy [http://en.wikipedia.org/wiki/Design_Patterns#Patterns_by_Type](http://en.wikipedia.org/wiki/Design_Patterns#Patterns_by_Type)
![](/img/posts/native-adapter/adapter.png)
In this pattern there is a decision to be made about which responsibilities are fulfilled by the Native App and which by the Adapter API.
So lets delve straight into the comparison, which I’ve split into three separate sections; Performance, Security and Flexibility.

#### Performance
The precursor to any discussion on performance is always to remind ourselves that “Premature optimization is the root of all evil” [http://en.wikiquote.org/wiki/Donald_Knuth](http://en.wikiquote.org/wiki/Donald_Knuth)
With that out of the way lets talk about the performance benefits of Mashup Apps:

#### Scaling
If you are just  consuming other peoples APIs from a Native App then you are “scaling on the client-side” and have nothing to scale! The benefits of this should never be underestimated although many apps (particular being delivered by large enterprises) don’t actually have this luxury.
Shortest Path
Introducing a Native Adapter API means that there is an “extra hop” when getting a request from the device, to the backend API and back. The great thing about the Mashup App pattern in this respect is that you know that it’s not you introducing the bottleneck..
That said, there are potential performance benefits to be had by introducing a component that is dedicated to making life easier for the app.. (particularly if some of the APIs that you have to integrate with are not as performant as you might hope!)
So, still bearing in mind our feelings on “premature optimisation”, what options do a Native Adapter API introduce?

#### Caching
If the nature of your app means multiple users are making the same queries (or combination of queries ending in the same result) within an appropriately short period of time then caching can provide latency improvement for the later requests.
Ideally caching should be carried out “as close to source” as possible so before resorting to caching in your adapter you should see whether performance improvements can be delivered by the backend API.
However caching can also be used to deliver improved resiliency by serving “stale” data if a backend is unavailable therefore helping the app to maintain an acceptable level of service. This can be achieved in combination with a back-end API by respecting HTTP Cache headers set by the API.

#### Prefetching
Prefetching is a kind of caching where the Native Adapter API may be able to predict one or two services that the app may require next. In this case they would make the predicted calls in advance of the user actually invoking the features and store the result in cache. This can deliver performance benefits over and above that which could be delivered by tuning the backends but at the cost of wasting resources if the resulting calls don’t materialise.
Bandwidth Reduction and Request Pruning
Both performance improvements, and reduced consumer cost of data-usage, can be made by focussing on how much data is sent “over the wire” (this is especially so, in the case of cellular devices)
There are three main ways to reduce the bandwidth within a single request:
Remove Irrelevance - The simplest way to reduce bandwidth, strip out any data in the response that the native app is not interested in.
Provide Deltas - Where an app makes multiple requests to the same service but only a small part of the data has changed then the Adapter API can (by agreement) only send the changed parts. The ultimate case of this is sending a 304 (Not Modified) response if there has been no change at all.
Compression - in most cases it isn’t worth implementing custom compression but for very verbose data interchanges an Adapter can implement GZIP Content-Encoding (if not already supported by a backend API). Most Native App libraries will happily handle decompression on the client side.
Request pruning simply means combining multiple requests into one. This should only be done if the app would require the result of both requests to react to the user (otherwise the performance of the app will degrade to the lowest performing provider).

#### Security
The main security problem associated with the “Mashup” App pattern is that “There are no secrets on the client-side”. What this really means is that whatever you do to try and misdirect, hide or obfuscate what you are doing, everything that is done on a user’s hardware, or sent to a user’s hardware, can be observed by a user (whether that user is genuine, or an attacker!).
Keeping Secrets from an Attacker
A great example of needing to keep a secret is in an authentication scheme such as OAUTH2 [http://oauth.net/2/](http://oauth.net/2/). If you are integrating with an API using this sort of scheme then you will be issued with a Client ID and a Client Secret. Typically, you redirect the user to a login/permission dialog (either on the web or in a native app/account manager) and then you are issued an auth code. You then use your Client Id and Client Secret, and the auth code issued to you, to get access to the APIs. The problem is with a pure client-side implementation is that you have no way of keeping your client secret secret (!). This means that no API provider (in their right mind!) will give you access to any privileged APIs unless you have a way of getting round this. With a Native Adapter API, the client secret is kept securely on the server-side and so the problem is avoided.
Key Exposure - Denial of Service
In the case of an API that doesn’t return user information (but is protected by, for example, an API key) a relatively trivial , low-volume, Denial-of-service (DoS) attack can be carried out by an attacker taking a copy of your API key (via packet-capture, a ‘jailbroken’ device  or reverse engineering of client-side code). The attacker can then automatically play a high-volume of requests directly against the API using your key to activate any rate-limiting or throttling, thereby denying service to genuine users of the app.

#### Content Filtering
In the case of partner and internal APIs, the information returned from the APIs can often be rather richer than an application needs. In the case of a web application, then this is simply filtered out on the server-side and so the user is never aware (this can be thought of as “Keeping secrets from the user”!). However, in client-side apps we know that there are no such things as secrets but an Adapter API can be used to “filter” the response from an API to only send information to the client that is actually required in the actual user view.

Whilst there are certain aspects of security that can only be achieved server-side, introducing any component to an architecture is introducing another vector of attack; this also applies to a Native Adapter API.

#### Flexibility
One of the main decisions for businesses, when deciding to support users accessing content or services via a mobile device, is Native Application vs Mobile Web (eg, HTML5/Responsive Design).
Often the two points that swing the decision in the direction of a Native app are (1) Discoverability (being able to be found in an appstore has an enormous amount of value to a business) and (2) Native Interaction (either through actually needing ‘native-only’ features or, rightly or wrongly,  just feeling that there is a ‘crispness’ that can’t be delivered in non-native technologies).
However, once this decision is made many projects entirely abandon many of the benefits that a ‘mobile web’ project can give (e.g. fast changing content, instant delivery of incremental updates, seamless bug fixes etc). When a Native Mobile App is consuming APIs directly then all logic relating to the featureset of the App has to be built into the app. This means that the only way changes can be made is by forcing upgrades of the app. This is never a “free” process as incremental versions of the app often introduce new testing overhead and process overhead (dealing with the Wheels of Cupertino!) as well as creating ‘user apathy’ to new updates of the app.

A Native Adapter API can go some way to bridging the gap between a Native App and the benefits of Mobile Web:
Data-Driven Experiences
When using a Native Adapter API you have the option of pulling large amount of the presentation domain logic into the API. This could start off simply as string files for copy (allowing spelling corrections to be made without app releases) or  error mappings and messages and continue to item ordering logic, and business rules that operate on the responses of multiple APIs.
Ideally logic data should be delivered to the app “lazily”  so as not to negatively impact the overall user experience of the app. This could be done by combining logic data with actual data updates as well as possible background loading app logic updates and fixes after the app has delivered the main features to the users  (just don’t stick a spinner on the screen every time the app starts whilst downloading a whole new version of the app!)
“Shims”
Picture the scenario: you’ve just released a new app (with great fanfare!) which is consuming one of your Organization's “Strategic APIs”. Despite all of the automated testing that you had in place you find out on launch day (after the app has gone to the app store and users have started downloading it) that certain high-res devices have a rendering issue causing any prices of 3 digits not to be visible (prices of 2 and 4 digits are fine!) . What do you do?
At first this seems to be a case of the Irresistible Force vs the Immovable Object. The business rely on this being fixed, updates to the app store will take time (and not necessarily reach all users). Well, we all know that the Irrestible Force vs the Immovable Object is a paradox [http://en.wikipedia.org/wiki/Irresistible_force_paradox](http://en.wikipedia.org/wiki/Irresistible_force_paradox) and I can guarantee you that you’ll be asked to “hack” your Strategic API so that itpads all 3 digit prices to 4 digits. As you probably already have other consumers of the API you don’t want to change it for all consumers so you have to change the API to give a specialised (and incorrect!) response for a given consumer.... not looking so “Strategic” now is it!! So, a Native Adapter API can also “protect” inappropriate change being pushed onto strategic APIs.
Shims can also be used to make short term fixes to data anomalies that come from back-end data or to implement new business logic to meet short term deadlines that a Strategic API team might not be able to meet (but remember to give all of your automated test cases to the Strategic API team when they do come to implement the feature!).
In terms of flexibility it’s hard to make any argument for a “Mashup” app offering more than a Native Adapter API.

### In Conclusion
If you’re an app developer with limited resources, then don’t jump to start deploying Native Adapter APIs in the cloud, without considering what you are taking on.
However, if you are an enterprise shop, used to deploying and running server-side components then don’t forget to consider the benefits that a Native Adapter API could deliver.
Most importantly, this decision can be made on a per API basis leaving you with a hybrid model that should deliver the best of both worlds.
![](/img/posts/native-adapter/hybrid.png)
---
layout: post
title: REST 4XX Status Codes, Syntax and Semantics
---

The [Hypertext Transfer Protocol rfc(2616) states](http://tools.ietf.org/html/rfc2616#section-10.4):

> __Client Error 4xx:__ The 4xx class of status code is intended for cases in which the client seems to have erred.

## Contractual Obligations

Let's look at a couple of scenarios that illustrate what I see as an annoying gap in convention for how to handle this class of error.

> __400 Bad Request__ The request could not be understood by the server due to malformed syntax. The client SHOULD NOT repeat the request without modifications.

- GIVEN: The server has an "endpoint" ```POST /user```
- AND: Expects a json entity
```{ "username" : <at least 12 characters> } ```
- WHEN: Client requests ```POST /user```
- AND: Includes the JSON entity ```{ "usernam" : "this_is_long_enough" }``` (notice the json key)
- THEN: Server responds with ```400 BAD REQUEST ```

Compared to (assuming the same GIVEN, AND)

- WHEN: Client requests ```POST /user```
- AND: Includes the JSON entity ```{ "username" : "tooshort" }
- THEN: Server responds with ```400 BAD REQUEST```

The first example (where the username field name was misspelt) you could think of as a "psuedo-compile" error (e.g. it could be found through a sufficiently advanced static analysis tool).
The second example (where the value did not meet some validation criteria) could only really be found at runtime. However both give the same status code.

> __404 Not Found__ The server has not found anything matching the Request-URI. No indication is given of whether the condition is temporary or permanent.

- GIVEN: The server has an "endpoint" ```GET /report/{report_id}```
- AND: report '1' exists in the datastore
- WHEN: Client requests ```GET /reprt/1```
- THEN: Server responds with ```404 NOT FOUND```

Compared to (assuming the same GIVEN, AND)

- WHEN: Client requests ```/report/2```
- THEN: Server responds with ```404 NOT FOUND```

Again, the first example (where the resource type was misspelt) you know is wrong simply be inspecting the _contract_, but the second example you can't know if it's wrong without knowing the internal state of the provider.
Again, the same status code is returned.

So the two examples above give us two distinct classes of __"the client seems to have erred"__

1. Client did not meet the communicated (and "reasonably enforceable") contract
1. Client met the contract but provided illegal arguments

From a consumer point of view, the action they would have to take pretty different:

1. Get the development team out of bed to go back and inspect the contract more closely
1. Probably handled dynamically by the system (possibly propagating some sort of validation error back to a downstream system)






## Disambiguation Options

So given the above what are the options.

#### Do Nothing

The first option (always!) is Do Nothing. You are obviously already including explanations for 4xx errors as per [the spec](http://tools.ietf.org/html/rfc2616#section-10.4):

> __Client Error 4xx:__ the server SHOULD include an entity containing an explanation of the error situation, and whether it is a temporary or permanent condition

However, I find this unsatisfactory. It requires a level of inspection that shouldn't be required for such a broad difference in context, response entities are harder to see in most network devices, logging frameworks and debug tools and there is no good convention for how to structure them.

#### Use Decimal places in status codes

As per [Microsoft](http://www.microsoft.com/technet/prodtechnol/WindowsServer2003/Library/IIS/b1e3c76d-7569-4538-86bc-bd8194eb4823.mspx?mfr=true)

I'm not really a big fan. Whilst it's not explicitly against the spec as far as I can see, I think most people (and maybe protocol implementations!) expect status codes to be 3 digit integers NOT floats/strings  .

#### Create your own status codes

Nginx, twitter and Spring Framework have all had a habit of doing in the past.

But it means there is no external reference point for the (unless you are a big-enough deal to get on the [Wikipedia Page](http://en.wikipedia.org/wiki/List_of_HTTP_status_codes)!)


#### Use existing status codes more consistently

So it turns out the existing 4XX codes are sufficient if (a) you are willing to think more carefully about what they actually mean and (b) you don't mind resorting to some that came from the WebDav spec!

Contract failures:

- ```405 METHOD NOT ALLOWED``` = Request could not be pattern-matched (uri + method) to an endpoint (e.g don't know which contract to inspect). I'm advocating using this for e.g. `GET /doesnotexist`
- ```400 BAD REQUEST``` = Request matched endpoint but did not meet the agreed contract. In this case I mean it is more likely that the consumer dev team will need to change their implementation.

"Dynamic/Runtime" failures:

- ```422 UNPROCESSABLE ENTITY``` = Request matched and met syntactic contract but validation failed
- ```404 NOT FOUND``` = Request matched contract but stateful entity  (or ```409 GONE``` if you know a resource instance has been deleted)

The great thing about this is you get to use `422` which is my favourite [Http Status Dog](http://httpstatusdogs.com/)

![](/img/posts/4xx-status-codes/unprocessable-entity.jpg)
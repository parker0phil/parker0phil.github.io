---
layout: post
title: Enumeration of User IDs

---
Some organisations implement security in a way which allows “Enumeration of user ids”. Whilst this (as with everything) is a business decision and has to be weighed up against other factors, it should be made with full knowledge of implications.

In simple terms “Enumeration of User IDs” means that from the outside of the system, with no compromised access and no knowledge of user credentials it is possible to test whether or not a given user id is registered with the system.

The common use case of this (that “everybody knows”) is that when incorrect details are entered into a login form the correct response should be “User details were incorrect” rathern than “User does not exist” or “Password was incorrect”. This vulnerability can also be exposed through forgotten password features (for example asking for a user ID when the password has been forgotten and responding with an error message if the user does not exist).

The Open Web Application Security Project (OWASP) talks about enumeration of user ids here:

[Authentication_and_Error_Messages (OWASP)](https://www.owasp.org/index.php/Authentication_Cheat_Sheet#Authentication_and_Error_Messages)

[Testing_for_User_Enumeration_and_Guessable_User_Account (OWASP)](https://www.owasp.org/index.php/Testing_for_User_Enumeration_and_Guessable_User_Account_(OWASP-AT-002))

With a random, or self chosen, user id the main threat here is that revealing whether or not the user exists makes it easier for an attacker to target which user accounts are valid and so are worth brute­force password breaking against.

#### Email Address as User ID

If the user id that an organisation uses is an email address then there are two other threats that can result from allowing enumeration of user ids.

##### 1. Targeted Phishing Attacks

An attacker can use the fact that they know which email addresses have an account on a given website to carry out targeted phishing attacks. Targeting phishing attacks are less likely to be discovered due to “non­account holders” not reporting the anomaly.

##### 2. Targeted Direct Marketing (SPAM) Attacks

Competitors (and this includes Direct Marketers that may work outside the UK jurisdiction) can use the enumeration of email addresses to target customers with rival services and can also lead to “bad will” if customers assume that the marketing is linked to the genuine service (for example if customers use service­specific email addresses).

## Defense against Enumeration

There are only 2 defenses against enumeration and typically organisations will have to use both of these in different situations:

##### 1. Generify Error Messages and User Feedback

As in the example above with the login form customer feedback should not reveal whether a user id exists unless that user has already provided some form of authentication. This can lead to a more difficult customer experience, particularly in the case where users may not be sure if they have signed up for a service, or if they are unsure what user id they may have used.

##### 2. Make Direct Contact with User

Typically used in a forgotten credentials scenario. In this case when the user enters their user id then the service attempts to contact the user on a predefined side­channel (e.g. an email or text message). In addition to sending this communication a noncommittal response is made to the user (e.g. “If we find a user with that id then we will send an email to the registered email address”). If using email addresses as user ids then the experience can be slightly improved by sending an email to the address even if it is not registered. This email can confirm whether or not the user id is registered on the system (although be aware that this can also be used for malicious annoyance purposes if the amount of communications sent to a single address in a given time period is not limited).

## Mitigation against Enumeration

If the above defences are for some reason not feasible then the only way to attempt to mitigate the risk is by throttling the amount of guesses at a user id that an individual can make. Unfortunately the problem with this is how to identify an “individual” (i.e. what to limit on).

The most obvious/common choice is to limit on the caller's IP address. This can accidentally apply limits early (including on first attempts) when users are on a network that uses [Network Address Translation (NAT)](http://en.wikipedia.org/wiki/Network_address_translation) (i.e. multiple users on a network share a single ip address).

If the above is acceptable then the question is what to do when a limit is reached. If you prevent access to the service altogether then the limit itself can be used to create a [Denial\-of\-service attack](http://en.wikipedia.org/wiki/Denial-of-service_attack) by forcing the service to limit access to certain users (either from the same network, as above, or in combination with a [Cross\-Site Scripting (XSS)](https://www.owasp.org/index.php/Cross-site_Scripting_(XSS)) attack).

The alternative to limiting altogether is to introduce some sort of “automation defence” for example a [CAPTCHA](http://en.wikipedia.org/wiki/CAPTCHA). Unfortunately these provide a notoriously bad experience and they have been proven very cheap to circumvent for a determined attacker ([A Low\-cost Attack on a Microsoft CAPTCHA](http://homepages.cs.ncl.ac.uk/jeff.yan/msn_draft.pdf)).

## Conclusion

Whilst it sometimes seems at first glance that an individual’s customer experience can be improved by allowing enumeration the effect on the userbase as a whole can be very undesirable.

Be sure to properly evaluate whether the expected improvements in customer experience are so great to justify the implications (particularly bearing in mind that the industry, in general, has settled on an approach that does not allow this).
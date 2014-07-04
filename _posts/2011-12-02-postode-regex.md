---
layout: post
title: Postcode Regex
---
I was looking for a relatively simple postcode regex and came across [this](http://www.mgbrown.com/PermaLink66.aspx).

I removed a few of the capture groups (just retaining one for the [outcode](http://en.wikipedia.org/wiki/Postcodes_in_the_United_Kingdom#Format) which left me with this little beauty:

    ^[Gg][Ii][Rr] 0[Aa]{2}|([A-Za-z][0-9]{1,2}|[A-Za-z][A-Ha-hJ-Yj-y][0-9]{1,2}|[A-Za-z][0-9][A-Za-z]|[A-Za-z][A-Ha-hJ-Yj-y][0-9]?[A-Za-z]) {0,1}[0-9][A-Za-z]{2}$



You can see a groovy script using this here:

[http://groovyconsole.appspot.com/script/599002](http://groovyconsole.appspot.com/script/599002)

    def pattern = /^[Gg][Ii][Rr] 0[Aa]{2}|([A-Za-z][0-9]{1,2}|[A-Za-z][A-Ha-hJ-Yj-y][0-9]{1,2}|[A-Za-z][0-9][A-Za-z]|[A-Za-z][A-Ha-hJ-Yj-y][0-9]?[A-Za-z]) {0,1}[0-9][A-Za-z]{2}$/

    assert !("SW1A 1AAA" =~ pattern).matches()
    assert ("SW1A1AA" =~ pattern).matches()
    assert ("SW1A 1AA" =~ pattern).matches()

    assert ("SW1A 1AA" =~ pattern)[0][0] == "SW1A 1AA"
    assert ("SW1A 1AA" =~ pattern)[0][1] == "SW1A"
---
title: Stardates in Toto
date: 2010-06-06
layout: single
---

I was messing around with the date and instead of going with _the 6th day of the 6th month of the year 2010; a glorious Sunday!_, I decided to implement stardates. Don't worry, you can still mouseover the stardate to get the Gregorian date, but I figured, who pays attention to the date anyway? Well, it might catch your attention now.~

<script src="http://gist.github.com/427294.js?file=stardates-in-toto.rb"></script>

For the first two digits, I use a somewhat unique approach amongst trekkies: instead of using years since 1900, I drop the leading digit just like we do with our centuries. Thus, instead of today being 110430.1, it's simply 10430.1; this fits with the 5-digit years used in the series. Of course, my stardates won't align with those in the show.

The rest is the standard fractional year: there are 1000 stardates a year, so take the current day and divide it by the total number of days and multiply by 1000. Round to the nearest tenth. There's 8.76 hours per star-day, and 52 minutes per 0.1 star-day. (Granted, toto doesn't use time, just the date, so I'm using too many significant digits.)

As an aside, you might be interested to know this tidbit about the Gregorian calendar:

> [The Gregorian calendar] was introduced by Pope Gregory XIII, after whom the calendar was named, by a decree signed on 24 February 1582. The need for the Gregorian reform stemmed from the fact that the Julian calendar system assumes time between vernal equinoxes is 365.25 days, when in fact it is about 11 minutes less. The accumulated error between these values was about 10 days when the reform was made, resulting in the equinox occurring on March 11 and moving steadily earlier in the calendar. Since the equinox was tied to the celebration of Easter, the reform in the calendar was undertaken by the Roman Catholic Church.<sup>[[1]][1]</sup>

This was certainly an improvement, but now that we have standardized time, we also need [leap seconds][2].

[1]: http://en.wikipedia.org/wiki/Gregorian_calendar
[2]: http://en.wikipedia.org/wiki/Leap_second

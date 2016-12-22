---
layout: post
title:  "Introduce my PHP time conversion library"
introduction: "An quick introduction to my last created library to convert, compare and operate with time in different unit's."
---

Some time ago i saw this tweet from [@dantleech](https://twitter.com/dantleech).
[![https://twitter.com/dantleech/status/798842090482331648](/public/images/tweet_798842090482331648.png){:width="600px"}](https://twitter.com/dantleech/status/798842090482331648)

I know a library to convert some things called "php-conversion" from Christoffer Niska that can 
be found [here](https://github.com/crisu83/php-conversion). But this library has more than 
one context for conversion and personal for me a less good API.
 
By thinking about the problem, the PHP implementation of Martin Fowler's money pattern came into my mind.
And in german exist a saying: "Time is money". So why currencies cannot be time unit's? ;-)

So i'm implemented this pattern for time and the result of this idea 
is now available on [https://github.com/tommy-muehle/php-time-conversion](https://github.com/tommy-muehle/php-time-conversion)
The library comes with three logic parts. Converting, comparing and operating.

To understand that, here are an example:

```
use TM\TimeConversion\Time;
use TM\TimeConversion\Unit;

// initialization
$tenWeeks = Time::fromValue(10.0, Unit::WEEK);

// convert
$tenWeeksInDays = $tenWeeks->convert(Unit::DAY);

echo $tenWeeksInDays->getAmount(); // 70

// compare
var_dump($tenWeeks->equals($tenWeeksInDays)); // true

// operate -> subtract
echo $tenWeekInDays->subtract(Time::fromValue(10, Unit::DAY)); // 60
```

More examples can be found [here](https://github.com/tommy-muehle/php-time-conversion/tree/master/examples).

I don't care if this solve Dan's problem but it makes me a lot of fun to implement and test this library.
And maybe someone are searching a fully unit-tested and working library to handle time.

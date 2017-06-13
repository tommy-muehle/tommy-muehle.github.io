---
layout: post
title:  "First PHP7 extension"
introduction: "I created my first PHP7 extension for learning and as a farewell gift."
---

My time at [move:elevator](https://www.move-elevator.de) ends this week.
And I want to give the remaining colleagues a farewell gift.

I decided to make a tiny PHP extension so they can feel the lost spirit every day. Whether they want to or not. ;-)

So I created [my first PHP extension](https://github.com/tommy-muehle/php-in-memoriam-extension) with three "features".
The main feature, an output on every "php -v" call, was unfortunately not implementable in the short time:  

[![https://twitter.com/dantleech/status/798842090482331648](/public/images/tweet_870146339753078784.png){:width="600px"}](https://twitter.com/dantleech/status/870146339753078784)

But I (test-driven) create some cool features that you can check out, if you like.

## Here are my resources 

[Thomas Weinert](https://twitter.com/ThomasWeinert) published a 
great [PHP extension sample](https://github.com/ThomasWeinert/php-extension-sample) with which every
interested extension developer should start.

This repository has more than [thirty branches](https://github.com/ThomasWeinert/php-extension-sample/branches/stale) and every one of this show's one specific use case.
So the learning curve is extremely high.

Thomas also published [his slides](https://speakerdeck.com/thomasweinert/introduction-php-extensions) about this on Speaker Deck.

Another good resource was [Derick Rethans](https://twitter.com/derickr) [geospatial extension](https://github.com/derickr/geospatial). 
And I also read through [Sara Golemon's book](https://www.amazon.com/Extending-Embedding-PHP-Sara-Golemon/dp/067232704X). 

The last days I also read about the [PHP Internals Book](http://www.phpinternalsbook.com/).
This seems to be another great resource in the future if the [update for PHP7](https://github.com/phpinternalsbook/PHP-Internals-Book/issues/9) is finished.

---
layout: post
title:  "Starting my new project tmy.li"
introduction: "I'm starting a new project with different aspects for a proof of concept."
---

Since the last few day's i'm starting a new private project which will be
later available on [https://tmy.li](https://tmy.li). I also planned a blog 
series about.

And yes, it will be a further url shortener. But [bit.ly](https://bit.ly) 
and others does not have to be afraid.

It's a learning/proof of concept project with which i'm pursue some goals.  
My personal main goals are:

* use PHP 7.1
* use Symfony 3.x, especially [the micro-kernel feature](http://symfony.com/doc/current/configuration/micro_kernel_trait.html)
* use [DunglasActionBundle](https://github.com/dunglas/DunglasActionBundle)
* use Domain-driven Design with Symfony
* using [Let's Encrypt](https://letsencrypt.org/) with Docker-Compose and especially in production
* complete client rendering for the frontend with [ReactJS](https://facebook.github.io/react/)
* using [PHP-PM](https://github.com/php-pm/php-pm) with NGINX for high-performance 

But with the result, i will be create some links which are **one** character
shorter as by bit.ly for example:

```
http://tmy.li/nk/foo  -> 20 chars
http://bit.ly/2ifj8vn -> 21 chars
```

## Next steps

I planned this project in four parts. 

At first i'm initialize the project and create a dummy Symfony application.
The second part is about the infrastructure with Docker and Docker-Compose and how 
the services comes together.

Part three, the main part, is the real application implementation with front- and backend (API).
The last part is the PHP-PM integration.

After finishing a part i will be create a new blog post 
about this step.

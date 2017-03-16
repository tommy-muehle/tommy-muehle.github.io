---
layout: post
title:  "Starting my new project tmy.li"
introduction: "I'm starting a new project with different aspects for a proof of concept."
---

For the last few days I've been starting a new private project which will be
available on [https://tmy.li](https://tmy.li). I also planned a blog 
series about it.

And yes, it will be another url shortener. But [bit.ly](https://bit.ly) 
and others will not have to be afraid.

It's a learning/proof of concept project with which I'm pursuing some goals.  
My personal main goals are:

* use PHP 7.1
* use Symfony 3.x, especially [the micro-kernel feature](http://symfony.com/doc/current/configuration/micro_kernel_trait.html)
* use [DunglasActionBundle](https://github.com/dunglas/DunglasActionBundle)
* use Domain-driven Design with Symfony
* using [Let's Encrypt](https://letsencrypt.org/) with Docker-Compose and especially in production
* complete client rendering for the frontend with [ReactJS](https://facebook.github.io/react/)
* using [PHP-PM](https://github.com/php-pm/php-pm) with NGINX for high-performance 

As for the result, I want to create some links which are **one** character
shorter than the links of bit.ly. 

For example:

```
http://tmy.li/nk/foo  -> 20 chars
http://bit.ly/2ifj8vn -> 21 chars
```

## Next steps

I planned this project in four parts. 

At first I will initialize the project and create a dummy Symfony application.
The second part is about the infrastructure with Docker and Docker-Compose and how 
the services are coming together.

Part three, the main part, is the real application implementation with front- and backend (API).
The last part is the PHP-PM integration.

After finishing each step, I'm going to create a new blog post 
about it.

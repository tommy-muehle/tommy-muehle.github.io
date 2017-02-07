---
layout: post
title:  "A Composer plugin development environment"
introduction: "Introduce my personal Composer plugin/script development environment."
---

I created several Composer [plugins](https://github.com/tommy-muehle/composer-tool-installer-plugin) and [script's](https://github.com/tommy-muehle/tooly-composer-script) in the last time.
Currently i'm working on the [plugin for PHIVE](https://github.com/phar-io/composer-plugin).

So sometimes people ask me, how I develop and test a plugin or a Composer script.
So I released a [small development environment](https://github.com/tommy-muehle/php-composer-plugin-devenv) powered by Docker and Docker-Compose which you can
use to develop your own Composer extension.

With this "dev-env" you don't make your local Composer environment dirty and you don't need a
[further local Git repository and move some tags around](http://stackoverflow.com/questions/22935567/test-and-debug-composer-plugins).

The only requirement is to be minimal familiar with Docker and Docker-Compose.

## How it works

The [repository](https://github.com/tommy-muehle/php-composer-plugin-devenv) contains two container's. One for Composer and one for the plugin.
Both based on [Alpine Linux](https://alpinelinux.org/) and includes PHP 7.1 over CLI. So they are really small and use minimal disk space.
You can found every Docker image in the appropriate directory.

I added also a [dummy plugin](https://github.com/tommy-muehle/php-composer-plugin-devenv/tree/master/plugin/src) to see the process directly in action.
So after you checked out the repository via Git and built the container's via

```
docker-compose build
```

you can install your new (or the dummy) plugin via:

```
docker-compose run --rm composer global require my/new-plugin
```

The composer container depends on the plugin container. And the sources of your plugin are always symlinked 
to the Composer container. I used the [path type as repository option](https://getcomposer.org/doc/05-repositories.md#path) to register the plugin to Composer.
So you instantly see changes by every command call.

For example to see the new dummy Composer CLI command run:

```
docker-composer run --rm composer new-plugin:sample
```

## Notices and options

Of course you can change the PHP version which is used in the Docker container's.

If you need a special Composer version or want to test your plugin with a special one,
change the [Dockerfile in the composer directory](https://github.com/tommy-muehle/php-composer-plugin-devenv/blob/master/composer/Dockerfile) and extend for example from the [offical Composer Docker images](https://hub.docker.com/r/composer/composer/).

You can also extend the [Dockerfile for the plugin](https://github.com/tommy-muehle/php-composer-plugin-devenv/blob/master/plugin/Dockerfile). Maybe to install PHPUnit or an 
other testing framework. So you can run a command like this:

```
docker-composer run --rm plugin vendor/bin/phpunit
```

## Summary

I hope this help's other developers a lot to create a new Composer extension.  
And if you have questions or some additions, please leave a comment below.

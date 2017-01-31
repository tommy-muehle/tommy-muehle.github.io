---
layout: post
title:  "Docker image for PHPStan"
introduction: "Some day's ago i created a Docker image to check my older PHP 5.6 projects with the great PHPStan."
---

Today i updated [my implementation](https://github.com/tommy-muehle/docker-phpstan) to differentiate between releases  
and dev-master. 

Currently the latest release for PHPStan are "0.6.3". This is now available
via the new Docker image tag **0.6**.

So you can decide between the latest PHPStan version or one of the current release branch.
Also tests with Travis CI are now exist.

The result of this can be found here:
[https://hub.docker.com/r/tommymuehle/docker-phpstan](https://hub.docker.com/r/tommymuehle/docker-phpstan/)

The images are built daily at night. So you get always the latest commit on dev-master and the last release in the 0.6 branch by day.

## Use alias for short command option

For a better developer experience you can use an alias to type a short command that are expanded out into a much 
longer command. 

In my ~/.zshrc file (though you might have a ~/.bashrc or ~/.profile or something similar) i created a new alias:

```
alias phpstan="docker run -v $PWD:/app --rm tommymuehle/docker-phpstan"
```

Now I can simply type **phpstan** anywhere from the command line and the PHPStan image will kick up.

For example:

```
phpstan analyse src --level=5
```
in the /path/to/project directory was internally transformed to:
```
docker run -v /path/to/project:/app --rm tommymuehle/docker-phpstan analyse src --level=5
```

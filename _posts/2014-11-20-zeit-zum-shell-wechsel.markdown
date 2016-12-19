---
layout: post
title:  "Time to change your unix shell"
introduction: "Why i'am using the ZSH shell. (German)"
---

Letzte Woche habe ich einen sehr interessanten [Artikel](http://code.joejag.com/2014/why-zsh.html){:target="_blank"}
von Joe Wright bzgl. der Zsh Shell gelesen. Dieser hat mich ebenso dazu veranlasst, meine Standard-Shell unter Mac OS X zu wechseln.

Unter OS X ist bereits eine Zsh Shelll unter /bin/zsh vorhanden. Jedoch ist diese zum Tag heute in Version "5.0.5"
verfügbar. Aktuell ist jedoch Version "5.0.7". Diese ist auch über Homebrew installierbar.
Die Installation der aktuellen Zsh Shell über Homebrew ist dabei gewohnt einfach.

{% highlight bash %}
brew update
brew install zsh
{% endhighlight %}

Danach muss der von Homebrew erzeugte Symlink unter "/usr/local/bin/zsh" noch in die Liste der verfügbaren
Shells aufgenommen werden.

{% highlight bash %}
sudo vim /etc/shells
# Inhalt der Datei
/bin/bash
/bin/csh
/bin/sh
/bin/tcsh
/bin/zsh
# Hinzufuegen
/usr/local/bin/zsh
{% endhighlight %}

Danach könnt ihr mit dem folgenden Befehl eure Standard-Shell wechseln:

{% highlight bash %}
chsh -s /usr/local/bin/zsh
{% endhighlight %}

Ab jetzt empfiehlt es sich, seine Shell-Konfiguration mit ["oh-my-zsh"](https://github.com/robbyrussell/oh-my-zsh){:target="_blank"}
zu erweitern. Die Installation wird auf der Github-Seite gut beschrieben.
Mit "oh-my-zsh" stehen dann sehr sehr viele [Plugins](https://github.com/robbyrussell/oh-my-zsh/tree/master/plugins){:target="_blank"}
und [Themes](https://github.com/robbyrussell/oh-my-zsh/tree/master/themes){:target="_blank"} zur Verfügung, so dass man sein "Shell-Erlebnis" entsprechend anpassen kann.

Aktuell nutze ich das Theme "avit" sowie die Erweiterungen für Git, Vagrant, OSX und Symfony2.
Nach 3 Tagen testen bin ich immernoch sehr angetan und kann es nur jedem empfehlen.

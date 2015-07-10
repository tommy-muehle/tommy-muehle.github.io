---
layout: post
title:  "Composer, PHPCS und Coding-Standards"
date:   2014-10-03 12:49:00
categories: PHP COMPOSER
---

Mittlerweile ist es die Regel seine PHP-Projekte mit [PHPCS](https://github.com/squizlabs/PHP_CodeSniffer){:target="_blank"} auf den jeweiligen zu verwendenden Coding-Standard zu prüfen.

In Symfony2 Projekten bspw. mit Hilfe von [lapistano's Symfony2 Coding-Standard](https://github.com/lapistano/Symfony2-coding-standard){:target="_blank"} oder aber auch gegen eigene vorab im Team festgelegte Coding Guidelines.
Installiert man PHPCS jedoch via Composer erhält man nur die bereits integrierten [Standards](https://github.com/squizlabs/PHP_CodeSniffer/tree/master/CodeSniffer/Standards){:target="_blank"}

Um jetzt seine eigenen Standards/Styles zu hinterlegen und diese im Projekt nutzen zu können, verwende ich [Composer-Hooks bzw. Scripts](https://getcomposer.org/doc/articles/scripts.md){:target="_blank"}.
Hierbei nutze ich Callbacks, welche nach jedem "update" oder "install" Command aufgerufen werden:

{% highlight json %}
{
    "require-dev": {
        "squizlabs/php_codesniffer": "1.5.4"
    },
    "scripts": {
        "post-update-cmd": "TM\\MyClass::postUpdate",
        "post-install-cmd": "TM\\MyClass::postInstall"
    },
    ...
}
{% endhighlight %}

In der aufgerufenen PHP-Klasse installiere ich den erforderlichen Standard dann mithilfe von Composer's Zip-Downloader im richtigen Verzeichnis.
Den Code dazu findet ihr im entsprechenden Gist auf Github.

Danach könnt ihr euren projekt-spezifischen Code bspw. wie folgt testen:

{% highlight bash %}
./vendor/bin/phpcs --standard=Symfony2 --extensions=php src
{% endhighlight %}

### Tipp

Das Ganze hat natürlich noch einen riesen Vorteil. Nutze ich ein Integrationssystem wie bspw. Hudson oder Jenkins muss ich auf diesen nicht mehr alle möglichen Standards zentral hinterlegen, sondern verlagere dies in die jeweiligen Projekte.

### Update

Alternativ kann der Standard natürlich auch via absoluten Pfad angegeben werden.
PhpStorm unterstützt dies mittlerweile mit der "Custom"-Option auch.
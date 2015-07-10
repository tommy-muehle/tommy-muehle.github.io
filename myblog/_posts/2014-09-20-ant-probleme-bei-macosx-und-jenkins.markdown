---
layout: post
title:  "Probleme mit dem lokalen Jenkins und Ant"
date:   2014-09-20 20:00:00
categories: MAC ANT JENKINS
---

Ich benutze bereits für mehrere Projekte einen lokalen Jenkins auf meinem MacBook, um Releases auf Codestyle, Unit-Tests usw. zu prüfen.
Teilweise als Pre-Commit Hook für Github Projekte.

Nach einem Tausch der Jenkins-Installation von der Homebrew-Variante auf den normalen Installer wurde ich jetzt mit zweierlei Fehlermeldungen konfrontiert.

Die erste Meldung war:

> IOException: Cannot run program "ant" ...

Gut dachte ich, setzt du die zu verwendende Ant-Version fest. 
Ant hatte ich bereits vor einiger Zeit per Homebrew installiert.

In der allgemeinen Jenkins-Konfiguration:
![My helpful screenshot](/images/fixed_ant_version1.png)

Und in der jeweiligen Job-Konfiguration:
![Job Konfiguration](/images/fixed_ant_version2.png)

Beim nächsten Buildversuch erschien dann folgende Meldung:

> Could not find or load main class org.apache.tools.ant.launch.Launcher

Eine kurze Google-Suche bestätigte die Vermutung, dass hier ein PATH Problem vorlag. 
Mein Jenkins-Server nutzt eine eigene PATH-Variable.

### Und hier die Lösung
Ihr könnt in der folgenden Datei die PATH Variable einfach selbst definieren.

{% highlight bash %}
/Library/LaunchDaemons/org.jenkins-ci.plist
{% endhighlight %}

Dazu stoppt ihr euren Jenkins zu allererst mit:

{% highlight bash %}
sudo launchctl unload /Library/LaunchDaemons/org.jenkins-ci.plist
{% endhighlight %}

Dann fügt ihr folgenden Teil ein:

{% highlight xml %}
<key>EnvironmentVariables</key>
<dict>
    <key>PATH</key>
    <string>/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/bin:/usr/local/git/bin</string>
</dict>
{% endhighlight %}

Damit werden bspw. auch alle durch Homebrew nachinstallierten Programme und Pakete gefunden. Dann startet ihr euren Jenkins mit folgenden Befehl wieder:

{% highlight bash %}
sudo launchctl load /Library/LaunchDaemons/org.jenkins-ci.plist
{% endhighlight %}

Jetzt sollte ant wieder gefunden und die Tasks wieder ausgeführt werden.

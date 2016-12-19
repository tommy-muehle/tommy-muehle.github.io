---
layout: post
title:  "PHP application packages"
introduction: "How to build and distribute PHP application packages. (German)"
---

Als [Sebastian Bergmann](https://twitter.com/s_bergmann){:target="_blank"} vor ein paar Tagen bei uns war,
erwähnte er beiläufig, man könne doch als Resultat eines Builds der CI ein RPM oder DEB der Applikation auswerfen,
welches dann zum Produktiv-System übertragen und installiert wird.

Ein Thema, welches mich nicht so richtig losgelassen hat. Die Vorstellung unsere (Symfony2-) Applikationen
versioniert und geprüft zu packen und dann als eine Datei zu deployen, fande ich extrem spannend.

Aktuell ist der Synchronisationsprozess und die Release-Nachbereitung bei uns doch noch etwas aufwändiger.

### Spannendes Thema

In den letzten Tagen habe ich dazu ziemlich viel recherchiert und probiert.
Erste Versuche mit [rpmbuild](http://www.rpm.org/max-rpm-snapshot/rpmbuild.8.html){:target="_blank"} und mit zahlreichen [Snippets](https://gist.github.com/catchamonkey/8575619f450fe4c94acd){:target="_blank"}
und [Tutorials](http://www.sme-server.de/download/Howtos/RPM-Build/Beginners_Guide_Own_RPMs.htm){:target="_blank"} führten aber nicht so richtig zum Ziel.

Vorab war ich bereits auf [FPM](https://github.com/jordansissel/fpm){:target="_blank"} - Effing Package Management gestoßen.
Doch die vielen Optionen inkl. Support mehrerer Paket-Typen je nach Distribution befand ich zu Beginn viel zu viel,
sodass ich davon Abstand genommen hatte. :weary:

Da die Versuche mit meinem Spec-File und meiner simplen Symfony Konsolen-App dann aber auch nicht
richtig funktionierten, beschloss ich nochmal FPM auszuprobieren.

### Mithilfe von FPM zum Ziel

Mit FPM und etwas Zeit kam ich dann doch recht schnell voran, sodass ich einen funktionieren CLI-Befehl hatte, welcher
das RPM baute. Dieses konnte ich dann in meiner [CentOs Testbox](https://github.com/tommy-muehle/puppet-vagrant-boxes/releases/download/1.0.0/centos-6.6-x86_64.box){:target="_blank"}
auch erfolgreich installieren. :muscle:

Jetzt musste das Ganze nur noch irgendwie auf den Jenkins bzw. ins [Ant](http://ant.apache.org/){:target="_blank"}.
Dabei sieht mein Anwendungsfall wie folgt aus:

* Auschecken einer bestimmten Version der Applikation auf Basis eines Git-Tags
* Abhängigkeiten via Composer laden
* Qualitätssicherung durchführen
* Deployment und Integration
* Integrationstests durchführen

### Mein Lösungsansatz

Hier mein aktueller Lösungsansatz. Vorher noch ein Hinweis. Da wir bei uns ausschließlich CentOs 6.4 - 6.6
einsetzen, erzeugt dieser ein RPM. Ein DEB wäre aber mit einer Type-Änderung auch möglich. (siehe mein Website-Beispiel)

{% highlight xml %}
<project default="build" basedir="./">

    <property environment="env"/>

    <property name="php" value="/usr/bin/php56"/>
    <property name="php_ini_file" value="/opt/remi/php56/root/etc/php.ini"/>

    <target name="build" depends="vendors, package" />

    <target name="vendors">
        <exec dir="${basedir}/source" executable="php" failonerror="true">
            <arg value="composer.phar" />
            <arg value="update" />
            <arg value="--optimize-autoloader" />
        </exec>
    </target>

    <target name="package" description="Start fpm to build the RPM package.">
        <exec executable="fpm">

            <!-- target os -->
            <arg value="--rpm-os"/>
            <arg value="linux"/>

            <!-- architecture (x86_64, noarch etc.) -->
            <arg value="-a"/>
            <arg value="all"/>

            <!-- source-type -->
            <arg value="-s"/>
            <arg value="dir"/>

            <!-- package-type -->
            <arg value="-t"/>
            <arg value="rpm"/>

            <!-- name of package -->
            <arg value="-n"/>
            <arg value="my_application"/>

            <!-- force overwriting rpm file -->
            <arg value="--force"/>

            <!-- path on target-system -->
            <arg value="--prefix=/var/www/my_application/releases/1"/> <!-- Do flexible release -->

            <!-- exclude files -->
            <arg value="-x"/>
            <arg value="**/.git*"/>
            <arg value="-x"/>
            <arg value=".git*"/>
            <arg value="-x"/>
            <arg value="**/logs/**"/>
            <arg value="-x"/>
            <arg value="**/bin/**"/>
            <arg value="-x"/>
            <arg value="**/cache/**"/>

            <!-- revision number -->
            <arg value="--iteration"/>
            <arg value="1"/> <!-- CI build-number -->

            <!-- version -->
            <arg value="--version"/>
            <arg value="1.0"/> <!-- Project version -->

            <!-- package build path -->
            <arg value="-p"/>
            <arg value="${basedir}/build/"/>

            <!-- script for doing some tasks on target machine -->
            <arg value="--after-install"/>
            <arg value="${basedir}/source/after_install.sh"/>

            <!-- user things -->
            <arg value="--rpm-user"/>
            <arg value="root"/>
            <arg value="--rpm-group"/>
            <arg value="root"/>

            <!-- share variables from jenkins to scripts like after_install -->
            <arg value="--template-scripts"/>
            <arg value="--template-value"/>
            <arg value="php=${php}"/>
            <arg value="--template-value"/>
            <arg value="php_ini_file=${php_ini_file}"/>

            <!-- Path to the source-files. Note: This path are combined with prefix (see above). -->
            <arg value="source"/>

        </exec>
    </target>
</project>
{% endhighlight %}

Mein genutztes "after-install"-Skript, mit den von Ant übermittelten Variablen, sieht wie
folgt aus:

{% highlight bash %}
#!/bin/bash -ex

echo "Used PHP: <%= php %>"
echo "Used PHP .ini file: <%= php_ini_file %>"

<%= php %> -c<%= php_ini_file %> -r "echo 'Do something ...' . PHP_EOL;"

{% endhighlight %}

Resultat des Ganzen ist dann die Datei "my_application-1.0-1.noarch.rpm".
Diese lässt sich dann auf dem Zielsystem wie folgt installieren:

{% highlight bash %}
rpm -ivh my_application-1.0-1.noarch.rpm
{% endhighlight %}

### Ausbaufähig

Das Ganze ist jetzt natürlich noch sehr ausbaufähig und bildet auch noch nicht den genannten Anwendungsfall ab.
Aber es ist wie geschrieben ein Ansatz. Es fehlen beispielsweise noch die Abhängigkeiten (depends). Auch die Version könnte
noch automatisch aus dem Git gezogen werden. Weiterhin müssten die Skripte (before- und after-install) noch auf die jeweiligen Environments angepasst werden.

### Live-Beispiel

Für meine eigene Website auf Basis von [Silex](http://silex.sensiolabs.org/){:target="_blank"} erzeuge ich
[hiermit](https://github.com/tommy-muehle/tommy-muehle.de/blob/master/build/release.xml){:target="_blank"}
aber bereits ein DEB Paket für das Deployment. Natürlich ist dies jedoch eine sehr simple Applikation.

### Ausblick

Wer nicht die Pakete händisch auf die Systeme übertragen und installieren möchte, kann natürlich auch einen
eigenen [Repository-Server](http://wiki.centos.org/HowTos/CreateLocalRepos){:target="_blank"} aufsetzen, von welchem sich die Applikations-Server die neuesten Versionen ziehen.

#### Links

* [[FPM]](https://github.com/jordansissel/fpm){:target="_blank"}
* [[Beispiel]](https://github.com/tommy-muehle/tommy-muehle.io/blob/48e031b69f8963863a4721d1de300d2e58e06e35/release.xml){:target="_blank"}

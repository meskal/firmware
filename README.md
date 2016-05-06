
m

to

Aktuelle Firmware
=================

Prerequisites
-------------

`apt-get install zlib1g-dev lua5.2 build-essential unzip libncurses-dev gawk subversion git realpath libssl-dev`

Sicherlich müssen noch mehr Abhängigkeiten Installiert werden, diese Liste wird sich hoffentlich nach und nach Füllen. Ein erster Ansatzpunkt sind die Abhängigkeiten von OpenWRT selbst

`git clone `[`https://github.com/FreifunkFranken/firmware.git`]
`cd firmware`

Erste Schritte
--------------

Mit Hilfe der Community-Files werden Parameter, wie die ESSID, der Kanal sowie z.B. die Netmon-IP gesetzt. Diese Einstellungen sind Community weit einheitlich und müssen i.d.R. nicht geändert werden.

`./buildscript selectcommunity community/franken.cfg`

Je nach dem, für welche Hardware die Firmware gebaut werden soll muss das BSP gewählt werden:

`./buildscript selectbsp bsp/board_ar71xx.bsp`
`./buildscript`

Was ist ein BSP?
================

Ein BSP (Board-Support-Package) beschreibt, was zu tun ist, damit ein Firmware Image für eine spezielle Hardware gebaut werden kann.

Typischerweise ist eine folgene Ordner-Struktur vorhanden:

-   .config
-   root\_file\_system/
    -   etc/
        -   rc.local.board
        -   config/
            -   board
            -   network
            -   system
        -   crontabs/
            -   root

Die Daten des BSP werden nie alleine verwendet. Zu erst werden immer die Daten aus dem “default”-BSP zum Ziel kopiert, erst danach werden die Daten des eigentlichen BSPs dazu kopiert. Durch diesen Effekt kann ein BSP die “default” Daten überschreiben.

Der Verwendung des Buildscripts
===============================

-   Das BSP file durch das Buildscript automatisch als dot-Script geladen, somit stehen dort alle Funktionen zur Verfügung.
-   Das Buildscript lädt ebenfalls automatisch das Community file und generiert ein dynamisches sed-Script, dies geschieht, damit die Templates mit den richtigen Werten gefüllt werden können.

./buildscript prepare
---------------------

-   Sourcen werden in einen separaten src-Folder geladen, sofern diese noch schont da sind. Zu den Sourcen zählen folgende Komponenten:
    -   OpenWRT
    -   Sämtliche Packages (ggfs werden Patches angewandt)
-   Ein ggfs altes Target wird gelöscht
-   OpenWRT wird ins Target exportiert (kopiert)
-   Eine OpenWRT Feed-Config wird mit dem lokalen Source Verzeichnis als Quelle angelegt
-   Die Feeds werden geladen
-   Spezielle Auswahl an Paketen wird geladen
-   Patches werden angewandt
-   board\_prepare() aus dem BSP wird aufgerufen (wird. z.B. fur Patches für eine bestimmte HW verwendet)

./buildscript build
-------------------

-   prebuild
    -   $target/files aufräumen
        -   (In $target/files liegen Dateien, die später direkt im Ziel-Image landen)
    -   Files aus default-bsp und bsp kopieren
    -   OpenWRT- und Kernel-Config kopieren
    -   board\_prebuild() aus dem BSP wird aufgerufen
-   Templates transformieren
-   GIT Versionen speichern: $target/files/etc/firmware\_release
-   OpenWRT: make
-   postbuild
    -   board\_postbuild() wird aufgerufen

./buildscript config
--------------------

Um das Arbeiten mit den OpenWRT .config's zu vereinfachen bietet das Buildscript die Möglichkeit die OpenWRT menuconfig und die OpenWRT kernel\_menucon

  [`https://github.com/FreifunkFranken/firmware.git`]: https://github.com/FreifunkFranken/firmware.git

# Was ist ein digitaler Schaukasten und wie richte ich ihn ein

**Copyright:** Die Idee des Digitalen Schaukastens auf Basis eines Raspberry Pis stammt ursprünglich von Lutz Neumeier und wurde unter http://neumedier.de/schaukasten.html unter ccbysa veröffentlicht.
Die  Idee wurde von mir jedoch auf Grund sicherheittechnischer Bedenken, insbesondere auf Grund der "upload.php" verändert. In meiner Version werden die Seiten ausschließlich über einen lokalen Webserver bereitgestellt und sind nicht "von außen" verfügbar.

**Autor:** Lena Carta  
**Datum:** 04.04.2020    

Den klassischen Schaukasten vor dem Gemeindehaus oder oder Kirche kennt wohl jeder. Hier gibt es aktuelle Information rund um Veranstaltungen und Termine der Kirchengemeinde.
Oft sind hier Flyer und Plakate zufinden, ganz klassisch auf Papier. Ein digitaler Schaukasten soll diesen analogen Präsentationsmodus in das digitale Zeitalter führen. Die einzelnen Seiten in einem klassischen Schaukasten werden dabei zu einzelnen digitalen Seiten, die man auf einem Bildschirm betrachten kann. Wie die einzelnen Seiten dabei aussehen, kann jeder selbst festlegen. Einige Beispiele, die man dirkt befüllen kann, findet man unter /Beispielseiten oder auch bei neumdier.de von dem ich diese Idee aufgenommen und angepasst habe.

## Material

Um einen digitalen Schaukasten aufzubauen, braucht man je nach Anspruch und Umfang die folgenden Materialien:

Notwenige Materialien:
- Einen Bildschirm, das senkrecht aufgehangen werden kann (in unserem Beispiel mit FullHD Auflösung)
- Ein Zuspielgerät, das dafür sorgt, dass unsere Seiten auf dem Bildschirm erscheinen (in unserem Beispiel ein RaspberryPi3 B+ mit Raspian und Stromversorgung)
- Ein Kabel passender Länge, um das Zuspielgerät mit dem Bildschirm zu verbinden (in unserem Beispiel ein HDMI-Kabel)
- Maus und Tastatur um alles einzurichten.

Optional Materialien (vereinfachen die Einrichtung)
- Einen eigenen "Webspace"/Homepage auf der ihr neue Seite erstellen könnt - die Seiten für euren Schaukasten.
- zusätzlich dazu dann Internet an dem Ort, an dem euer Zuspielgerät steht.
- Zum Anpassen und erstellen einer Seite einen Texteditor (bspw. Notepad++)


Für technisch nicht so versierte Benutzer, oder alle, die nicht sehr viel Zeit investieren möchten, bietet sich das optionale Zubehör an. Dann ist der Schaukasten mit wenig Aufwand einzurichten.

## Wie soll das Ganze funktionieren?

Die einzelnen Seiten eures Schaukastens sind einzelne php-Seiten, die nach einander aufgerufen werden. Wielange dabei eine einzelne Seite zu sehen ist, lässt sich einstellen.
In unserem Beispiel wird eine Seite 30 Sekunden angezeigt.Die orange gefärbten Bereiche sind dafür für Inhalte vorgesehen. Hier kann man Text plazieren oder Bilder einfügen. Wie das geht steht im nachfolgenden Abschnitt.

Damit das Ganze später ohne Einwirkung des Nutzers funktioniert, startet der RaspberryPi, den wir konfigurieren wollen, alles automatisch. Zur Benutzung sind also später nur die folgenden Schritte nötig:

1) RaspberryPi mit dem Bildschrim per HDMI-Kabel verbinden verbinden (FullHD Auflösung).

2) Bildschirm anschalten und auf die HDMI Quelle umschalten.

3) RaspberryPi mit dem Strom verbinden und warten.


## Erstellen einer Seite

Die einzelnen Seiten, die im Ordner Beispielseiten zu finden sind, können nach belieben angepasst werden.
Dazu muss eine solche Seite mit einem Texteditor am PC geöffnet werden - dabei nicht erschrecken, es sieht schlimmer aus, als es ist!

### Die Struktur einer Seite

Alle Zeilen die in `<!---->` eingefasst sind, sind sogenannte Kommentare. Sie haben keinen funktionalen Nutzen, sondern erklären, was die darunter befindlichen Zeilen tun.
Da gerne einmal durchschauen, was es so gibt, alle wichtigen Anpassungen werden gleich erklärt.

Unter `<!--Definieren, wie bestimmte Elemente gerendert werden-->` werden einzelne Bereiche angelegt (erkennbar durch den # zu Beginn der Zeile) und festgelegt, wie groß die Bereiche sind und welche Farbe sie haben. So enstehen auf den einzelnen Seiten auch die orangenen Kästchen. Um ein wenig zu verstehen, die das Ganze funktioniert, bietet es sich an dort mit den Werten etwas herumzuspielen und sich dann mal anzusehen, wie das Ganze aussieht. Dazu die Seite als html-Seite abspeichern (aus Seite1.php wird Seite1.html) und diese entweder per Doppelklick oder über "rechte Maustaste -> öffnen mit" im Browser öffnen.
WICHTIG Die Seiten sind für einen hochkant verwendeten FullHD Bildschirm gedacht, also einer Auflösung von 1080x1920. Hat man einen PC mit FullHD Auflösung kann man den etwa um 90 Grad rotieren um das zu simulieren. Sonst kann man zumindest einen kleinen Eindruck vo Design bekommen, in dem man das Browserfenster kleiner zieht.

Ab dem Bereich `<!--Konkreter Inhalt in die oben definierten Bereiche-->` kann nun der eigentliche Inhalt angepasst werden. Das passiert in HTML. Einen sehr guten Einlick (auf Englisch) gibt hier https://www.w3schools.com/html/. 

Natürlich können auch einfach selbst Seiten erstellt und/oder bestehende Seite verändert werden. Auf eine Besonderheit ist hier zu achten. In einer der oberen (Meta-)Zeilen sorgt dafür, dass die Seiten automatisch nacheinander erscheinen. Das wird durch
`<meta http-equiv="refresh" content="30; URL=http://pischaukasten/schaukasten/index2.php">` erreicht.
Die URL ist dabei anzupassen, wenn ihr nicht der Anleitung folgt, sondern eure eigene Homepage verwendet. In diesem Fall muss die URL der Link auf die jeweils nächste Seite sein, die angezeigt wird. Die letzte Seite in dieser Kette verweist wiederum auf die Erste.

## Grundeinrichtung des Pi

Die Nachfolgenden Schritte beschreiben eine komplette Konfiguration des RaspberryPi OHNE die optionalen Materialien. Statt also die Homepage der eigenen Germeine zu verwenden werden alle Seiten "lokal" geladen, der RaspberryPi braucht deshalb auch keine Internetverbindung. Wer die eigene Homepage verwenden möchte, muss wie oben erwähnt die Links in seinen Schaukastenseiten anpassen und kann dann direkt mit einem späteren Abscnitt weitermachen.

Damit können wir mit der Grundeinrichtung des RaspberryPi beginnen:

1) Raspian auf den RaspberryPi spielen:
    * Dazu Raspbian Buster with desktop von https://www.raspberrypi.org/downloads/raspbian/ herunterladen.
    * Entsprechend SD Karte formatieren, bspw. mit Raspberry Pi Imager (https://www.raspberrypi.org/downloads/).  
2) Die fertige SD-Karte in den RaspberryPi stecken, Maus und Tastatur sowie einen Bildschirm per HDMI verbinden. Ist der RaspberryPi mit Strom versorgt startet er auf den Dektop.
3) Grundlegende Einstellungen vornehmen, bspw. ändern des Passworts. Das Standardpasswort des Benutzers Pi ist *raspberry*.
4) Unter dem allgemeinen Menü unter *Pi Config* den Rechnernamen auf pischaukasten ändern oder alternativ später die Websiten anpassen.
5) In der *Pi Config* den Zugriff per SSH oder VNC erlauben, sollte das später erwünscht sein (ich selbst habe alles per SSH eingerichtet).

So konfiguriert ist der Pi start klar. Sollten keine automatischen Updates installiert worden sein, dann einmal per

```shell
sudo apt-get update && sudo apt-get upgrade -y
```
im Terminal ausführen.

Alle nachfolgenden Befehle werden im Terminal ausgeführt (lokal, oder durch Zugriff per SSH).


## Einrichten von Apache und PHP 7.1

Um intern einen eigenen kleinen Webserver zu verwenden, installieren wir Apache2. Das wird uns ermöglichen später im Browser unsere Websiten entsprechend aufzurufen und automatisch durch zu scrollen.

### Apache2 installieren

1) Apache2 kann mit dem folgenden Befehl installiert werden:

    ```
    sudo apt install apache2
    ```
2) Ausschalten von "Directory Listing", da wir keinen Index-Baum angezeigt bekommen möchten.

    ```
    sudo sed -i "s/Options Indexes FollowSymLinks/Options FollowSymLinks/" /etc/apache2/apache2.conf
    ```

3) Apache bei Systemstart starten:

    ```
    sudo systemctl stop apache2.service
    sudo systemctl start apache2.service
    sudo systemctl enable apache2.service
    ```

Damit ist Apache2 erstmal installiert. Es fehlt nun noch PHP 7.1 um dann die entsprechenden Konfigurationen vorzunehmen.

### PHP 7.1 installieren

1) Als erstes die Paketliste überprüfen, dazu folgenden Befehl aufrufen.

    ```
    sudo nano /etc/apt/sources.list
    ```
2) Dort solte der folgende Eintrag vorhanden sein, wenn nicht, dann entsprechend anpassen:

    ```
    deb http://raspbian.raspberrypi.org/raspbian/ buster main contrib non-free rpi
    ```

3) Wenn Änderungen vorgenommen wurden einmal updaten:

    ```
    sudo apt-get update && sudo apt-get upgrade -y
    ```

4) Fall bereits PHP installiert war, (bei Neuinstallation nicht der Fall) die beiden nachfolgenden Befehle ausführen.

    ```
    dpkg -l |grep php
    ```

    ```
    sudo apt-get remove '^php.*'
    ```

5) Nun PHP an sich installieren:

    ```
    sudo apt-get install php7.1-cli php7.1-common php7.1-curl php7.1-gd php7.1-json php7.1-mbstring php7.1-mysql php7.1-opcache php7.1-readline php7.1-xml
    ```

6) Anschließend Apache neu starten.

    ```
    sudo systemctl restart apache2.service
    ```

Damit sind alle Vorbereitungen getroffen um den Webserver einzurichten.

### Einrichten des Webservers

Da wir die Seiten des Schaukastens später unter http://pischaukasten/schaukasten erreichen wollen, konfigurieren wird dies nun.

1) Erstelle einer neuen Konfigurations Datei für /schaukasten:

    ```
    sudo nano /etc/apache2/sites-available/schaukasten.conf
    ```

2) Die folgenden Daten in die Datei eintragen:

    ```
    <VirtualHost *:80>
            DocumentRoot /var/html/schaukasten/

            ErrorLog /var/log/apache2/error.log
            LogLevel warn
            CustomLog /var/log/apache2/access.log combined

    </VirtualHost>
    ```

3) Alle Dateien die zum Darstellen der Seite benötigt werden, sollten laut obiger Datei (DocumentRoot) in */var/html/schaukasten/* zu finden sein. Deswegen wird dieser Ordner nun angelegt:

    ```
    sudo mkdir /var/html/schaukasten
    ```

4) Der Ordner muss nun mit allen Daten befüllt werden, sprich php-Seiten, Bildern und ähnlichem. Dazu die Daten nach */var/html/schaukasten* kopieren, bspw. durch Anpassen des entsprechenden Befehls:

    ```
    sudo mv <source> <destination>
    ```

5) Damit Apache den Inhalt lädt, einmal neu starten.

    ```
    sudo systemctl restart apache2.service
    ```

Nun sollte die erste Seite (index.php) unter http://pischaukasten/schaukasten zu sehen sein. Alle 30 Sekunden wird die Seite aktualisiert und nacheinander alle Seiten von index.php bis index5.php angezeigt. Die letzte Seite leitet dann wieder über zu ersten.

### Den Raspberry Pi konfigurieren

Wir wollen nun den Pi so konfigurieren, dass wir ihn nur noch an den Bildschirm/Monitor/TV anschließen müssen und er automatisch im Hochformat durch die konfigurierten Seiten scrollt.

1) Chromium als Webbrowser installieren um http://pischaukasten/schaukasten aufzurufen.

    ```
    sudo apt-get install chromium-browser
    ```

2) Damit die Maus nicht angezeigt wird installieren wir unclutter.

    ```
    sudo apt-get install unclutter
    ```

3) Da unsere Seiten im Hochformat angezeigt werden sollen, müssen wir den Bildschrim rotieren, dazu

    ```
    sudo nano /boot/config.txt
    ```
    ausführen und `display_rotate=3` am Ende einfügen.

4) Wir ändern nun noch die Autostart-Datei, damit bspw. Chromium automatisch im Kioskmodus gestartet wird und sämtliche Fehlermeldungen unterdrückt werden. Dazu

    ```
    sudo nano /etc/xdg/lxsession/LXDE-pi/autostart
    ```
    ausführen. Die Datei ändern, sodass sie wie nachfolgend aussieht:

    ```
    @lxpanel --profile LXDE-pi
    @pcmanfm --desktop --profile LXDE-pi
    @xscreensaver -no-splash
    @unclutter
    @chromium-browser --icognito --kiosk --noerrdialogs http://pischaukasten/schaukasten
    @xset s noblank
    @ xset s off
    @xset -dpms
    @sed -i 's/"exited_cleany":false/""exited_cleany":true/' ~/.config/chromium/Default/Preferences
    ```

    Wer seine eigene Homepage verwendet, muss die Zeile "@chromium-browser" entsprechend anpassen.
    Nach einem Neustart des Pi sollte nun alles wie oben beschrieben funktionieren!

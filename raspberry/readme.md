# Raspberry


## **Hardware**
Folgende Einplatinenrechner wurden erfolgreich getestet
| Einplatinenrechner | Version |
| :- | :- |
| Raspberry Pi 3 | B+ |
| Raspberry Pi 4 | 4GB |

Raspberry Pi Zero, Raspberry Pi Zero W werden aufgrund der erhöhten Hardwareanforderungen nicht empfohlen!
<br><br>

## **Software**

| Datei | Version (**) | Link |
| :- | -: | :-: |
| Raspberry Pi Imager | 1.6.2 | [Webseite](https://www.raspberrypi.com/software/) |
| Raspberry Pi OS Lite (*) | Bullseye - 30.10.2021 | [Webseite](https://https://www.raspberrypi.com/software/operating-systems/) |
| OctoPi & OctoPrint (*) | 0.18.0 & 1.7.3 | [Webseite](https://github.com/OctoPrint/OctoPi-UpToDate/releases) |

(*) Aufzählung dient zur Versionskontrolle und müssen bei Nutzung des Raspberry Pi Imager nicht zurätzlich herunter geladen werden. <br>
(**) Die Versionen beziehen sich auf den Zeitpunkt der Erstellung dieser Dokumentation.
<br>Als Grundsystem für diese Dokumentation liegt Windows 10 vor.

<br>


## Software - Installation | Konfiguration

<details>
    <summary><b><i> Raspberry Pi Imager </i></b></summary>

Empfohlen wird eine Class10 SD-Karte mit mindestens 8GB Flashspeicher. Diese kann nun eingelegt und im Hauptmenü unter *SD-KARTE WÄHLEN" ausgewählt werden.

Vorbereitend für die beschriebenen Varianten werden folgende Konfigurationen über *Erweiterte Optionen* (erreichbar über Strg+Shift+X) getroffen:

- *Hostname* aktivieren und vergeben
- *SSH aktivieren* und Passwort vergeben
- *Wifi einrichten* aktivieren und Anmeldeinformationen eingeben
- *Spracheinstellungen festlegen* aktivieren und konfigurieren

</details>

<details>
    <summary><b><i> Variante: Raspbian & OctoPrint </i></b></summary>

Text

*   <details>
    <summary><b><i> Raspbian </i></b></summary>

    Die folgende Anleitung ist nicht als Komplettdokumentation zu sehen! Sie beschreibt lediglich einen oberflächlichen Weg zu einem funktionsfähigem Steuersystem eines 3D-Druckers.

    *   <details>
        <summary><b><i> Installation | Flash </i></b></summary>

        Über den Menüpunkt *OS WÄHLEN* wird das Image unter *Raspberry Pi OS (other) > raspberry Pi OS Lite (32-bit)* ausgewählt und über den Menüpunkt *SCHREIBEN* auf die SD-Karte übertragen.

        Nach dem Schreibvorgang kann die SD-Karte in einen Raspberry eingelegt und jener gestartet werden.

        Der Erste Bootvorgang und die Einwahl in das LAN/WLAN kann bis zu 2 Minuten in Anspruch nehmen!

        </details>

    *   <details>
        <summary><b><i> Konfiguration </i></b></summary>

        Per Kommandozeile wird nun eine SSH-Verbindung aufgebaut:

        > ssh pi@\<HOSTNAME>

        > ssh pi@\<IP>

        Im Anschluss werden folgende Einstellungen getroffen:

        Ein gesondertes Passwort für *root* setzen

        > sudo passwd

        *Raspberry Pi Software Configuration Tool* aufrufen und diverse Einstellungen treffen

        > sudo raspi-config

        - Beim Booten auf eine Netzwerkverbindung warten
            > 1 System Options > S6 Network at Boot > Yes

        - Raspberry Pi Camera-Option aktivieren
            > 3 Interface Options > P1 Camera > Yes

        - Systemsprache ändern
            > 5 Localisation Options > L1 Locale > de_DE.UTF-8 UTF-8 aktivieren
            
            > Systemsprache auf de_DE.UTF-8 ändern

        Paketlisten aktualisieren

        > sudo apt-get update

        System aktualisieren

        > sudo apt-get upgrade

        Nicht mehr benötigte bzw. nicht mehr unterstützte Pakete deinstallieren

        > sudo apt-get autoremove

        Das System neustarten, um die Änderungen zu initialisieren

        > sudo reboot now

        </details>

    </details>

*   <details>
    <summary><b><i> OctoPrint </i></b></summary>

    Text

    *   <details>
        <summary><b><i> Installation </i></b></summary>

        Text

        Abhängigkeiten nachinstallieren

        > sudo apt install python3-pip python3-dev python3-setuptools python3-venv git haproxy

        OctoPrint Ordner erstellen und aufrufen

        > mkdir OctoPrint && cd OctoPrint

        Eine virtuelle Umwelt für Octoprint aktivieren

        > python3 -m venv venv
        > source venv/bin/activate

        PIP upgraden

        > pip install pip --upgrade
        
        OctoPrint installieren

        > pip install octoprint

        User \<Pi> zur Dialout-Gruppe hinzufügen

        > sudo usermod -a -G tty pi

        > sudo usermod -a -G dialout pi

        Vorbereitungen für den Autostart von OctoPrint

        > wget https://github.com/OctoPrint/OctoPrint/raw/master/scripts/octoprint.service && sudo mv octoprint.service /etc/systemd/system/octoprint.service

        Autostart für OctoPrint aktivieren

        > sudo systemctl enable octoprint.service

        Das System neustarten, um die Änderungen zu initialisieren

        > sudo reboot now

        </details>

    *   <details>
        <summary><b><i> Konfiguration </i></b></summary>

        Text

        *   <details>
            <summary><b><i> Grundkonfiguration </i></b></summary>

            Nach erfolgreicher Installation kann OctoPrint über das WebInterface aufgerufen und Konfiguriert werden.

            > http://\<IP>:5000

            > http://\<HOSTNAME>:5000

            Die folgenden Einstellungen sind selbsterklärend und sollten aufmerksam durchgelesen und mit bedacht ausgefüllt werden. 
            
            Folgende Empfehlungen sollten beachtet werden:

            - Zugangsbeschränkung > Ein Useraccount mit sicherem Passwort anlgegen
            - Onlineprüfung aktivieren
            - Plugin Blackliste aktivieren
            - Druckerprofil anlegen

              *   <details>
                  <summary><b><i> Ender-5 </i></b></summary>

                  Allgemein

                  > Name > Ender-5

                  > Modell > Creality

                  Druckbett & -volumen

                  > Breite (X) > 235

                  > Tiefe (Y) > 235

                  > Höhe (Z) > 300

                  Achsen

                  > Z-Achse invertieren

                  </details>

              *   <details>
                  <summary><b><i> Ender-5 Plus </i></b></summary>

                  Allgemein

                  > Name > Ender-5 Plus

                  > Modell > Creality

                  Druckbett & -volumen

                  > Breite (X) > 330

                  > Tiefe (Y) > 330

                  > Höhe (Z) > 400

                  Achsen

                  > ???Z-Achse invertieren

                  </details>

            - Serverbefehle
              - OctoPrint neustarten
                > sudo service octoprint restart
              - System neustarten
                > sudo shutdown -r now
              - System herunterfahren
                > sudo shutdown -h now

            - Webcam & Zeitraffer
              - Stream-URL
                > /webcam/?action=stream
              - Snapshot-URL
                > http://127.0.0.1:8080/?action=snapshot
              - Pfad zu FFMPEG
                > /usr/bin/ffmpeg

            Der Wizard-Einstellungen sind nun gesetzt. Ab hier werden alle Einstellungen über das OctoPrint eigene Menü (erreichbar über das Maulschlüsselsymbol oben rechts) erledigt.

            - Drucker > Temperaturen > Voreinstellungen

                | | Extruder | Bett |
                | :- | :-: | :-: |
                | Vorwärmen | 65 | 65 |

            - Drucker > GCODE Scripts

              *   <details>
                  <summary><b><i> Ender-5 </i></b></summary>

                  > Vor dem Start eines Druckjobs
                  > ```
                  > G28								; Homing

                  > Nach Vollendung eines Druckjobs
                  > ```
                  > G91								; Aktuelle Position relativieren
                  > G1 E-5							; Hotend 5mm Retract
                  > G1 Z5							; Hotend um 5mm vom Bauteil entfernen
                  > G28 X Y							; X-Y Achse in HomePosition
                  > M104 S0							; Hotendheizung abschalten
                  > M140 S0							; Bettheizung abschalten
                  > M106 S0							; Bauteileluefter abschalten
                  > M84 X Y E						; X Y E Motoren abschalten
                  > ```

                  > Nach dem Abbruch eines Druckjobs
                  > ```
                  > G91								; Aktuelle Position relativieren
                  > G1 E-5							; Hotend 5mm Retract
                  > G1 Z5							; Hotend um 5mm vom Bauteil entfernen
                  > G28 X Y							; X-Y Achse in HomePosition
                  > M104 S0							; Hotendheizung abschalten
                  > M140 S0							; Bettheizung abschalten
                  > M106 S0							; Bauteileluefter abschalten
                  > M84 X Y E						; X Y E Motoren abschalten

                  > After serial connection to printer is established
                  > ```
                  > M42 I1 P5 S255					; Hotendluefuter einschalten
                  > M42 I1 P6 S255					; Boardluefter einschalten
                  > G28 X Y							; Homing X- Y-Achse

                  > Before serial connection to printer is closed
                  > ```
                  > M104 S0							; Hotendheizung abschalten
                  > M140 S0							; Bettheizung abschalten
                  > M84								; Alle Motoren abschalten

                  </details>

              *   <details>
                  <summary><b><i> Ender-5 Plus </i></b></summary>

                  > Vor dem Start eines Druckjobs
                  > ```
                  > M301 E0 P28.08 I2.79 D70.67		; PID-Tuning Nozzle 2021-11-24
                  > M304 E-1 P119.40 I10.21 D930.67	; PID-Tuning Bed 2021-11-24
                  > M851 Z-2.75						; Z-Probe Offset 2021-12-15
                  > G28								; Homing

                  > Nach Vollendung eines Druckjobs
                  > ```
                  > G91								; Aktuelle Position relativieren
                  > G1 E-5							; Hotend 5mm Retract
                  > G1 Z5							; Hotend um 5mm vom Bauteil entfernen
                  > G28 X Y							; X-Y Achse in HomePosition
                  > M104 S0							; Hotendheizung abschalten
                  > M140 S0							; Bettheizung abschalten
                  > M106 S0							; Bauteileluefter abschalten
                  > M84 X Y E						; X Y E Motoren abschalten
                  > ```

                  > Nach dem Abbruch eines Druckjobs
                  > ```
                  > G91								; Aktuelle Position relativieren
                  > G1 E-5							; Hotend 5mm Retract
                  > G1 Z5							; Hotend um 5mm vom Bauteil entfernen
                  > G28 X Y							; X-Y Achse in HomePosition
                  > M104 S0							; Hotendheizung abschalten
                  > M140 S0							; Bettheizung abschalten
                  > M106 S0							; Bauteileluefter abschalten
                  > M84 X Y E						; X Y E Motoren abschalten

                  > After serial connection to printer is established
                  > ```
                  > M301 E0 P28.08 I2.79 D70.67		; PID-Tuning Nozzle 2021-11-24
                  > M304 E-1 P119.40 I10.21 D930.67	; PID-Tuning Bed 2021-11-24
                  > M851 Z-2.75						; Z-Probe Offset 2021-12-15
                  > G28 X Y							; Homing X- Y-Achse

                  > Before serial connection to printer is closed
                  > ```
                  > M104 S0							; Hotendheizung abschalten
                  > M140 S0							; Bettheizung abschalten
                  > M84								; Alle Motoren abschalten

                  </details>

            </details>

        *   <details>
            <summary><b><i> Optionale Erweiterungen </i></b></summary>

            Text

            </details>

        </details>

    </details>

</details>

<details>
    <summary><b><i> Variante: OctoPi & OctoPrint </i></b></summary>

Text

</details>
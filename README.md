# HB-UNI-Sen-DUMMY-BEACON
**Dummy-Device zum Vermeiden von "Kommunikation gestört"-Servicemeldungen deaktivierter HomeMatic-Geräte (nur HM-RF, kein HmIP / Wired).**<br/>

Die ist z.B. sehr nützlich bei saisonal verwendeten Geräten, die in Programmen verwendet werden, jedoch z.B. in Wintermonaten nicht genutzt werden.
Denn ein Ablernen der Geräte während der Nutzungspausen würde bedeuten, dass Programme nach dem erneuten Anlernen wieder bearbeitet werden müssten.

Es erfolgt:
 - eine automatische Aussendung zyklischer Telegramme an die CCU (bei Sensoren)
 - eine Quittung (Ack) von Steuerbefehlern von der CCU (bei Aktoren)

## Hardware
### Bauteile
 - 1x Arduino Pro Mini (3.3V 8MHz) (ca. 2,20 EUR bei eBay)
 - 1x CC1101 Funkmodul 868MHz (ca. 2,60 EUR bei eBay)
 - 1x FTDI Adapter (falls nicht schon vorhanden, gibts bei Amazon)
 - 1x Taster (beliebig, z.B. Kurzhubtaster)
 - 1x Widerstand 330Ohm
 - 1x LED
 
### Verdrahtung
![wiring](Images/wiring.png)

Die Stromversorgung erfolgt indealerweise aus einem 5V-Steckernetzteil und wird an **RAW** (+) sowie **GND** (-) des Pro Mini angeschlossen.

## Software
### Arduino IDE
- Arduino IDE [herunterladen](https://www.arduino.cc/en/Main/Software) und installieren
- AskSinPP Bibliothek als [ZIP herunterladen](https://github.com/pa-pa/AskSinPP/archive/master.zip) 
- notwendige Bibliotheken in der Arduino IDE hinzufügen:
  - Sketch -> Bibliothek einbinden -> .ZIP-Bibliothek hinzufügen
    - heruntergeladene AskSinPP Bibliothek ZIP-Datei auswählen
  - Sketch -> Bibliothek einbinden -> Bibliotheken verwalten
    - im Suchfeld folgende Bibliotheken suchen und installieren:
      - EnableInterrupt
      - Low-Power
 - Board einstellen:
   - Board: `Arduino Pro or Pro Mini`
   - Prozessor: `ATmega328P (3.3V, 8 MHz)`
   - Port: `COM-Port` des FTDI Adapters <br>
 - Sketch öffnen. (Die 3 Dateien [HB-UNI-Sen-DUMMY-BEACON.ino](https://raw.githubusercontent.com/jp112sdl/HB-UNI-Sen-DUMMY-BEACON/master/HB-UNI-Sen-DUMMY-BEACON.ino), [HB_Device.h](https://raw.githubusercontent.com/jp112sdl/HB-UNI-Sen-DUMMY-BEACON/master/HB_Device.h) und [HB_MultiChannelDevice.h](https://raw.githubusercontent.com/jp112sdl/HB-UNI-Sen-DUMMY-BEACON/master/HB_MultiChannelDevice.h) müssen sich in einem gemeinsamen Verzeichnis befinden.)
            
- Sketch hochladen:
  - Sketch
    - Hochladen

## HomeMatic
### Addon installieren
Damit das Gerät von der CCU erkannt und unterstützt wird, ist es erforderlich, das [JP-HB-Devices-addon](https://github.com/jp112sdl/JP-HB-Devices-addon) zu installieren.<br/>Es wird **mindestens Version 1.18** benötigt.

### Anlernen an HomeMatic
Das Anlernen erfolgt nach Installation des Addons wie man es von anderen HomeMatic-Geräten gewohnt ist:
- Geräte anlernen -> HM-Gerät anlernen klicken
- **Config-Taster** am Arduino Pro Mini **kurz** drücken
- das neue Gerät erscheint anschließend im Posteingang


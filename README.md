# HB-UNI-Sen-DUMMY-BEACON
**Dummy-Device zum Vermeiden von "Kommunikation gestört"-Servicemeldungen bis zu 8 deaktivierter HomeMatic-Geräte (nur HM-RF, kein HmIP / Wired).**<br/>

Die ist z.B. sehr nützlich bei saisonal verwendeten Geräten, die in Programmen verwendet werden, jedoch z.B. in Wintermonaten nicht genutzt werden.
Denn ein Ablernen der Geräte während der Nutzungspausen würde bedeuten, dass Programme nach dem erneuten Anlernen wieder bearbeitet werden müssten.

Es erfolgt:
 - eine automatische Aussendung zyklischer Telegramme an die CCU (bei Sensoren)
 - eine Quittung (Ack) von Steuerbefehlen der CCU (bei Aktoren)
 
_Wird ein Telegramm des "echten" Gerätes empfangen, so wird das Aussenden der Fake-Telegramme automatisch deaktiviert, um Kollisionen / Fehlverhalten zu vermeiden._
 
 ![bedienung](Images/CCU_Bedienung.png)

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

### Konfiguration
Damit das Dummy-Device weiß, wessen Telegramme es nun ersetzen soll, müssen die **Adressen** _(Achtung: nicht die Seriennummern!)_ der Geräte im Dummy-Device hinterlegt werden.
<br/>
_Hintergrund:
Die Adresse (RF_ADDRESS) eines HomeMatic-Geräts ist leider nicht ohne Umwege in der WebUI ersichtlich.
Sie lässt sich beispielsweise als hexadezimalwert dem Device-File im Dateisystem `/etc/config/rfd/<SERIAL.dev>` entnehmen:<br/>
<img src="Images/dev-file.png" width=350 />
<br/>_

Um das Hinterlegen der gewünschten Adressen weitestgehend zu automatisieren ist wie folgt vorzugehen:<br/><br/>
**1.** In den Geräteeinstellungen des Dummy-Device ist ein Kanal umzubenennen.<br/>
Der Kanalname muss beginnen mit **X** gefolgt von der Seriennummer, des HomeMatic Geräts, das abgebildet werden soll.<br/>
Beispiel:<br/>
 - das HomeMatic-Gerät hat die Seriennummer: **NEQ0386972**
 - der Kanalname des Dummy-Device muss lauten: **XNEQ0386972**

![gerate](Images/CCU_Geraete.png)
 <br/>
 
 **2.** Ausführen des [Parametrierungsskripts](https://raw.githubusercontent.com/jp112sdl/HB-UNI-Sen-DUMMY-BEACON/master/additional/ccu_script.txt) auf der CCU.<br/>
(Sollte die Seriennummer des Dummy-Device *nicht* **JPBEACON01** lauten, so ist diese im Skript entpsrechend zu ändern (`string FAKEDEV = "JPBEACON01";`).)
<br/>
Durch Ausführen des Skripts werden nun die Adressen der Geräte ausgelesen und zum Dummy-Device übertragen.

**3.** Einstellungen<br/>
![einstellungen](Images/CCU_Einstellungen.png)
<br/>
In den Einstellungen können Parameter
 - für das Gerät
   - Max. Sendeversuche: Anzahl der Sendeversuche bei bidirektionalen Nachrichten
 - je Kanal (also je emuliertem Gerät)
   - Geräte-ID: RF-Adresse; hier dezimal! (wird vom Skript gesetzt)
   - Übertragungsintervall: alle x Sekunden wird ein zyklisches Telegramm gesendet (bei Sensoren)
   - Telegrammübertragung aktiviert: Dummy-Nachrichten werden für dieses Gerät generiert
 
 festgelegt werden.

**4.** Betrieb<br/>
Unter "Status und Bedienung"->"Geräte" ist eine Übersicht der aktivierten Kanäle zu sehen.<br/>
![bedienung](Images/CCU_Bedienung.png)
<br/><br/>
**Achtung:** Der Status der Telegrammübertragung (aktiviert / deaktiviert) muss nicht zwingend der Einstellung wie unter **3.** festgelegt entsprechen!<br/>
Es wird der tatsächliche Status wiedergegeben. Ist z.B. ein Kanal in **3.** aktiviert und es wird jedoch ein Funktelegramm vom "echten" HomeMatic-Gerät empfangen, wird der Kanal in der Übersicht deaktiviert!

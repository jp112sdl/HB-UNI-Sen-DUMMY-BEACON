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

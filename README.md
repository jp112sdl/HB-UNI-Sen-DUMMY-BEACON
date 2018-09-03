# HB-UNI-Sen-DUMMY-BEACON
**Dummy-Device zum Vermeiden von "Kommunikation gestört"-Servicemeldungen deaktivierter HomeMatic-Geräte (nur HM-RF, kein HmIP / Wired).**<br/>

Die ist z.B. sehr nützlich bei saisonal verwendeten Geräten, die in Programmen verwendet werden, jedoch z.B. in Wintermonaten nicht genutzt werden.
Denn ein Ablernen der Geräte während der Nutzungspausen würde bedeuten, dass Programme nach dem erneuten Anlernen wieder bearbeitet werden müssten.

Es erfolgt:
 - eine automatische Aussendung zyklischer Telegramme an die CCU (bei Sensoren)
 - eine Quittung (Ack) von Steuerbefehlern von der CCU (bei Aktoren)

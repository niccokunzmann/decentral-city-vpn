# Dezentrales Community-VPN

A decentral city and community VPN for Freifunk.

## Motivation

Die bestehenden Lösungen für ein gemeinschaftliches VPN in Potsdam und Berlin laufen auf eniger als 10 Servern und
sind von weniger als 10 Personen aktiv verwaltet.
Hier können wir eine Dezentralisierung anstreben, die es anderen Freifunkern einfacher macht, zu der VPN-Infrastruktur beizutragen.

Grundsätzlich kann man die bestehenden VPN-Lösungen in zwei Klassen einteilen, die sich nicht ausschließen:

- **Mesh-VPN**  
  Diese VPN-Server dienen in Potsdam und Berlin dazu, Freifunkrouter, die keinen Funkkontakt zu den
  Gemeinschaftsnetzen haben, an das Gemeinschaftsnetz via VPN anzugliedern.
  Durch sie kann man Dienste und andere Geräte aus allen verbundenen Freifunkinseln erreichen.
- **Gateway-VPN**  
  Dieses VPN bietet kein Meshing bzw. Verbindung der Clients untereinander.
  Der eingehende Verkehr wird durch den Server weiter in das Internet geleitet.

### Zertifikatvergabe

Die VPNs sind unverschlüsselt.
Trotzdem gibt es eine Authentifizierung, die sicherstellt, dass nicht jeder das VPN benutzen kann.
Diese erfolgt über Zertifikate, die den Server und den Client gegenüber einander identifizieren.

Bei den zentralen Instanzen ist es so: ein Client erhält ein Zertifikat vom dem VPN-Server-Betreiber
über eine E-Mail mit dem privaten Schlüssel des Clients, dem öffentlichen Zertifikat des Servers und
dem privaten Zertifikat des Clients.

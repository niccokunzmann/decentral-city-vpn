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

### Dezentralisierung

Eine Dezentralisierung des VPNs ermöglicht, dass die Inseln ausfallsicherer angebunden werden.
Gleichzeitig bietet ein privates, schnelles Aufsetzen von VPN-Servern für Freifunk, dass mehr 
bestehende LAN-Infrastruktur zum Meshen genutzt werden kann: Es erlaubt Meshing zwischen DMZ und internem Netz.
Bei großen Netzen (Universität, Firma) kann das Meshing über die LAN-Segmente hinweg erfolgen.

## Implementierung

Wie können wir ein solches dezentrales Netz implementieren?
Es gibt verschiedene Punkte, die die Skalierung ermöglichen:

- Jeder Client kann für alle Server ein Zertifikat nutzen. Damit das Vertrauen hergestellt ist, bedeutet das, dass der Client selbst entscheiden kann, wer das Zertifikat generiert.
- Jeder Server kann entscheiden, welche Clients sich verbinden können.
- Server können untereinander wie Clients funktionieren. Damit erlauben wir, dass Inseln über kleine VPN-Server angebunden werden und nicht jeder Client zu jedem Server verbunden werden kann.

## Aufsetzen eines Servers

Einen Server aufzusetzen sollte einfach sein.
Neue Clients im Server zu erlauben, sollte einfach sein.
Fremde Clients der Community im eigenen Server zu erlauben, sollte einfach sein.

Vorgeschlagene API:

    docker run niccokunzmann/decentral-community-vpn

Das lässt einen Server laufen. Folgende Konfiguration erfolgt über die Umgebungsvariablen:

- `SERVER_CLIENT_CERTIFICATE_URL`  
  eine URL zu gemeinschaftlich verwalteten, öffentlichen Schlüsseln als ZIP-Datei.
- `SERVER_CLIENT_CERTIFICATE_URL_UPDATE` default 3600  
  Die Zeit, nach der die Zertifikate erneuert werden.
- `SERVER_FORWARD_TO_INTERNET` defaults to `false`  
  Wird der Server-Verkehr in das Internet weitergeleitet?
- `SERVER_MESH` defaults to `true`  
  Können die Clients durch den Server meshen?
- `SERVER_INTERCONNECTIONS_URL`  
  Eine URL zu einer ZIP-Datei, in der die Zertifikate und URLs für Server enthält,
  mit denen man sich verbinden möchte.
  Das servereigene Zertifikat wird für die Vernindung genutzt.
- `PRINT_SERVER_CERTIFICATE` defaults to `true`  
  Gibt beim Start den öffentlichen Teil des Serverzertifikates aus.
  Damit kann man ihn zu der Liste in der `SERVER_INTERCONNECTIONS_URL`
  hinzufügen.
- `SERVER_AS_CLIENT_CONNECTIONS` defaults to `true`  
  Die Server der Gemeinschaften legen selbst Zertifikate an.
  Diese können in einen Ordner kopiert werden, damit sich der Server automatisch mit
  diesen Servern verbindet.

Weiterhin kann man sich überlegen, verschiedene Tags für die Communities einzufügen,
sodass diese verschiedene Konfigurationen haben können.

    docker run niccokunzmann/decentral-community-vpn:potsdam
    docker run niccokunzmann/decentral-community-vpn:berlin

Diskussionen
------------

- [[Berlin-wireless] puppets für community tunnel - wo sind die Puppenspieler?](https://lists.berlin.freifunk.net/pipermail/berlin/2018-April/037490.html)
- [[FFP] Dezentrales VPN](https://lists.freifunk-potsdam.de/pipermail/users/2018-March/018483.html)
- [[FFP] Zwecke vom Potsdam-VPN](https://lists.freifunk-potsdam.de/pipermail/users/2018-March/018478.html)
- [[Berlin-wireless]Foo-over-UDP (FoU) oder L2TP+Tunneldigger als Alternative zu OpenVPN für VPN03, c-base heute ](https://lists.berlin.freifunk.net/pipermail/berlin/2016-February/032051.html)

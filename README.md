# Wie man einen VPN mit Hilfe von Docker Container aufsetzt
Verwendete Komponenten:
- Docker
- Portainer
- Wireguard
- Dyndns
- Domäne (von Strato)

# Als erstes das System updaten
```
sudo apt update
sudo apt upgrade
sudo reboot
```

# Docker installieren
`curl -sSL https://get.docker.com | sh`

## Sicherstellen, dass der User Docker Container ausführen darf
`sudo usermod -aG docker $USER`

Anschließend mit dem User neu einloggen!

# Portainer installieren
```
sudo docker pull portainer/portainer-ce:latest
sudo docker run -d -p 9000:9000 -p 9443:9443 --name=portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:latest
```
## Einstellungen
In Portainer (`http://<ip-des-servers>:9000`) bei `Environments -> local` die IP festlegen, damit beim Klick auf den Port des jeweiligen Containers direkt die richtige Adresse göffnet wird.

# (Sub)Domäne erstellen
Die Domäne bzw Subdomäne die verwendet werden soll, muss beim jeweiligen Anbieter erstellt werden: zB vpn.meinedomaene.de

Außerdem muss DynDNS für die Domäne aktiviert werden (**Achtung:** Die Aktivierung kann je nach Anbieter **24h dauern!**)

# DynDNS Container
In Portainer einen neuen Stack anlegen, die [dyndns-compose.yaml](dyndns-compose.yaml) einfügen und den Stack deployen.
 
Für Infos siehe das Video [Docker: Wireguard VPN Server leicht gemacht.](https://www.youtube.com/watch?v=awWwU4w1Unw).

## Testen
Über `nslookup <meine-vpn-domaene>` kann man herausfinden ob das DynDNS geklappt hat. Denn genau dann, wenn die IP die nslookup liefert der öffentlichen IP des Servers entspricht.

# Wireguard VPN Container
In Portainer einen neuen Stack anlegen, die [wireguard-compose.yaml](wireguard-compose.yaml) einfügen und den Stack deployen.

Für Infos siehe [GitHub von HoekieN/docker-dynamic-dns](https://github.com/HoekieN/docker-dynamic-dns).

## Portweiterleitung
Anschließend muss im Router noch eine Portweiterleitung eingerichtet werden, damit der Traffic an den Port 51820 (siehe [wireguard-compose.yaml](wireguard-compose.yaml)) auch an die IP des Wireguard Server weitergeleitet wird.

Hier am Beispiel einer Fritzbox:
- http://192.168.178.1 aufrufen und anmelden
- Wechsel auf `Internet > Freigaben > Portfreigaben`
- `Gerät für Freigaben hinzufügen` und das jeweilige Gerät auswählen
- `Neue Freigabe > Portfreigabe`

![grafik](https://github.com/MatthiasOs/docker-vpn/assets/36775764/89430db7-c03a-4bf9-a12d-ac0785fce997)

## Einstellungen
Die IP des Servers mit dem Wireguard Port 51820 öffnen, ggf mit dem festgelegten Passwort anmelden (siehe [wireguard-compose.yaml](wireguard-compose.yaml)).
Neue VPN Verbindung Anlegen und auf dem Client Gerät die Verbindung registieren: zB über die Wireguard App auf dem Smartphone und dann den QR Code von der Seite abscannen.

# Shell in a Box
Damit man über die Browser (ohne zB Putty) per SSH auf den Server zugreifen kann, kann man `shellinabox` wie [hier](https://youtu.be/cO2-gQ09Jj0?si=xu-OL-PZgERREs1b&t=711) beschrieben installieren.
`sudo apt install shellinabox`

Anschließend ist über https://<lokale.ip.des.servers>:4200 die shell zu erreichen (Achtung: http**s**)
 
 # Quellen
 - [Docker: Wireguard VPN Server leicht gemacht.](https://www.youtube.com/watch?v=awWwU4w1Unw)
 - [Pi-Hosted : Raspberry Pi 4](https://www.youtube.com/watch?v=cO2-gQ09Jj0&list=PL846hFPMqg3jwkxcScD1xw2bKXrJVvarc)

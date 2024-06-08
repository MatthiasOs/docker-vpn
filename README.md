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
In Portainer bei Environments -> local die IP festlegen, damit beim Klick auf den Port des jeweiligen Containers direkt die 

# (Sub)Domäne erstellen
Die Domäne bzw Subdomäne die verwendet werden soll, muss beim jeweiligen Anbieter erstellt werden: zB vpn.meinedomaene.de

Außerdem muss DynDNS für die Domäne aktiviert werden (**Achtung:** Die Aktivierung kann je nach Anbieter **24h dauern!**)

 # DynDNS Container
 In Portainer einen neuen Stack anlegen.

 # Shell in a Box
 Damit man über die Browser (ohne zB Putty) per SSH auf den Server zugreifen kann, kann man `shellinabox` wie [hier](https://youtu.be/cO2-gQ09Jj0?si=xu-OL-PZgERREs1b&t=711) beschrieben installieren.
`sudo apt install shellinabox`

Anschließend ist über https://<lokale.ip.des.servers>:4200 die shell zu erreichen (Achtung: http**s**)
 
 # Quellen
 - [Docker: Wireguard VPN Server leicht gemacht.](https://www.youtube.com/watch?v=awWwU4w1Unw)
 - [Pi-Hosted : Raspberry Pi 4](https://www.youtube.com/watch?v=cO2-gQ09Jj0&list=PL846hFPMqg3jwkxcScD1xw2bKXrJVvarc)

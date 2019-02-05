# Tracking WIFI #

## Introduction ##

L'avènement des appareils communiquant induit un important transit d'informations sur les ondes radios. Si une partie de ses informations sont chifrées, il reste toujours des données non chiffrés tel que l'adresse MAC.

Le tracage WIFI est partout. Des entité commerciales collectent ces signaux pour mesurer, tracer et profiler des foules. Les forces de l'ordre s'en servent pour retrouver des appareils volés. Depuis les révélations Snowden, on sait que la NSA fait usage du traçage Wi-Fi, et que l'adresse MAC fait partie des 'selectors' de son fameux logiciel PRISM.

## Adresse MAC ##

L'adresse MAC (Medium Access Control) est un identifiant qui assure le partage d'un medium entre plusieurs machines. Elle est composé de 48 bits, divisé en deux parties :
* OUI (Organisation Unique Identifier)
  Partie haute de l'adresse, c'est l'identifiant du constructeur de la carte. Il est attribué par l'IEEE.
* NIC (Network Interface Controler
  Est l'identifiant propre de la carte.

![schema_mac_address](img/1024px-MAC-48_Address.svg.png)

## Pratique ##

### Tracage Wi-Fi ###

#### Méthode 1 - airmon-ng ####

Avant toute chose, la capture des trames Wi-Fi nécessite une interface Wi-Fi en mode monitor.
```bash
sudo iwconfig wlan0 down
sudo iwconfig wlan0 mode monitor
sudo iwconfig wlan0 up
```

Puis nous allons lancé **airmon-ng**:
```bash
sudo airmon-ng wlan0
```

Puis taper trois fois sur la touche [a] pour passer le mode d'affichage en mode STA, c'est à dire que seules les stations seront affichées. Les infos sont:
* L'adresse MAC
* L'indicateur de réception RSSI
* Le SSID

#### Méthode 2 - tshark ####

On peut aussi utiliser tshark qui est la version CLI de wireshark:
```bash
tshark -i wlan0 -Y "wlan.fc.type_subtype == 4" -T fields -e wlan.sa -e radiotap.dbm_antsignal -e wlan_mgt.ssid
```
On filtre les trames de type **probe requests** grace au filtre *wlan.fc.type_subtype == 4*.
Puis on affiche la MAC, le RSSI, et le SSID.

#### Méthode 3 - tcpdump ####

```bash
sudo tcpdump -i wlan0 -e type mgt subtype subtype probe-req
```

### Fabriquant de la carte ###

Les adresses MAC contienne l'OUI, l'identifiant du fabriquant. Executer le script oui_resolver.sh dans le même repertoire que le oui.txt. Passer l'adresse MAC en argument.

### Détection de présence ###

Réveiller un ordinateur sur commande lorsque l'on détecte l'adresse MAC. Il faut le paquet wakeonlan et tcpdump.
```bash
apt-get install wakeonlan[ou wol] tcpdump
```

CLI:
```bash
sudo tcpdump -l -e -i wlan0 | grep adresse_mac_a_detecter | ./wol.pl
```
L'option -l désactive le buffering, ce qui permet d'éviter la détection du telephone. -e pour afficher les adresse MAC

## Protection contre le traçage ##

### Adresse MAC aléatoire ###

Nous pouvons générer des adresse MAC et les utiliser.

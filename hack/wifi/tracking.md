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

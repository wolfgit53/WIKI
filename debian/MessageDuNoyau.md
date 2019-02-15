# Lire la sortie des services activés au démarrage #

## Introduction ##
Les messages qui sont affichés sur la console du système lors du démarrage sont générés par:
* Le noyau
* Les programmes qui configurent le système et les applications qui démarrent

## Les messages du noyau ##
Le noyau enregistre dans le un tampon **/var/log/dmesg**, que l'on peut afficher:
```bash
dsmeg
```

Les options notables:
* -H Human readable
* -T Human time

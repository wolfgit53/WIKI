# Installation de logiciels sous linux #

## dpkg ##

La commande **dpkg** est chargé de l'installation, la création, la suppression et la gestion des paquets Debian.

### Installation, mise à jour et suppression ###

#### Installation ####

Installer un paquet au format .deb
```bash
dpkg -i paquet.deb
```

Installer les paquets d'un répertoire:
```bash
dpkg -R folder
```

#### Suppression ####
Supression
```bash
dpkg -r zip
```
Purge
```bash
dpkg -P apache
```

#### Lister les paquets ####
Lister les paquets
```bash
dpkg -l
```

Avec un système de filtrage:
```bash
dpkg -l "apt*" | grep ^ii
```

#### Trouver un paquet contenant un fichier ####
```bash
dpkg -s /usr/bin/basename
```

#### Lister le contenu d'un paquet ####
Lister le contenu d'un paquet
```bash
dpkg -L paquet
```


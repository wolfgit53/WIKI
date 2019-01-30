# GESTION DES PAQUETS #

## Anatomie d'un paquet binaire ##

Un paquet binaire debian est constitué d'un unique fichier DEB. Le nom d'un paquet se divise en 3 parties séparé par un underscore:
* Le nom
* Le numéro de version
* L'architecture matérielle pour laquelle il à été compilé

Exemple: postfix_2.1.5-9_i386.deb :
* postfix
* 2.1.5-9
* i386

## Disection d'un paquet binaire ##

Un paquet .deb peut être ouvert et manipuler. Il s'agit en réalité d'une archive **BSD ar**.
Pour ouvrir le contenu :

```bash
ar t postfix_2.1.5-9_i386.deb
debian-binary
control.tar.gz
data.tar.gz
```

Ce sont trois fichiers distinct qui compose l'archive .deb :
* debian-binary
Ce fichier ne sert que de marqueur pour reconnaitre l'archive comme un paquet Debian. IL contient le numéro de version du format de paquet utilisé.
* control.tar.gz
Ce fichier est une archive tar compressé par gzip, et contient les informations de contrôle utilisées par les outils de gestion de paquets.
* data.tar.gz
Contient les fichiers installés par le paquet sur le système.

Ilustration :
```bash
cp /var/spool/apt/archives/postfix_2.1.5-9_i386.deb .
ar x postfix_2.1.5-9_i386.deb
cat debian-binary
  2.0
tar tzf control.tar.gz
  config  templates shlibs postinst preinst
  prerm   postrm    conffiles md5sums control
tar tzf data.tar.gz
  ./usr/sbin/postfix
  [...]
```

## Les fichiers de contrôle ##

* control
contient les métadonnées utilisées par les outils de gestion de paquets pour afficher les informations sur le paquet et de s'assurer que les dépendances sont stisfaites avant de tenter une installation. Fichier le plus important.
* conffiles
Tout les fichiers listés dans ce fichiers sont décrits avec leur chemin d'accès complet.
* preinst
Le script preinst est invoqué avant toute tentative d'installation ou de MAJ. Le fichier de l'ancienne version du paquet est invoqué en cas d'échec de mise à jour, et rétablir une configuration.
* postinst
Le script postinst est invoqué lors de la configuration d'un paquet, apres le désarchivage des fichiers contenus dans le paquet. Il est invoqué aussi lors d'une erreur durant une mise a jour, ou de suppression de paquet.
* prerm
Executer avant la suppression du paquet.
* postrm
Invoquer apres la suppression du paquet, avec l'info de si on purge ou on supprime le paquet.
* md5sums
Contient les signatures MD5 de tous fichiers installé par le paquet. Pour vérifier leur intégrité.
* shlibs
Contient la liste des lib fournies par le paquet. Pour savoir d'où provient la version donnée d'un bibliothèque. Cela permet de gérer en partie les dépendances.
* config
Permet de poser les questions à l'utilisateur, afin de collecter des infos relatives aux paramètre de configuration. Elle sont stocké dans la base de donnée de **debconf**.


## Au contact des binaires ##

DPKG est un gestionnaire de paquets. Il gère :
* l'installation
* configuration
* suppression
* suivie des paquets installés
Il fournit aussi une série d'outils pour gérer les paquets.

DPKG est un outil très fiable. Avant de lancer une action DPKG vérifie que cela ne va pas créer d'incohérence entre l'état réel du system et celui décrit par sa base de données. En cas de problème l'action sera bloqué plutot que permise.
Mais il a un champs de vision limité du fait qu'il traite les paquets individuellement.

## DPKG et sa famille ##

DPKG contient en réalité deux autres programmes : dpkg-deb et dpkg-query.
* **dpkg** installe et supprime des paquets sur un système
* **dpkg-deb** permet de manipuler les paquets
* **dpkg-query** fournit un accès à la base de donnée de dpkg.
Mais **dpkg** fournis l'interface avec les deux autres programmes. Qui peuvent être également lancé séparément.

## Manipulation de paquets binaires ##

Afficher les informations de contrôle de paquet :
```bash
dpkg-deb --info postfix_2.1.5-9_i386.deb
```
Il est possible de filtrer les champs obtenu dans le retour de la commande précédente avec l'option *--field*:
```bash
dpkg-deb --field postfix_2.1.5-9_i386.deb Version
  2.1.5-9
```

Afficher la liste des fichiers qui s'installe avec le .deb :
```bash
dpkg-deb --contents postfix_2.1.5-9_i386.deb
  drwxr-xr-x root/root      0 2005-03-11 02:05:22 ./var/spool/postfix/
  [...]
```

Extraire les données de contrôle d'un .deb :
```bash
dpkg-deb --control postfix_2.1.5-9_i386.deb
```
Extraire les fichiers installables :
```bash
dpkg-deb --extract postfix_2.1.5-9_i386.deb
ls -F *

DEBIAN:
conffiles control postinst* preinst* shlib [...]

etc:
init.d/ network/ postfix/
usr:
bin/ lib/ sbin/
var:
log/ spool/
```

Il est aussi possible de faire le chemin inverse, de creer une archive .deb :
```bash
mkdir pfpkg
mv --target-directory=pfpkg DEBIAN etc usr var
dpkg-deb --build pfpkg
  dpkg-deb : construction du paquet "postfix" dans "pfkg.deb".
file pfkg.deb
pfpkg.deb: Debian binary package (format 2.0)
dpkg-deb --field pfpkg.deb Version
  2.1.5-9
```

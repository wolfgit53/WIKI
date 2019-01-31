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

## Installation des paquets ##

Installer un paquet .deb :
```bash
dpkg --install postfix_2.1.5-9_i386.deb
```
### Etape d'une installation ###

#### Extraction ####
La première étape est l'extraction réalisé par dpkg-deb, dpkg se contente de diriger la procédure :
```bash
dpkg --unpack postfix_2.1.5-9_i386.deb
```

1. Verifie que c'est bien un paquet Debian. Si oui l'information de contrôle est extraite et stockée de manière temporaire dans /var/lib/dpkg/temp.ci.
2. Si preinst existe il est invoqué. Par exemple certain paquet arrète des service pendant l'installation.
3. dpkg récupère dans le fichier de contrôle conffiles une liste de fichiers pour lesquels le traitement est spécial. Ces fichiers sont extrait dans un emplacement temporaire, puis dans la destination finale.
4. dpkg extrait ensuite le reste (data.tar.gz) vers le système de fichiers.
5. Puis les fichiers de contrôle sont envoyé dans le répertoire /var/lib/dpkg/info.
6. Puis dpkg marque le paquet comme 'dépaqueté' dans sa base de donnée.

#### Configuration ####
Une fois installer la phase de configuration arrive.
Manuellement on peut le faire avec :
```bash
dpkg --configure postfix
```

1. DPKG demande le comportement à avoir auprès de l'administrateur. Ecraser un fichier local, remplacer par le fichier nouveau du .deb, soit de garder le fichier local en l'état.
2. postinst est exécuté si il existe. Si le paquet utilise la base de debconf il va y avoir intéraction avec l'utilisateur pour connaitre des paramètre de config. Mais debconf peut avoir déjà parametré pour répondre au questions.
3. postinst doit apporter les modification au système si besoin, niveau périphérique par exemple.
4. dpkg met à jour sa base de données:  le paquet est marqué installé

Pour configurer tout paquet marqué 'dépaqueté' et non 'installé' on peut exécuter :
```bash
dpkg --configure -a
```

### Intéraction avec la base de données des paquets ###

**DPKG** garde en mémoire tout paquets sur le système, même ceux supprimer et désinstallé par le passé.
Cette base de donnée se trouve dans : '/var/lib/dpkg'.

Les infos stockées :
1. L'état du paquet (fichier status)
2. Informations si le paquet est installé, ou supprimé (fichier status)
3. La base de données des alternatives (fichier alternatives).
4. La base de données des surcharges de permissions (fichier statoverride).
5. Dans le sous répertoire **info/** la liste de tous les fichiers installés par le paquet,et celle des conffiles. Contient également les 4 script \*rm \*inst.
6. Parfois les md5
7. Les infos lié à debconf
8. La liste des paquets dispo.

Cette organisation en fichiers plats est la principale cause de lenteur de **dpkg**. 

#### Manipulation de la base ####

Afficher la liste de tout les paquets :
```bash
dpkg --list
  Souhait=inconnU/Installé/suppRimé/Purgé/H=à garder
| État=Non/Installé/fichier-Config/dépaqUeté/échec-conFig/H=semi-installé/W=attend-traitement-déclenchements
|/ Err?=(aucune)/besoin Réinstallation (État,Err: majuscule=mauvais)
||/ Nom                                   Version                                     Architecture Description
+++-=====================================-===========================================-============-===============================================================================
ii  accountsservice                       0.6.43-1                                    amd64        query and manipulate user account information
ii  acl                                   2.2.52-3+b1                                 amd64        Access control list utilities
ii  adduser                               3.115                                       all          add and remove users and groups
```

La première colonne comporte deux lettres :
La première lettres :
**i**
L'utilisateur a requis l'installation du paquet.
**r**
L'utilisateur a requis la suppression du paquet.
**p**
La purge
**h**
L'utilisateur a spécifier que ce paquet devait être figé (gardé dans sa version actuelle) et que aucune mise à jour ne peut inpacter.

La seconde lettre:
**n**
paquet non installé
**i**
paquet completement installé et configurer
**c**
installé puis supprimer, mais on a garder ses conffiles
**u**
depaqueté mais pas configuré
**f**
la configuration a échoué.
**h**
Le paquet a été installé mais l'installation ne s'est pas terminé correctement

Troisième lettre :
**h**
Le paquet est figé de manière forcé, car il constitue la dépendance d'un paquet mannuellement figé.
**r**
Paquet hors service
**x**
Paquet figé et cassé de manière forcée.

Inspection de paquets individuels :
```bash
dpkg --list ssh
```

Recherche dans la liste des fichiers installés
```bash
dpkg-query --search /usr/lib/postfix/*d
```
Mais on peut aussi utiliser la commande **dlocate**:
```bash
dlocate -lsconf postfix
```
Afficher en plus les MD5 par exemple :
```bash
dlocate -md5check postfix
```
Suppression des paquets :
Il existe deux manière de supprimer un paquet:
* suppression
On desinstalle le logiciel en gardant les fichiers de conf:
```bash
dpkg --remove postfix
```
* purge
```bash
dpkg --purge postfix
```
On supprime tout même les fichiers de configurations.

## Gestion des paquets : APT ##

### L'outil ###

Le problème de DPKG est la gestion des dépendances. C'est pour cela que l'on utilise APT (Advanced Package Tool). C'est un outils de haut niveau qui voit les paquets de manière abstraite. Il voit que :
* un nom qui identifie le paquet
* un num de version
* une adresse URL pour le fichier DEB
* des informations de dépendances

APT ne traite pas directement l'installation et la suppression des paquets, mais délègue ces tâche a DPKG. Il gère simplement l'acquisition des dépendances, et l'orchestration des appels à DPKG.
APT garde un liste des paquets disponibles, que l'admin met à jour avec ```bash apt update ```. A la suite, APT compile les informations réunis dans les paquets, et en déduis un arbres des dépendances en utilisant la théorie des graphes

### Spécifier une source de paquets ###

URI source :
* **cdrom** permet d'accéder à des paquets stockés sur des lecteurs de CD-Rom locaux.
* **file** permet d'utiliser un emplacement quelconque du système de fichier.
* **copy** comme file mais copie dans le répertoire cache d'APT pour les stocker après l'acquisition.
* **http** la methode la plus rapide et la plus recommandée.
* **ftp** légèrement moins rapide que **http**, elle est néamoins plus flexible par la configuration à travers /etc/apt/apt.conf.
* **ssh** déconseillé pour des raisons de sécurité.

### Configuration d'APT ###

APT peut être en grande partie configuré grace au fichier **/etc/apt/apt.conf**.
Cela fonctionne avec un système de clé valeur, dont la syntaxe est expliqué dans apt.conf.

```bash
APT::Cache-Limit 16777216;
APT::Ge::Show-Upgraded true;
APT::Get::Default-Release "sarge";
APT::Get::Purge false;
Acquire::Queue-Mode host;
Acquire::Retries 0;
```

Dans /etc/apt/apt.conf on peut spécifier le comportement de DPKG. Par exemple pour ne jamais remplacer un paquet installé par une version plus ancienne, et de ne jamais réinstaller la version actuellement installée le code à insérer est:
```bash
DPkg {
  Options { "--refuse-downgrade"; "--skip-same-version"; }
  };
```

Crochets d'APT :
On peut spécifier des commandes à effectuer à des moments précis. Par exemple sur certain système, /usr est monté en lecture seule. Pour réactiver l'écriture avant l'invocation de dpkg et pour la redésactiver après :
```bash
DPkg {
  Pre-Invoke { "mount -o remount,rw /usr"; };
  Post-Invoke { "mount -o remount,ro /usr"; };
};
```
Il existe aussi Pre-Install-Pkgs, qui invoque ses commandes avant même celle de Pre-Invoke.

### Utilisation d'APT ###

Installer un paquet:
```bash
apt-get install postfix
```

Rechercher un paquet:
```bash
apt-cache search postfix
```

Afficher les infos d'un paquet:
```bash
apt-cache show postfix
```

Rechercher un fichier spécifique:
```bash
apt-file search /usr/bin/convert
```

Afficher les dépendances entre paquets:
```bash
apt-cache depends cmatrix
```

Réinstaller un paquet:
```bash
apt-get install --reinstall postfix
```
Suppression de paquet en gardant les confs:
```bash
apt-get remove postfix
```

Suppression total:
```bash
apt-get remove --purge postfix
```

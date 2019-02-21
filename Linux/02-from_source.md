# Installer depuis les sources #

En général, les sources sont obtenus dans des archives provenant des sites de developpement. Ces archives contiennent:
* Le code source sous forme de fichier
* Parfois un makefile permettant d'automatiser la compilation du logiciel
* Souvent un .configure permettant de générer un makefile adapté au PC

## Pré-requis et dépendances ##
Installer:
* outil **make**
* compilateur **gcc**
* dépendances tel que les bibliothèques

## Installation ##
Décomprésser l'archive:
```bash
tar xvf archive.tar.bz2
```

Puis ce placer dans le dossier créer par la décompression:
```bash
cd archive_folder/
```

Executer le fichier *.configure*:
```bash
./configure
```

./configure permet de fournir les dépendances et va fournir le makefile. Ensuite il faut télécharger les dépendances si il en manque:
```bash
apt install biblio biblio2
```

Puis lancer la compilation avec la commande **make**:
```bash
make
```

Enfin finir en installant le produit avec **make install**:
```bash
su -c "make install
```

On peut aussi desinstaller:
```bash
su -c make uninstall
```

## Les bases du Makefile ##

## Gérer les bibliothèques ##

### Principe ###
Une lib partagée est un fichier qui contient une liste de fonctions accessible à tout programme en ayant besoin sans avoir à les réécrire. Un ensemble de fonctions proposées par une ou plusieurs bibliothèque partagées forme une API.

Un lien est établi entre le programme et une bibliothèque partagée lors de l'étape de l'édition des liens par l'éditeurs de liens **ld**, lui même appelé par le compilateur gcc avec l'option **-l** **\<lib>**.

Sous linux, les lib partagées sont appelées des **Shared Objects** (so) dans le sens où il s'agit de fichiers objets sans bloc d'instruction **main**. Il portent le suffixe **.so**.

### Lieu de stockage ###
- /lib : lib systèmes de base, vitales
- /usr/lib : lib utilisateur de base, non nécessaire au boot
- /usr/local/lib : lib locales aux produits pour la machine

La lib la plus importante est la **libc**.

### Lien avec programme ###
On peut afficher les lib utilisé par un programme avec la commande **ldd**
```bash
ldd /usr/bin/diff
```

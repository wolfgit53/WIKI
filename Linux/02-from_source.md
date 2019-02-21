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

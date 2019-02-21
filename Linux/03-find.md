# Chercher des fichiers #

## find ##

### La commande ###
La syntaxe:
```
find chemin critère options
```

### Critère de recherche ###
1. -name
Chercher un fichier par son nom:
```bash
find . -name "fic*" -print
```

2. -type
Par type de fichier:
* B : Fichier spécial en mode bloc
* C : Fichier spécial en mode caractère
* D : Directory
* F : Fichier ordinaire
* L : Lien symbolique
* P : Tube (pipe)
* S : Socket (Connexion réseau)

```bash
find . -name "re*" -type d -print
```

3. -user et -group
Par apparenance de groupe ou utilisateur:
```bash
find . -name "re*" -type f -user wolf -group sudo -print
```

4. -size
Par la taille du fichier:
-size 152c : fichiers d'une taille de 152 octets
-size 10k  : fichiers d'une taille de 10 Ko
-size +5000k : fichiers de plus de 5000 Ko
-size -100k : les fichiers de moins de 100 Ko

5. -atime, -mtime, -ctime
-atime : date du dernier accès (lire, ou simplement un ls du repertoire)
-mtime : dernière modification de contenu
-ctime : rechercher sur la date de changement (changement de numéro d'inode)

exemple:
-mtime 1  : fichier modifié hier (entre 24 et 48 heures)
-mtime -3 : fichier modifié il y a moins de trois jours
-atime +4 : fichier modifié il y à plus de 4 jours

6. -perm
Recherche sur la valeur de permission:
```bash
find . -type d -perm -111
```

7. -links et -inum
-links recherche par le nombre de hard link.

-inum par numéro d'inode
```bash
ls -i
[nb]

find . -inum [nb] -print
```

### Commandes ###

1. -exec
```bash
find . -type f -name "*.mp3" -exec rm -f {} \;
```

2. -ok
pareil que -exec mais le shell demande confirmation à l'utilisateur

3. 

# Représentation des disques #

## Manipulation de bas niveau ##
La commande **hdparm** avec le paramètre **-I** permet d'afficher les caractéristiques d'un disque:
```bash
hdparm -I /dev/sda
```

Les différents paramètres possible:
* **-c** : largeur du bus de transfert EIDE sur 16 ou 32 bits. 0=16, 1=32, 3=32 compatible.
* **-d** : utilisation du DMA. 0=pas de MDA, 1=DMA activé
* **-x** : modifie le mode DMA.
* **-C** : statut de l'economie d'énergie sur le disque
* **-g** : afficher la géométrie du disque
* **-M** : indique ou modifie l'état du Automatic Acoustic Management (AAM) 0=off, 128=quiet, 254=fast
* **-r** : passe le disque en lecture seule.
* **-T** : bench de lecture du cache disque, idéal pour tester les performances de transfert entre linux et le cache du disque. Relancer la commande 2-3 fois.
* **-t** : bench de lecteur du disque, hors cache. 2-3 relance.

## Choisir un système de fichiers ##

### 1. Principe ###

#### Les méta-données ####
Un fichier décrit par des propriétés appelées les méta-données. Sous Linux, il s'agit de l'inode. Le contenu (les données) est placé dans d'autres blocs du support de stockage. On retrouve sous Linux:
* les droits
* les dernières dates d'accès et de modification
* le propriétaire et le groupe
* la taille
* le nombre de blocs utilisés
* le type de fichiers
* le compteur de liens
* un arbre d'adresses de blocs de données

#### Les noms des fichiers ####
Les noms sont limité à 255 caractères.
L'extension n'a pas d'existence en tant que composante du système de fichiers
Le type est déterminé par le contenu, les premiers octets

### Les filesystems sous Linux ###

#### ext2 ####
- rapide
- requiert moins d'écriture
- usure amoindrit des disques
- taille max fichier = 2To
- taille max partition = 32 To
- pas journalisé

#### ext3 ####
- journalisé
- souple
- taille max fichier = 2 To
- taille max partition = 32 To

#### reiserfs ####
- journalisation efficace
- organisation indexée des entrées des répertoires
- manipulation des fichiers de petite taille
- performance exceptionnelles avec de milliers de petit fichiers
- performance faible avec des moyens fichiers
- redimensionnable à chaud
- lent sur les gros fichiers
- taille max fichier = 8 To
- taille max partition = 16 To

### Partitionnement ###

#### Organisation d'un disque ####

##### MBR #####
**M**aster **B**oot **R**ecord
Taille de 512 octets
Programme d'amorçage sur 444 octet


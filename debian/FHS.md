# Le FHS (File Hierarchy Standard) #

La section 9.1.1 de la chartre de Debian stipule que "l'emplacement de tous les répertoires et fichiers installés doit être conforme au standard de la hiérachie du système de fichiers FHS.

![FHS](img/FHS.png)

** / - Le répertoire racine **
Aucun fichier ou répertoire, autres que ceux déjà présents, ne doit être placé dans le répertoire le plus haut niveau. Cependant on peut y placer des liens symbolique vers des fichiers utilisés pour amorcer le noyau.

** /etc - Configuration spécifique à l'hôte **
Tout fichier que l'administrateur est censé pouvoir modifier directement sur un système doit être placé dans /etc.

** /usr - Données en lecture seule partageables **

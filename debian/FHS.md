# Le FHS (File Hierarchy Standard) #

La section 9.1.1 de la chartre de Debian stipule que l'emplacement de tous les répertoires et fichiers installés doit être conforme au standard de la hiérachie du système de fichiers FHS.


**/ - Le répertoire racine**
Aucun fichier ou répertoire, autres que ceux déjà présents, ne doit être placé dans le répertoire le plus haut niveau. Cependant on peut y placer des liens symbolique vers des fichiers utilisés pour amorcer le noyau.

**/etc - Configuration spécifique à l'hôte**
Tout fichier que l'administrateur est censé pouvoir modifier directement sur un système doit être placé dans /etc.

**/usr - Données en lecture seule partageables**
Les fichiers situé sous /usr ne doivent pas exiger de permission d'écriture, et doivent être statiques par nature. En-dehors des phase de mise à jour, le système doit pouvoir fonctionner normalement si /usr est sur un volume en lecture seule. Il ne doit pas y avoir de fichiers ou de répertoires placés directement dans /usrs.

**/usr/lib - Ressources spécifiques à une architecture**
Les fichiers requis par des logiciels locaux et qui dépendent de l'architecture matérielle (principalement des binaires) seront placé ici.

**/usr/share - Ressources indépendantes de l'architecture**
On trouve ici les fichiers requis par les logiciels installés, mais qui peuvent être utilisés tels quels indépendamment de l'architecture matérielle.

**/usr/local - Logiciels et ressources personnalisés**
Les logiciels et ressources installés indépendamment de la distribution Debian peuvent être placés dans /usr/local, et n'y seront pas dérangé par le système Debian.

**/tmp - Données temporaires**
Ce "Bac à sable" peut contenir des données de toutes sortes. Leur persistance n'est pas garantie d'un processus à l'autre.

**/var - Données transitoire**

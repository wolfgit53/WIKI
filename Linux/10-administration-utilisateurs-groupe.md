# Administration des utilisateurs #

## Général ##

### Les utilisateurs ###
Les utilisateurs est l'association d'un **nom**, à un **UID**, et au moins un **GID**

### Les groupes ###
Un utilisateur fait partie d'au moins un groupe. Il peut également y avoir des groupes secondaire.

Pour connaitre les informations par rapport à un utilisateur nous avons la commande **id**.

### Les mots de passes ###
Chiffrer avec MD5, DES.

## Les fichiers ##

### /etc/passwd ###

Login:password:UID:GID:comment:homedir:shell

Login = nom d'utilisateur pour se logger
Password = mot de passe
UID = User IDentifer
GID = Group IDentifer
Comment = Commentaire ou descriptif
homedir =  chemin du repertoire personnel
shell = le shell par défaut de l'utilisateur. Peut être n'importe quelle commande

### /etc/group ###

Group:password:GID:user1,user2,...

Group = nom du groupe
Password = Mot de passe
GID = Group Identifer
userX = la liste des utilisateurs faisant partie du groupe

### /etc/shadow ###
C'est le fichier qui contient les mots de passe.

Une ligne contient 9 champs:
1 = le login
2 = le mot de passe crypté
3 = nb de jour depuis le dernier changement du mot de passe
4 = nb de jour avant lequel on ne peut changer le mot de passe. 0 pour pouvoir le changer quand on veut
5 = nb de jour apres lequel le mot de passe doit être changé
6 = nb de jour avant l'expiration du mot de passe durant lequel l'utilisateur doir être prévenu
7 = nb de jour après l'expiration du mot de passe après lequel le compte est désactivé
8 = nb de jour depuis le temps que le compte est désactivé
9 = réservé

# Kernel #

## Log du système ##
La commande **dmesg** affiche les logs du système:
```bash
dmesg
```

Puis nous avons aussi le fichier **/var/log/messages**

## Noyau ##
Le noyau est le coeur du système. Il est placé dans **/boot**. Pour obtenir sa version:
```bash
uname -r
  4.9.0-8-amd64
```

4 -> La version majeure du noyau
9 -> version developement car impaire, paire = version stable
0 -> version mineure, petit changement, patchs

uname:
* **-m**achine Type de matériel (x86_64)
* **-n**odename Nom de la machine (stvictoire)
* **-r**elease Version du noyau
* **-s**ysteme Système (Linux)
* **-p**processor Type de processeur (unknow)
* **-v**ersion

## Gestion des modules ##
Les modules se trouve dans **/lib/modules/$(uname -r)**
.ko pour kernel object

Pour voir les modules actuellement chargés:
```bash
lsmod
```
1 colonne : nom
2 colonne : la taille
3 colonne : le nombre de composant système qui utilise le module
4 colonne : dépand de quel autre module

La commande modinfo donne les informations sur n'importe quel module dont le nom est fourni en paramètre:
```bash
modinfo vfat
```

Pour charger un module sans penser aux dépendances **modprobe**:
```bash
modprobe vfat
```
Et pour le décharger:
```bash
modprobe -r vfat
```

Dans le fichier **/etc/modprobe.conf** nous pouvons configurer les modules à charger

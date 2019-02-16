# Utiliser le terminal de commande #

## Statut du shell ##
Le dollar $, signifie que nous sommes loggé en tant que simple utilisateur lambda
Le dièse  #, signifie que nous somme l'utilisateur root, qui est le full administrateur de la machine

## Raccourcis dans le shell ##

### Naviguation dans la ligne de commander ###

|  Ctrl + F(orward) | avancer d'un charactère |
|:-----------------:|------------------------:|
| Ctrl + B(ackward) | reculer d'un charactère |
|  Alt  + F(orward) |     avancer d'un mot    |
| Alt  + B(ackward) |     reculer d'un mot    |
| Ctrl + A          | début de la ligne       |
| Ctrl + E(nd)      | fin de la ligne         |
| Ctrl + L (cLear)  | effacer le terminal     |

### Editions de la ligne de commande ###

|       Ctrl + D      |                 effacer le char actif (SUPPR)                |
|:-------------------:|:------------------------------------------------------------:|
|      Backspace      |                   effacer le char précédent                  |
|  Ctrl + T(ranspose) |   switch le char actif avec le précédent, puis curseur + 1   |
|  Alt  + T(ranspose) |                    switch les mots proches                   |
|  Alt  + U(ppercase) |   met en majuscule le mot courant à partir du char courant   |
|  Alt  + L(owercase) |   met en minuscule le mot courant à partir du char courant   |
| Alt  + C(apitalize) | met en majuscule le 1er char du mot a partir du char courant |
|    Ctrl + K(Cut)    |             Coupe la ligne apres le char courant             |
| Ctrl + U(Cut begin) |             Coupe la ligne avant le char courant             |
|       Ctrl + W      |                 Coupe le mot avant le curseur                |
|       Alt  + D      |                     Coupe le mot suivant                     |
|       Ctrl + Y      |                    Coller le texte récent                    |
| Alt  + Y            | Coller le texte précédent                                    |
| Ctrl + C            | Efface la ligne                                              |

## Personnaliser le prompt du shell ##

### Caractère du prompt ###
Le prompt du shell est afficher à chaque attente de commande. Il obtiens son contenu de la part de la variable d'environnement PS1. Mais c'est une variable comme une autre que l'on peut éditer. Le promt de base de Debian est obtenu avec la variable suivante: PS1="\u@\h:\w$ "

* "\!" Cela affiche le numéro de prompt shell. Incrementé par chaque commande rentré et executé.
* "\#" Le diese affiche le numéro de commande. Seulement pour la session active.
* "\$" Affiche soit $ soit #, en fonction de l'utilisateur.
* "\W" Affiche le repertoir courant. Si on est dans '/etc/dhcp/dhcp-exit-hooks.d/' il affiche 'dhcp-exit-hooks.d'
* \\ Juste afficher un backslash
* \d Ceci affiche la date, ex: 'mar. janv. 22'
* \h Affiche le hostname de la machine.
* \n Cause un retour à la ligne, c'est rigolo^^
* \nnn Affiche le charactère à partir de l'octal. Ex: "\056" => '.'
* \s Affiche le nom du shell
* \t Affiche l'heure
* \u Afficher l'utilisateur courant
* \w Afficher le chemin complet du repertoire courant

* \[ Les caractère non imprimable.Tel que la couleur.
* \] Fin des chars non imprimable
*Exemple:
\[\033[1;34m\][\$(date +%H%M)][\u@\h:\w]$\[\033[0m\]*

### Couleurs ###
| Black      | 0;30 | Dark Gray    | 1;30 |
|------------|------|--------------|------|
| Blue       | 0;34 | Light Blue   | 1;34 |
| Green      | 0;32 | Light Green  | 1;32 |
| Cyan       | 0;36 | Light Cyan   | 1;36 |
| Red        | 0;31 | Light Red    | 1;31 |
| Purple     | 0;35 | Light Purple | 1;35 |
| Brown      | 0;33 | Yellow       | 1;33 |
| Light Gray | 0;37 | White        | 1;37 |

## Historique des commandes ##

Linux enregistre toutes les commandes executées. Et leur attribut un numéro qui s'incrémente à chaque nouvelle commande.

* Taper la commande qui suit, pour afficher les 8 dernières commandes rentrée dans le shell :
$ history 8
382 date
383 ls /usr/bin | sort -a | more
384 man sort
385 cd /usr/local/bin
386 man more
387 useradd -m /home/chris -u 101 chris
388 passwd chris
389 history 8

* En tapant un point ! suivi du nombre assigné à une commande il est possible de la rejouer :
$ !382
(affiche la date)

* Deux !! rejoue la dernière commande.
$ !!
(affiche la date)

* !?[STRING]? rejoue la commande qui contient le pattern de STRING
$ !?da?
(affiche la date)

* [Ctrl + R] ouvre un champ de recherche pour taper un pattern et appuyer sur entrer pour executer la commande

* [Alt + P] Permet de recherche la commande qui commence par le pattern rentré

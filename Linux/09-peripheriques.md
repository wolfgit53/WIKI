# Périphériques #

## Fichiers spéciaux ##
Les fichiers spéciaux périphériques sont par convention placés dans le répertoire **/dev** et disposent comme n'importe quel autre fichiers, d'un unique inode. Nous pouvons donc connaitre les attributs avec ls -l.

Le premier caractère identifie le type de périphérique:
- **c** : type de périphérique en mode caractère
- **b** : type de périphérique en mode bloc

Ces modes dfférencient le type d'échange de données entre les module de gestion de fichier et le pilote du périphérique. En mode **caractère**, il n'y à pas d'utilisation de buffers du système et l'échange se fait octet par octet.
En mode **bloc** le système accède au périphérique via un index qui représente les coordonnées du bloc de données sur le support. Il est donc plus rapide pour les périphériques comme les disques.

• /dev/mem : la mémoire physique

• /dev/kmem : la m é m o i r e v i r t u e l l e du n o y a u .

• /dev/console : la console m a î t r e ( / d e v / s y s c o n ) .

• /dev/tty : l ’ e n t r é e / s o r t i e s t a n d a r d du p r o c e s s u s en c o u r s .
• /dev/mouse : la souris, souvent un raccourci.
• /dev/swap : le disque swap primaire.
• /dev/null : la poubelle UNIX. On peut y écrire. La lecture provoque un EOF.
• /dev/root : système de fichier spécial root.
• /dev/dump : le disque dans le quel le noyau fait son dump en cas de panique  système.
• /dev/rmt0 : lecteur de bande magnétique ou de cartouche en mode caractère.
• /dev/fd0 : lecteur de d i s q u e t t e s en m o d e b l o c .
• /dev/pts/1 : i d e m mais pour Unix SYSTEM V ( e t L i n u x ) .
• /dev/lp0 : une imprimante parallèle.
• /dev/ttyS0 : port COM1.
• /dev/ttyS1 : port COM2.
• /dev/psaux : port PS2 pour la souris.
• /dev/sound : carte son.
• /dev/dsp : contrôleur DSP de la carte son.
• /dev/sequencer : séquenceur MIDI de la carte son .
• /dev/ide/* : les périphériques IDE.
• /dev/scsi/* : les périphériques SCSI .
© ENI Editions - All rigths reserved -• /dev/usb/* : les périphériques USB.
• /dev/hdX : les disques IDE.
• /dev/sdX : les disques SATA ou SCSI.

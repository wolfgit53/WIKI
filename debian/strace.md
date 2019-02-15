# Surveiller les appels systèmes d'un programme #

Installer le paquet strace:
```bash
apt-get install strace
```

Puis lancer le programme strace:
```bash
strace -o tree.strace.log tree
```
La commande suivante va lancer le programme tree, et va logger tout les appels système qu'il engendre dans le fichiers tree.strace.log.

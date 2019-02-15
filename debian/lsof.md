# Quel processus maintient un fichier bloquer? #

Utiliser **lsof** sans paramètre, ce qui nous donne tout les fichiers locker par un processus:
```bash
lsof
```

Pour voir ce qui bloque un montage:
```bash
lsof /mnt
```

Pour faire l'inverse, on peut demander avec un PID:
```bash
lsof -p 3053
```

Ou par son nom:
```bash
lsof -c syslogd
```

Un processus ouvert par un utilisateur, par son nom:
```bash
lsof -u username -ac vi
```

Enfin par exemple les sockets:
```bash
lsof -i
```

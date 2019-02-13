# Chercher tout les fichiers setUID #

Trouver tous les fichiers dotés d'un bit setuid ou setgid défini:
```bash
find / -perm +6000 -type f -exec ls -ld {}\; > setuid.txt &
```

Pour retirer le bit **s**:
```bash
chmod a-s executable
```

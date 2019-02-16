# NMAP #

NMAP est un utilitaire pour scanner et faire de la découverte sur le réseau. Un scanneur de ports célèbre et disponible sur toutes les plateformes.

Scan et affichage sous forme de liste:
```bash
nmap -sL 192.168.0.0/24
```

Scan au ping:
```bash
nmap -sP 192.168.1.*
```

Scan non loggué sur la machine cible avec le message SYN:
```bash
nmap -sS 192.168.28
```

Scan tout les ports UDP ouvert de la machine:
```bash
nmap -sU 192.168.0.28
```




Détecter le système d'exploitation:
```bash
nmap -O 192.168.0.28
```
Et avec une liste d'OS potentiel:
```bash
nmap -O --osscan-guess 192.168.0.28
```


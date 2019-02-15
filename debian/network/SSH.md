# The SSH power!!! #

## Transférer des ports via ssh ##

On peut transférer des ports TCP vers l'autres extrémité de notre connexion. Cela peut permettre la consultation de page WEB, ou de mail de manière sécurisée;

```bash
ssh -f -N -L[port]:[serveur]:[port] -l utilisateur serveur
```

NAT
Vous pouvez autoriser d'autres clients (distants) à se connecter à votre port transféré grâce au commutateur **-g**.
Transfère toutes les co depuis le port 8000 de la passerelle vers le port 80 de l'hôte interne 10.42.4.6.
```bash
ssh -f -g -N -L8000:localhost:80 10.42.4.6
```

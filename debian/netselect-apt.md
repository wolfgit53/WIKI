# Choix des meilleurs dépots Debian #

Netselect permet de selectionner parmis les serveurs les plus rapides un dépot pour s'adapter à notre connexion internet.
```bash
apt-get install netselect-apt
```

Puis se placer dans un répertoire, où nous allons générer grâce à ce paquet un fichier source.list contenant l'adresse du Dépot répondant le plus rapidement.
```bash
cd /root/ && netselect-apt stable
```

Afficher le fichier:
```bash
ls
  sources.list

cat source.list
  # Debian packages for stable
  deb http://ftp.be.debian.org/debian/ stable main contrib
  # Uncomment the deb-src line if you want 'apt-get source'
  # to work with most packages.
  # deb-src http://ftp.be.debian.org/debian/ stable main contrib

  # Security updates for stable
  deb http://security.debian.org/ stable/updates main contrib
```

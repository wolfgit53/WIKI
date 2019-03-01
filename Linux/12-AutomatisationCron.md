# Automatisation #

## CRON ##
Le service **cron** permet la programmation d'événements à répétition. Il fonctionne à l'aide d'une table, appelée une **crontab**. Pour modifier la contab on utilise la commande crontab avec le paramètre **-e**dit. 

Exemple:

Exécution de df tous les jours, toute l'année, tous les quarts d'heure:
```bash
0,15,30,45 * * * * df

Exécution de df tous les jours ouvrable à 17h:
```bash
0 17 * * 1-5 df
```


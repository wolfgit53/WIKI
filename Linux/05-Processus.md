# Processus Linux #

1. Définition et environnement

Un **processus** représente à la fois un programme en cours d'exécution et tout son environnement d'exécution (mémoire, état, identification, propriétaire, père...)

**Un numéro de processus unique PID** : Chaque processus Unix est numéroté afin de pouvoir différencié des autres.
**Un numéro de processus parent PPID** : Chaque processus peut lancer d'autre processus. Chaque enfant reçoit le PID du processus qui l'a lancé.
**

## Background, foreground, jobs ##
Pour lancer un processus en background il faut rajouter un **&**, à la fin de la commande.
```bash
ls -R / > ls.txt &
```

Pour stopper un processus temporairemenent en tapant un [CTRL] + Z:
```bash
sleep 100
<CTRL+Z>
[1]+ Stopped
```


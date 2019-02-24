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

Son exécution est suspendue jusqu'à le replacez au premier plan avec la commande **fg**:
```bash
fg
```

On peut voir les processus avec la commande **jobs**:
```bash
jobs
```

Pour relancer en premier plan, il faut connaitre le numéro de jobs du processus:
```bash
fg [id]
```

Ou pour le laisser tourner en tâche de fond:
```bash
bg [id]
```

## Lister les processus ##
C'est la commande **ps** qui permet de lister les processus:
```bash
ps
```

Paramètres:
1. **-ef** processus en cours
2. **-u** suivi de l'utilisateur
3. **-g** suivi d'un nom de groupe
4. **-l** pour les infos techniques
  1. **C** Facteur de priorité, plus la valeur est grande plus la priorité est haute
  2. **STIME** Heure du lancement
  3. **TTY** Numéro du terminal qui à lancé le processus
  4. **TIME** Durée de traitement du processus
  5. **CMD** Commande exécutée
  6. **F** Drapeau du processus
  7. **S** Etat du processus S(leeping) R(unning) Z(ombie)
  8. **PRI** Priorité du processus
  9. **NI** Nice, incrément pour le sheduler
  
## Arrêt d'un processus ##
Pour arrêter un processus:
```bash
kill [-l] -Num_signal PID [PID2...]
```

Num_signal:
1. SIGHUP - Hang Up est envoyé par le père à tous ses enfants lorsqu'il se termine
2. SIGINT - Interruption du processus demandé (CTRL+C)
3. SIGQUIT - Génere un Core Dump (Fichier de débuggage)
9. SIGKILL - Signal ne peut être ignoré
15. SIGTERM - Signal envoyé pour que le processus se termine normalement

Avec -l pour lister les signaux possible.

## nohup ##
Quand le shell est quitter le signal SIGHUP est envoyé à tout les enfants pour qu'ils se terminent. Il est possible d'éviter que le processus s'arrête avec **nohup**. Les canaux de sortie

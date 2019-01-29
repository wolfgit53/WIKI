# La notion de Processus #

Sous Unix, toute tâche qui est en cours d'exécution est représentée par un **processus**. Un processus est une entité comportant à la fois :
* des données
* du code

A un instant donné, un processus peut, comme nous le verrons se trouver dans plusieurs états. Le noyau est chargé de réguler l'exécution des processus afin de garantir un comportement multitâche performant. Le noyau fournis un mécanisme de régulation des tâches qu'on nomme "ordonnancement" (en anglais *scheduling*). Cela assure la répartition équitable de l'accès au microprocesseur par les différents processus.

Sur une une machine uni-processeur, il n'y a qu'un seul processus qui s'exécute à un temps t. Le noyau assure une consommation régulière pour chaque processus. Sur une machine multi-processeur, plusieurs processus peuvent s'exécuter en même temps.

Avec la commande **ps** on peut observer la liste des processus à l'instant t, ou la commande **ps** est exécutée :
```bash
# ps ax
  PID TTY      STAT   TIME COMMAND
    1 ?        Ss     0:01 /sbin/init
    2 ?        S      0:00 [kthreadd]
    3 ?        S      0:00 [ksoftirqd/0]
    5 ?        S<     0:00 [kworker/0:0H]
    7 ?        S      0:00 [rcu_sched]
    8 ?        S      0:00 [rcu_bh]
    9 ?        S      0:00 [migration/0]
   10 ?        S<     0:00 [lru-add-drain]
   11 ?        S      0:00 [watchdog/0]
   12 ?        S      0:00 [cpuhp/0]
   13 ?        S      0:00 [cpuhp/1]
   14 ?        S      0:00 [watchdog/1]
   15 ?        S      0:00 [migration/1]
   16 ?        S      0:00 [ksoftirqd/1]
   18 ?        S<     0:00 [kworker/1:0H]
   19 ?        S      0:00 [cpuhp/2]
   20 ?        S      0:00 [watchdog/2]
   21 ?        S      0:00 [migration/2]
   22 ?        S      0:00 [ksoftirqd/2]
   24 ?        S<     0:00 [kworker/2:0H]
   25 ?        S      0:00 [cpuhp/3]
   26 ?        S      0:00 [watchdog/3]
   27 ?        S      0:00 [migration/3]
   28 ?        S      0:00 [ksoftirqd/3]
   30 ?        S<     0:00 [kworker/3:0H]
   [...]
```

## Identification par PID ##

Le premier processus du système est *init*, il est directement créer par le noyau au démarrage. La seul manière de créer un nouveau processus est d'appeler l'appel-système **fork()** qui va dupliquer le processus appelant. Après cette appel, deux processus identique s'exécuteront.


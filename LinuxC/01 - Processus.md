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
La différence essentielle entre ces deux processus est un numéro d'identification. Ce qui distingue le processus père et le processus fils. C'est le **PID** (Process IDentifier).

L'appel système de fork ce trouve dans <unistd.h>. On retrouve:
```C
pid_t fork(void)
```

Pour connaitre son propre identifiant PID, on utilise getpid() qui renvoi un type *pid_t*:
```C
pid_t getpid(void)
```

La création d'un processus fils à pour but de dialoguer avec. Le processus fils peut accéder au PID de sont processus père avec :
```C
pid_t getppid(void)
```

Lorsqu'un processus est créé par fork(), il dispose d'une copie des données de son père, également de l'environnement de celui ci, et d'un certain nombre d'autre paramètre. On parle d'**héritage** du père.

Sous linux l'appel fork() est très économe car il utilise un methode de "copie sur écriture". Cela signifie que toute les données qui doivent être transmise ne sont pas tout de suite recopiées.
En cas d'erreur fork() renvoi -1, et la variable *errno* contient le code d'erreur définis dans <errno.h>. La valeur peut être :
* ENOMEM
Qui indique que le noyau n'a plus assez de mémoire disponible pour créer un nouveau processus.
* EAGAIN
Qui signale que le système n'a plus de place libre dans sa table des processus. Mais qu'il y en aura probablement sous peu.

### Exemple ###
Voici un exemple de création d'un processus fils par l'appel système fork() :
```C
#include <stdio.h>
#include <stdlib.h>

#include <unistd.h>
#include <errno.h>
#include <sys/wait.h>

int main()
{
    printf("Hello world!\n");

    pid_t pid_fils;

    do {
        pid_fils = fork();
    } while ((pid_fils == -1) && (errno == EAGAIN));

    if (pid_fils == -1)
    {
        fprintf(stderr, "fork () impossible, errno=%d\n", errno);
        return 1;
    }
    if (pid_fils == 0) {
        fprintf(stdout, "Fils : PID=%d, PPID=%d\n", getpid(), getppid());
        return 0;
    } else {
        fprintf(stdout, "Père : PID=%d, PPID=%d\n", getpid(), getppid());
        wait(NULL);
        return 0;
    }
}
```
Lors de son exécution on observe quelque choses comme cela :
```bash
./exemple_fork
Père: PID=3133, PPID=3010, PID fils=3134
Fils: PID=3134, PPID=3133
```
Le PPID correspond à celui du shell où l'on execute le programme.

## Identification de l'utilisateur correspondant au processus ##

Un système unix est particulièrement orienté vers l'identification des ses utilisateurs. Chaque processus s'exécute sous une identité précise. Il s'agit en général de l'identité du lanceur du programme. Il existe trois identifiants d'utilisateur par procéssus :
* UID **réel**
UID de l'utilisateur qui à lancé le programme.
* UID **effectif**
UID correspond aux privilège accordé au processus.
* UID **sauvé**
C'est une copie de l'ancien UID effectif lorsque celui-ci est modifié par le processus.

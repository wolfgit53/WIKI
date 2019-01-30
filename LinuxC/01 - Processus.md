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

Lors d'une tentative d'accès à un fichier, le noyau effectue des vérification d'autorisation en prenant compte l'UID effectif du processus. Généralement l'UID effectif est le même que l'UID réel, pour les commandes classique (ls, cp, mv...) qui peuvent s'exécuter sans privilèges.

Pour accéder aux UIDs nous avons des appel systèmes venant de <unistd.h> :
* UID réel :
```C
uid_t getuid(void)
```
* UID effectif :
```C
uid_t geteuid(void)
```

Prenons en exemple le code suivant:
```C
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

int main()
{
    fprintf(stdout, " UID réel = %u, UID effectif = %u\n", getuid(), geteuid());
    return 0;
}
```
En l'exécutant avec un utilisateur lambda :
```bash
./exemple_uid
UID réel = 1000, UID effectif = 1000
```
Puis :
```bash
$ ./UID 
 UID réel = 1000, UID effectif = 1000
$ su
Mot de passe : 
 chown root.root UID
 chmod +s UID 
 ls -ln UID *
-rwsr-sr-x 1 0 0 11512 janv. 29 22:37 UID
-rwsr-sr-x 1 0 0 11512 janv. 29 22:37 UID
 ./UID 
 UID réel = 0, UID effectif = 0
 exit
exit
 ./UID
 UID réel = 1000, UID effectif = 0
```

Nous  voyons  l'attribut  Set-UID  indiqué  par  la  lettre  «s»  dans  les  autorisations  d'accès. L'UID réel est conservé à des fins d'identification éventuelle au sein du processus. Notre programme ayant UID effectif de root en a tous les privilèges. Vous pouvez en avoir le coeur net en lui faisant, par exemple, créer un nouveau fichier dans le répertoire /etc. Si vous n'avez pas les privilèges root sur votre système, vous pouvez néanmoins effectuer les  tests  en  accord  avec  un  autre  utilisateur qui  copiera  votre  exécutable  dans  son répertoire personnel (pour en prendre possession) et lui ajoutera le bit Set-UID. Il  existe  plusieurs  appels-système  permettant  à  un  processus  de  modifier  son  UID. Il ne peut toutefois s'agir que de  perdre des privilèges, éventuellement d'en retrouver des anciens, mais jamais d'en gagner. Imaginons un émulateur de  terminal série  (un peu comme kermit ou minicom). Il a besoin d'accéder à un périphérique système (le modem), même  en  étant  lancé  par n'importe  quel  utilisateur.  Il  dispose  donc  de  son  bit  Set-UID  activé, tout en appartenant à root. Cela lui permet d'ouvrir le fichier spécial correspondant au périphérique et de gérer la liaison. Toutefois,  il  faut  également  sauvegarder  sur  disque  des  informations  n'appartenant  qu'à  l'utilisateur  ayant  lancé  l'application  (sa  configuration  préférée  pour  l'interface,  par  exemple),  voire  enregistrer  dans  un  fichier  un historique  complet  de  la  session.  Pour  ce  faire,  le  programme  ne  doit  créer  des  fichiers  que  dans  des  endroits  où  l'utilisateur  est  autorisé à le faire. Plutôt que de vérifier toutes les autorisations d'accès, il est plus simple de  perdre  temporairement  ses  privilèges  root  pour  reprendre  l'identité  de  l'utilisateur original,  le  temps  de  faire  l'enregistrement (les  permissions  étant  alors vérifiées  par  le noyau), et de redevenir éventuellement root ensuite. Nous reviendrons à plusieurs reprises sur ce mécanisme. Le  troisième  type  d'UID  d'un  processus  est  l'UID  sauvé.  Il  s'agit  d'une  copie  de  l'ancien  UID effectif lorsque celui-ci est modifié par l'un des appels décrits ci-dessous. Cette copie est  effectuée  automatiquement  par  le  noyau.  Un  processus  peut  toujours  demander  à  changer  son  UID  effectif  ou  son  UID  réel  pour prendre  la  valeur  de  l'UID  sauvé.  Il  est  également possible de prendre en UID effectif la valeur de UID réel, et inversement. 32 Un processus avec le bit Set-UID positionné démarre donc avec un UID effectif différent de celui  de  l'utilisateur  qui  l'a  invoqué.  Quand  il  désire  effectuer  une  opération  non privilégiée. il peut demander à remplacer son UID effectif par l'UID réel. Une copie de l'UID effectif  est  conservée  dans  l'UID  sauvé.  Il pourra  donc  à  tout  moment  demander  à  remplacer à nouveau son UID effectif par son UID sauvé. Pour cela, il existe plusieurs appels-système permettant sous Linux de modifier son UID, et ayant tous une portabilité restreinte : setuid( ) est défini par Posix.1, seteuid( ) et  setreuid( ) appartiennent à BSD, 
setresuid( ) est spécifique à Linux. 

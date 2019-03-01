# PAM #

PAM (**P**luggable **A**uthentification **M**odules) est un ensemble de modules et une lib qui permet la mise en place des méchanisme d'auth pour de la sécu accrue.

Chaque module authentifie d'une manière particulière, LDAP, unix classique, emprunte digitale et retourne vrai ou faux. Suivant la configuration, un vrai peut être un requis, ou suffisant.

Les fichiers de configuration sont placés dans **/etc/pam.d**. La syntaxe est la suivante:
Type_module contrôle module arguments

## Type_module ##
- auth : module d'authentification (ex: login + mdp)
- account : autorisation, gestion de comptes (vérif l'utilisateur pour le service donné. Est-il autorisé?)
- password : Le mot de passe est-il encore valable?
- session : modification de l'environnement de l'utilisateur

## Contrôle ##
- required : réussite requise. Si échec, les autres modules sont appelés mais le PAM retourne un échec
- requisite : réussite requise. Si échec, arrêt total.
- sufficient : réussite contourne les autres modules
- optional : le résultat est ignoré

## Module ##
- pam_unix.so : auth standard via la fonction C getpw()
- pam_env.so : définition des variable d'environnement
- pam_securetty.so : interdit un co root depuis un terminal non sécurisé. La liste des terminaux autorisés sont dans **/etc/securetty**
- pam_stack.so : appelle un autre service PAM pour le chargement de modules supplémentaires
- pam_nologin.so : interdit la co si le fichier **/etc/nologin** est présent
- pam_deny.so : retourne toujours un échec
- pam_console.so : donne des permissions supplémentaires à un utilisateur local

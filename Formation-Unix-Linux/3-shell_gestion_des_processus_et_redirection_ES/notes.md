# 3 - Le Shell : gestion des processus, redirection d'entrées / sorties

## Définitions
Un programme est une entité passive. Lorsque le programme est lancé, un processus est créé par le système d'exploitation. Un processus est une entité active.

## Exécution d'une commande UNIX dans un shell
Lorsqu'on tape une commande externe au shell, un processus fils est créé et la commande s'y exécute. Le processus père (le shell) attend que le processus fils meure pour redevenir actif. Ainsi, deux appels système sont utilisés : **fork** et **exec**.

Il est possible d'obliger l'exécution d'un processus dans le shell courant en lançant la commande à travers la commande interne **exec** ou, dans le cas d'un script, en utilisant le point (**.**) ou la commande interne **source**.

## Caractéristiques des processus

### Caractéristiques générales
Un système UNIX identifie les processus grâce à un nombre appelé PID (Process IDentificator). Un processus a un propriétaire. Les informations concernant les processus s'obtiennent avec la commande **ps**, dont il existe une version System V et une version BSD.

	- UID : propriétaire
	- PID : numéro
	- PPID : numéro du père
	- PRI : priorité
	- NI : valeur de NICE
	- STAT : état
	- TTY : nom du device vers lequel il est dirigé
	- TIME : temps pasée dans la CPU
	- COMMAND : nom de la commande executée

NB : Tous les processus sont lances par le processus **init** (PID=1) dont le processus père est le noyau (PID=0).

La commande **pstree** permet de visualiser d'une autre manière l'arborescence des processus.

La commande **ps fux** permet de n'afficher que ses processus.

### Etat d'un processus

	- D : ininterruptible
	- R : en cours d'exécution
	- S : endormi
	- T : arrêté
	- Z : zombi (mort sans que son père le sache)

Quelques autres informations :

	- W : swappé
	- < : priorité haute
	- N : priorité basse
	- L : possède des pages mémoires verrouillées

### Les entrées / sorties
La création d'un nouveau processus s'accompagne de celle d'une table de description de fichier. Par défaut, les entrées suivantes sont créées :

	- STDIN (0) : entrée standard - clavier par défaut - redirection avec <
	- STDOUT (1) : sortie standard - TTY par défaut - redirection avec > et >>
	- STDERR (2) : sortie d'erreur standard - TTY par défaut - redirection avec 2> et 2>>

### Le code retour d'un processus
L'exécution d'une commande se termine par l'envoi d'un code retour au processus père :

	- 0 : tout va bien
	- 1 : erreur de syntaxe
	- 2 : erreur d'emploi

La variable **$?** du shell permet d'afficher le code retour de la dernière commande executée.

## Gestion des processus

### Background et Foreground
Pour exécuter une tâche en fond et récupérer le prompt immédiatement, il faut utiliser le caractere **&** à la fin de la commande et rediriger la sortie standard dans un fichier.
- **jobs** permet d'obtenir la liste des processus en tache de fond
- **fg** permet de passer un processus de l'arrière au premier plan
- *[CTRL-Z]* permet d'arrêter un processus
- **bg** permet de passer un processus du premier à l'arrière plan
**fg** et **bg** prennent en paramètre soit un PID, soit un numéro retourné par la commande **jobs**, lequel est précédé d'un **%**.

### Les signaux
Le systeme communique avec les processus à l'aide de signaux.

	- *[CTRL-Z]* envoie le signal numéro 24 au processus courant : SIGSTOP, pour arrêter le traitement
	- une déconnexion envoie le signal numéro 1 à tous les processus : SIGHUP
	- *[CTRL-C]* envoie le signal numéro 2 au processus courant : SIGINT

Si un processus reçoit un signal auquel aucun traitement n'est associé, il meurt. Il est parfois possible de définir ce traitement associé ou d'ignorer un signal.

La commande **kill** envoie par défaut le signal numéro 15. Pour être sûr d'éliminer un processus, il faut envoyer le signal 9.

### Détachement d'un processus
Un processus est tué en cas de déconnexion, mais il existe un moyen pour le laisser s'exécuter et visualiser les résultats stockés dans un fichier, en utilisant **nohup** avant la commande et en redirigeant le résultat vers un fichier.

### La commande **trap**
Elle permet d'ignorer des signaux ou de leur associer un traitement particulier.

### Priorité des processus
La commande **nice** permet de modifier la priorité d'exécution. Elle se place avant le programme à exéecuter en précisant une valeur entre 0 et 19 pour un simple utilisateur, et entre -20 et 19 pour un super utilisateur. La valeur par défaut est le 0.

La commande **renice** permet de modifier cette valeur pour un processus déjà chargé en mémoire.

La commande **top** permet de visualiser l'évolution du taux d'occupation dans la CPU des processus en temps réel.

La commande **time** permet d'indiquer le temps d'exécution d'une commande. Elle donne trois temps :

	- real : temps réel passé entre le début et la fin de l'exécution
	- user : temps passé en mode utilisateur
	- sys : temps passé en mode noyaux


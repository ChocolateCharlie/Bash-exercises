# 3 - Le Shell : gestion des processus, redirection d'entree / sortie

## Definitions
Un programme est une entite pasive. Lorsque le programme est lance, un processus est cree par le systeme d'exploitation. Un processus est une entite active.

## Execution d'une commande UNIX dans un shell
Lorsqu'on tape une commande externe au shell, un processus fils est cree et la commande s'y execute. Le processus pere (le shell) attend que le processus fils meure pour redevenir actif. Ainsi, deux appels systeme sont utilises : *fork* et *exec*.
Il est possible d'obliger l'execution d'un processus dans le shell courant en lancant la commande a travers la commande interne *exec* ou, dans le cas d'un script, en utilisant le point ou la commande interne *source*.

## Caracteristiques des processus

### Caracteristiques generales
Un systeme UNIX identifie les processus grace a un nombre appele PID (Process IDentificator). Un processus a un proprietaire. Les informations concernant les processus s'obtiennent avec la commande *ps*, dont il existe une version System V et une version BSD.

	- UID : proprietaire
	- PID : numero
	- PPID : numero du pere
	- PRI : priorite
	- NI : valeur de NICE
	- STAT : etat
	- TTY : nom du device vers lequel il est dirige
	- TIME : temps passe dans la CPU
	- COMMAND : nom de la commande executee

NB : Tous les processus sont lances par le processus *init* (PID=1) dont le processus pere est le noyau (PID=0).
La commande *pstree* permet de visualiser d'une autre maniere l'arborescence des processus.
La commande *ps fux* permet de n'afficher que ses processus.

### Etat d'un processus

	- D : ininterruptible
	- R : en cours d'execution
	- S : endormi
	- T : arrete
	- Z : zombi (mort sans que son pere le sache)

Quelques autres informations :

	- W : swappe
	- < : priorite haute
	- N : priorite basse
	- L : possede des pages memoires verrouillees

### Les entrees / sorties
La creation d'un nouveau processus s'accompagne de celle d'une table de description de fichier. Par defaut, les entrees suivantes sont creees :

	- STDIN (0) : entree standard - clavier par defaut - redirection avec <
	- STDOUT (1) : sortie standard - TTY par defaut - redirection avec > et >>
	- STDERR (2) : sortie d'erreur standard - TTY par defaut - redirection avec 2> et 2>>

### Le code retour d'un processus
L'execution d'une commande se termine par l'envoi d'un code retour au processus pere :

	- 0 : tout va bien
	- 1 : erreur de syntaxe
	- 2 : erreur d'emploi

La variable *$?* du shell permet d'afficher le code retour de la derniere commande execute.

## Gestion des processus

### Background et Foreground
Pour executer une tache en fond et recuperer le prompt immediatement, il faut utiliser le caractere *&* a la fin de la commande et redirifer la sortie standard dans un fichier.
- *jobs* permet d'obtenir la liste des processus en tache de fond
- *fg* permet de passer un processus de l'arriere au premier plan
- *[CTRL-Z]* permet d'arreter un processus
- *bg* permet de passer un processus du premier a l'arriere plan
*fg* et *bg* prennent en parametre soit un PID, soit un numero retourne par la commande *jobs*, lequel est precede d'un *%*.

### Les signaux
Le systeme communique avec les processus a l'aide de signaux.

	- *[CTRL-Z]* envoie le signal numero 24 au processus courant : SIGSTOP, pour arreter le traitement
	- une deconnexion envoie le signal numero 1 a tous les processus : SIGHUP
	- *[CTRL-C]* envoie le signal numero 2 au processus courant : SIGINT

Si un processus recoit un signal auquel aucun traitement n'est associe, il meurt. Il est parfois possible de definir ce traitement associe ou d'ignorer un signal.
La commande *kill* envoie par defaut le signal numero 15. Pour etre sur d'eliminer un processus, il faut envoyer le signal 9.

### Detachement d'un processus
Un processus est tue en cas de deconnexion, mais il existe un moyen pour le laisser s'executer et visualiser les resultats stockes dans un fichier, en utilisant *nohup* avant la commande et en redirigeant le resultat vers un fichier.

### La commande *trap*
Elle pertmet d'ignorer des signaux ou de leur associer un traitement particulier.

### Priorite des processus
La commande *nice* permet de modifier la priorite d'execution. Elle se place avant le programme a executer en precisant une valeur entre 0 et 19 pour un simple utilisateur, et entre -20 et 19 pour un super utilisateur. La valeur par defaut est le 0.
La commande *renice* permet de modifier cette valeur pour un processus deja charge en memoire.
La commande *top* permet de visualiser l'evolution du taux d'occupation dans la CPU des processus en temps reel.
La commande *time* permet d'indiquer le temps d'execution d'une commande. Elle donne trois temps :

	- real : temps reel passe entre le debut et la fin de l'execution
	- user : temps passe en mode utilisateur
	- sys : temps passe en mode noyaux


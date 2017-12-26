# 4 - Archivage et compression de fichiers

## L'extension d'un fichier sous UNIX
La commande **file** suivie d'un nom de fichier analyse son contenu et retourne son type.

	- .a .sl .so	librairies
	- .f		sources en Fortran
	- .ps .eps	sources en PostScript et PostScript encapsule
	- .tar .cpio	archives realises avec tar ou cpio
	- .gz .Z .bz	fichiers compresses avec gzip, compress ou bzip2
	- .C		fichiers compresses avec compact ou fichier C++

## L'archivage de fichiers
- **tar** (Tape Archive)

	- -c : creation
	- -v : visualisation
	- -t : visualiser le contenu de la sauvegarde
	- -x : restaurer la totalite de la sauvegarde, ou un fichier particulier en specifiant son nom

Pour sauvegarder une arborescence, on peut speficier les noms de chemin de maniere relative ou absolue.

Pour une sauvegarde sur bande, on peut utiliser */dev/st0* qui correspond au lecteur de bande sous Linux.

- **cpio** (Copy Input and Output)
L'entree standard de la commande est detachee, donc il faut lui associer une commande qui genere la liste des fichiers a sauvegarder (comme **ls** et **find**).

	- -o : output
	- -v : visualisation
	- -B : buffer

D'autres outils d'archivage ou de stockage existent, comme **dump** et **restore**.

## La compression de fichiers

# compress (.Z)
Cette commande reduit les fichiers suivant l'algorithme de Lempel-Ziv.

L'option -v permet de montrer le taux de compression obtenu.

	- **compress** et **uncompress** : decompresser des fichiers
	- **zcat** : afficher le contenu d'un fichier compresse sans avoir a le restaurer

# gzip (.gz)
Cette commande reprend l'algorithme de codage de Lempel-Ziv.

L'option -v affiche le taux de compression obtenu pour chacun des fichiers passes en parametre.

L'option -d permet de decompresser un fichier, comme **gunzip**.

**zcat** fonctionne aussi.

# bizip2 (.bz2)
Cette commande utilise l'algorithme de compression de texte par tri de blocs de Burrows-Wheeler et le codage de Huffman. Les taux de compression obtenus sont meilleurs qu'avec les commandes precedentes.

La syntaxe de **bzip2** et **bunzip2** est similaire.

Il faut utiliser **bzcat** et non **zcat**.



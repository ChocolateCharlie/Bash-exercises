# 4 - Archivage et compression de fichiers

## L'extension d'un fichier sous UNIX
La commande **file** suivie d'un nom de fichier analyse son contenu et retourne son type.

	- .a .sl .so	librairies
	- .f		sources en Fortran
	- .ps .eps	sources en PostScript et PostScript encapsulé
	- .tar .cpio	archives réalisées avec tar ou cpio
	- .gz .Z .bz	fichiers compressés avec gzip, compress ou bzip2
	- .C		fichiers compressés avec compact ou fichier C++

## L'archivage de fichiers
- **tar** (Tape Archive)

	- -c : création
	- -v : visualisation
	- -t : visualiser le contenu de la sauvegarde
	- -x : restaurer la totalité de la sauvegarde, ou un fichier particulier en spécifiant son nom

Pour sauvegarder une arborescence, on peut spéficier les noms de chemin de manière relative ou absolue.

Pour une sauvegarde sur bande, on peut utiliser */dev/st0* qui correspond au lecteur de bande sous Linux.

- **cpio** (Copy Input and Output)
L'entrée standard de la commande est detachée, donc il faut lui associer une commande qui génère la liste des fichiers à sauvegarder (comme **ls** et **find**).

	- -o : output
	- -v : visualisation
	- -B : buffer

D'autres outils d'archivage ou de stockage existent, comme **dump** et **restore**.

## La compression de fichiers

### compress (.Z)
Cette commande réduit les fichiers suivant l'algorithme de Lempel-Ziv.

L'option -v permet de montrer le taux de compression obtenu.

	- **compress** et **uncompress** : décompresser des fichiers
	- **zcat** : afficher le contenu d'un fichier compressé sans avoir à le restaurer

### gzip (.gz)
Cette commande reprend l'algorithme de codage de Lempel-Ziv.

L'option -v affiche le taux de compression obtenu pour chacun des fichiers passés en paramètre.

L'option -d permet de décompresser un fichier, comme **gunzip**.

**zcat** fonctionne aussi.

### bzip2 (.bz2)
Cette commande utilise l'algorithme de compression de texte par tri de blocs de Burrows-Wheeler et le codage de Huffman. Les taux de compression obtenus sont meilleurs qu'avec les commandes précédentes.

La syntaxe de **bzip2** et **bunzip2** est similaire.

Il faut utiliser **bzcat** et non **zcat**.



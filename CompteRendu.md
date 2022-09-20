# Exercice 1

1. Pour mettre à jour notre système, nous utilisons les commandes ```sudo apt update``` et ```sudo apt upgrade```.

2. Pour créer un alias, il faut utiliser la commande ```alias  maj='sudo apt update | sudo apt upgrade'```. Cet alias peut être enregistré dans le fichier **~/.bashrc** ou **~/.bash_aliases**, pour qu'il s'enregistre, il faut soit redémarrer le terminal, soit exécuter la commande ```source ~/.bashrc```.

3. Pour afficher les 5 derniers paquets installés sur notre machine, on peut soit utiliser la commande ```cat /var/log/dpkg.log``` et récupérer les 5 derniers. Une manière plus optimisée serait d'utiliser la commande `tail -5 /var/log/dpkg.log`.

4. Pour lister les derniers paquets installés avec la commande **apt install**, on doit utiliser la commande `dpkg -l` ou plus exactement la commande `dpkg -l | grep ^i | grep -v ^ia | grep -v ^...lib`.

5. Avec la commande **dpkg**, on peut compter le nombre total de paquets installés sur la machine avec la commande `dpkg -l | wc -l`. Il y en a 611. Utiliser `dpkg -l | grep '^ii' | wc -l` pur avoir le bon nombre.
Avec la commande **apt**, on peut compter le nombre total de paquets installés sur la machine avec la commande `
sudo apt list --installed | wc -l`. Il y en a 607.
La différence de comptage est dû à la présence de lignes d'information.
On ne peut pas directement utiliser le fichier dpkg.log car il ne contient pas uniquement les paquets installés sur la machine.

6. Il y a 68744 paquets disponibles en téléchargement sur les dépôts Ubuntu. Pour trouver ce résultat, il faut taper la commande `sudo apt list | wc -l`.

7. Glances est un outil de surveillance pour GNU/Linux. Il surveille le processeur, la charge système, la mémoire, la bande passante réseau, les E/S de disque, l'utilisation du disque ainsi que les processus. Pour l'installer, il faut taper la commande `sudo apt install glances`. Pour l'exécuter, il suffit de taper `glances`.
Tldr convertit les pages de manuel en explications concises et simples. Pour l'installer, il faut taper la commande `sudo apt install -g tldr`. Il est parfois nécessaire de faire un `tldr -u`. Si le dossier n'existe pas, le créer et relancer la commande. On peut ensuite tester la commande de cette manière : `tldr commande`. Ex. : tldr ls.
Hollywood est un programme qui simule un genre de centre de contrôle affichant des logs qui défilent, des lignes de commandes et du binaire façon Matrix. Pour l'installer : `sudo apt install hollywood`. Pour le lancer : `hollywood`.

8. Les paquets **gnome-sudoku**, **sudoku** et **ksudoku** proposent de jouer au sudoku.

# Exercice 2

La commande **ls** est installé à partir du paquet **coreutils**. Pour trouver le paquet, on fait :
`which -a ls | xargs dpkg -S 2>/dev/null | cut -f1 -d:`.
Pour généraliser la commande pour qu'elle soit utilisable pour n'importe quelle commande entrée en paramètre, on remplace **ls** par **$1**.

# Exercice 3

`dpkg -l coreutils 2> /dev/null | grep "^ii" > /dev/null && echo "INSTALLE" || echo "NON INSTALLE"`

Décortication :
* dpkg -l nom_paquet : va chercher le paquet en question
* 2> /dev/null : va rediriger (en cas d'erreur) le message vers /dev/null
* grep "^ii" : va afficher uniquement les lignes commençant par "ii"
* grep "^ii" > /dev/null : va envoyer le résultat vers /dev/null
* echo MESSAGE : affiche le message (&& commande => la commande précédente doit avoir fonctionné; || commande => la commande précédente doit avoir échoué)

# Exercice 4

Pour lister les programmes livrés avec **coreutils**, il faut utiliser la commande `dpkg -L coreutils`. L'option **-L** permet de lister le contenu du paquet.

# Exercice 5

Pour installer les deux paquets, il faut utiliser la commande : `sudo aptitude install nom_paquet`.
**Emacs** est un éditeur de texte auto-documenté et extensible.
**Lynx** définit la norme pour les clients web en mode texte.

# Exercice 6


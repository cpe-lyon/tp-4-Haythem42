BELHADJ MANSOUR Haythem<br>
3ICS - CPE Lyon
# TP 4 - Gestion des paquets

# Exercice 1.  Commandes de base

1 - Pour mettre à jour notre système, nous utilisons les commandes ```sudo apt update``` et ```sudo apt upgrade```.

2 - Pour créer un alias, il faut utiliser la commande ```alias  maj='sudo apt update | sudo apt upgrade'```. Cet alias peut être enregistré dans le fichier **~/.bashrc** ou **~/.bash_aliases**, pour qu'il s'enregistre, il faut soit redémarrer le terminal, soit exécuter la commande ```source ~/.bashrc```.

3 - Pour afficher les 5 derniers paquets installés sur notre machine, on peut soit utiliser la commande ```cat /var/log/dpkg.log``` et récupérer les 5 derniers. Une manière plus optimisée serait d'utiliser la commande `tail -5 /var/log/dpkg.log`.

4 - Pour lister les derniers paquets installés avec la commande **apt install**, on doit utiliser la commande `dpkg -l` ou plus exactement la commande `dpkg -l | grep ^i | grep -v ^ia | grep -v ^...lib`.

5 - Avec la commande **dpkg**, on peut compter le nombre total de paquets installés sur la machine avec la commande `dpkg -l | wc -l`. Il y en a 611. Utiliser `dpkg -l | grep '^ii' | wc -l` pur avoir le bon nombre.
Avec la commande **apt**, on peut compter le nombre total de paquets installés sur la machine avec la commande `
sudo apt list --installed | wc -l`. Il y en a 607.
La différence de comptage est dû à la présence de lignes d'information.
On ne peut pas directement utiliser le fichier dpkg.log car il ne contient pas uniquement les paquets installés sur la machine.

6 - Il y a 68744 paquets disponibles en téléchargement sur les dépôts Ubuntu. Pour trouver ce résultat, il faut taper la commande `sudo apt list | wc -l`.

7 - Glances est un outil de surveillance pour GNU/Linux. Il surveille le processeur, la charge système, la mémoire, la bande passante réseau, les E/S de disque, l'utilisation du disque ainsi que les processus. Pour l'installer, il faut taper la commande `sudo apt install glances`. Pour l'exécuter, il suffit de taper `glances`.
Tldr convertit les pages de manuel en explications concises et simples. Pour l'installer, il faut taper la commande `sudo apt install -g tldr`. Il est parfois nécessaire de faire un `tldr -u`. Si le dossier n'existe pas, le créer et relancer la commande. On peut ensuite tester la commande de cette manière : `tldr commande`. Ex. : tldr ls.
Hollywood est un programme qui simule un genre de centre de contrôle affichant des logs qui défilent, des lignes de commandes et du binaire façon Matrix. Pour l'installer : `sudo apt install hollywood`. Pour le lancer : `hollywood`.

8 - Les paquets **gnome-sudoku**, **sudoku** et **ksudoku** proposent de jouer au sudoku.

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

# Exercice 5. aptitude

Pour installer les deux paquets, il faut utiliser la commande : `sudo aptitude install nom_paquet`.
**Emacs** est un éditeur de texte auto-documenté et extensible.
**Lynx** définit la norme pour les clients web en mode texte.

# Exercice 6. Installation d’un paquet par PPA

1 - On tape les commandes pour installer la version Oracle de Java :
```
sudo add-apt-repository ppa:linuxuprising/java
sudo apt update
sudo apt install oracle-java17-installer
```
On installe **java17** car la **java15** n'est pas trouvé.

2 - En tapant la commande ```/etc/apt/sources.list.d``` , on peut remarquer la présence du fichier **linuxuprising-ubuntu-java-jammy.list**. Il contient l'url permettant de chercher la mise à jour de java.

# Exercice 7. Installation d’un logiciel à partir du code source

1 - On tape la commande ```git clone https://gitlab.com/jallbrit/cbonsai```. On se retrouve avec un dossier **cbonsai**.

2 - On compile le programme a=en suivant les instruction du README.md.

3 - On tape ```sudo apt install checkinstall``` pour installer **checkinstall**.

4 - on tape ```sudo checkinstall```pour refaire la compilation.
En tapant la commande cbonsai, le programme se lance et on obtient bien un bonsai.

# Exercice 8. Création de dépôt personnalisé

## Création d'un paquer Debian avec dpkg-deb

1 - Dans **scripts**, on crée un sous-dossier **origine-commande**. Dans celui-ci, on crée un sous-dossier **DEBIAN**.

2 - Dans le dossier **DEBIAN**, on crée un fichier control dans lequel on écrit :
```
Package: origine-commande 
Version: 0.1 
Maintainer: Foo Bar 
Architecture: all 
Description: Cherche l'origine d'une commande
Section: utils 
Priority: optional
```

3 - On revient dans le dossier parent de **origine-commande** et on tape la commande ```dpkg-deb --build origine-commande```. On a ainsi créé notre paquet.

## Création du dépôt personnel avec reprepro

1 - On crée le dossier **repo-cpe** avec la commande ```mkdir repo-cpe```.

2 - Dans celui-ci, on crée 2 sous-dossiers **conf** et **packages** à l'aide la commande ```mkdir conf packages```.

3 - Dans **conf**, on crée un fichier **distributions** dans lequel on insère :
```
Origin: home
Label: lorem
Suite: stable
Codename: focal
Architectures: i386 amd64 
Components: universe 
Description: lorem
```

4 - On tape la commande ```reprepro -b . export``` pour générer l'arborescence du dépôt. (Il faut installer reprepro si ce n'est pas déjà fait.)

5 - On copie le paquet **origine-commande.deb** dans le dossier **packages** à l'aide de la commande ```cp ../scripts/origine-commande.deb packages/```. En faisant ```ls packages```, on retrouve bien le fichier.

6 - On souhaite créer un nouveau fichier dans **/etc/apt/sources.list.d** et y écrire dedans. On tape la commande ```sudo touch /etc/apt/sources.list.d/repo-cpe.list```. Puis, nous utilisons la commande ```sudo nano /etc/apt/sources.list.d/repo-cpe.list``` et nous écrivons **deb file:/home/user/repo-cpe cosmic multiverse**.

7 - En tapant la commande ```sudo apt update```, on a entrepri la prise en compte de notre dépôt.

## Signature du dépôt avec GPG

1 - On tape la commande ```gpg --gen-key``` pour créer une nouvelle paire de clés. On entre son prénom et son email, puis un mot de passe (aléatoire).

2 - Dans le fichier **distributions**, on ajoute la ligne ```SignWith: yes```.

3 - On ajoute notre clé à notre dépôt en tapant la commande ```reprepro --ask-passphrase -b . export```.

4 - On ajoute notre clé publique à notre dépôt à l'aide de la commande ```gpg --export -a "auteur" > public.key```.

5 - Enfin, on ajoute la clé à la liste des clés fiables connues de **apt** avec la commande ```sudo apt-key add public.key```.

# GNU/Linux avancé - Exercices

## Exercice 1

> Objectifs : Paysage GNU/Linux, Savoir utiliser la documentation `man`,
> communiquer avec les utilisateurs en tant qu'admin

1.  **Citer** trois distributions GNU/Linux.
2.  Quelle est la première distribution qui popularise Linux ?
3.  **Citer** trois logiciels GNU.
4.  **Mettre à jour la date système** en vous aidant de la documentation
    (`man date`).
5.  **Modifier** le fichier `/etc/motd` avec `vi` qui affiche un message
    en début de connexion. **Tester** le résultat en vous reconnectant.

Pour la suite des exercices, **installer une machine virtuelle** (vm
`server`) sous [Debian](https://www.debian.org/distrib/) (**sans
interface graphique**) avec [Oracle
VirtualBox](https://www.virtualbox.org/). Réaliser les exercices sur
cette machine virtuelle pour ne pas toucher à votre système.

## Exercice 2

> Notions abordées : commandes shell de base, système de fichiers UNIX

1.  Quel est le rôle du shell ?
2.  Quelles informations sont contenues dans l'invite de commandes
    `neo@Debian:~/Documents$`?
3.  Comment créer un repertoire en ligne de commandes sous Linux ?
4.  Parmi ces chemins, lequel est absolu :
    1.  `./users/local/sbin`
    2.  `Documents/perso`
    3.  `/usr/local/sbin`
5.  Pourquoi ne faut il pas executer la commande `rm -rf /` ?
6.  Comment efface-t-on la ligne sur laquelle on se trouve dans `vi` ou
    `vim` ?
7.  vi est il installé par défaut sur tout système UNIX ?
8.  A quoi sert le pipe `|` ?
9.  Quels seront les résultats des commandes `ls | grep -v documents` et
    `ls | grep -iv documents` dans le repertoire contenant :

``` bash
Bureau
Documents
Images
Musique
```

10. Quelle doit être la première ligne d'un script shell ?

## Exercice 3

> Manipulation et recherche de fichiers

### Partie 1

1.  Se positionner dans le repertoire personnel de l'utilisateur.
2.  Comment vérifier qu'on est bien dans le repertoire personnel ?
3.  Lister les fichiers présents dans `/usr/bin` commençant par un "c"
    et sauvegarder le résultat dans le fichier `c.liste`. Quel est le
    chemin absolu du fichier `c.liste` ?
4.  Afficher seulement les fichiers de `c.liste` contenant `"char"` dans
    leur nom. Comment peut-on obtenir ce résultat sans créer le fichier
    `c.liste` ?
5.  Quel est le type du fichier `/usr/bin/crontab` ?
6.  Créer, en une seule commande, le répertoire `documents/perso/temp`.
7.  Copier les fichiers présents dans `/usr/bin` commençant par un "c"
    dans le répertoire `documents/perso/temp`. Combien le répertoire
    `documents/perso/temp` contient-il de fichiers ?
8.  Déplacer le fichier `documents/perso/temp/crontab` dans le
    répertoire personnel de l'utilisateur en le renommant `moncrontab`.
    Combien le répertoire `documents/perso/temp` contient il de fichiers
    maintenant ?
9.  Supprimer le fichier `moncrontab` et le répertoire
    `documents/perso`.
10. Dans quel répertoire se trouve la commande `passwd` ?
11. Dans quel répertoire se trouve le fichier `boot.log` ?
12. Avez-vous utilisé la même commande pour trouver les deux fichiers ?

> Commandes utiles : `file`, `wc`

### Partie 2

1.  Dans votre répertoire d'accueil, créer l'arborescence suivante, en
    n'utilisant que des chemins relatifs


```
    rep1
       |---fich11
       |---fich12
       |---rep2
          |---fich21
          |---fich22
       |---rep3
          |---fich31
          |---fich32      
```

2.  Comment déplacer toute l'arborescence rep3 sous le repertoire rep2 ?
    Supprimer tout sauf rep1, fich11 et fich12.

## Exercice 4

> Gestion des droits

1.  En utilisant les commandes mkdir, echo et cat, créez dans un nouveau
    répertoire le fichier `bienvenue` contenant la ligne de commande
    `echo Bienvenue dans le monde Unix`. Exécutez ce fichier.

2.  En utilisant les commandes `mkdir, echo, cp, chmod, cat` créer un
    fichier que vous pouvez lire et supprimer mais que vous ne pouvez
    pas modifier.

3.  En utilisant les commandes `mkdir, echo, cp, chmod, cat` créer un
    fichier que vous pouvez lire mais que vous ne pouvez ni modifier, ni
    supprimer.

4.  Vous travaillez avec un collègue appartenant au même groupe que vous
    sur la machine (serveur). Modifier les permissions du fichier crée à
    la question 1 (fichier `bienvenue`) de telle façon que votre
    collègue puisse le lire et l'exécuter, mais ne puisse pas le
    modifier ni le supprimer. Pouvez-vous modifier les permissions de ce
    fichier de telle sorte que votre collègue puisse le lire, le
    modifier et l'executer alors que vous-même ne pouvez pas le modifier
    ?

5.  Comment est attribuée la permission d'effacer un fichier ? ``{=html}

## Exercice 5

> Programmation shell

1.  Écrire un script shell qui écrit sur sa sortie standard les messages
    suivants :


```
    mon nom est xxxx
    je suis appelé avec yyy arguments
    qui sont : 111 222 333 444
```

où `xxx` sera remplacé par le nom sous lequel ce shell script a été
invoqué, `yyy` par le nombre d'arguments et `111, 222,` etc. par les
arguments en question.

2.  Quand ce script fonctionnera correctement, invoquez-le avec les 5
    arguments suivants: `Bienvenue dans le monde Unix`
3.  Puis avec un seul argument contenant la chaîne de caractères
    `"Bienvenue dans le monde Unix"`

## Exercice 6

> Programmation shell

En utilisant exclusivement les commandes `cd` et `echo`, écrire le shell
script `recurls` réalisant la même fonction que la commande `ls -R`.
C'est à dire que la commande `recurls rep1` devra lister les noms de
tous les fichiers et repertoires situés sous le répertoire `rep1`, y
compris les sous-repertoires et les fichiers qu'ils contiennent. Une
solution très simple consiste à rendre le script `recurls` récursif (le
script s'invoque lui même).

## Exercice 7

> Programmation shell

Écrivez le script `rename` permettant de renommer un ensemble de
fichiers. Par exemple `rename '.c' '.bak'` aura pour effet de renommer
tous les fichiers d'extension `.c` en `.bak`. Les fichiers `f1.c` et
`f2.c` deviennent `f1.bak` et `f2.bak`. Utiliser la commande `basename`.

## Exercice 8

> Programmation shell

Créer un script qui vous propose le menu suivant :

    1 - Vérifier l'existence d'un utilisateur
    2 - Connaître l'UID d'un utilisateur
    q - Quitter

Le script doit réaliser l'action demandée par l'utilisateur·ice.

## Exercice 9

> Programmation shell, gzip

L'espace disque sur les serveurs est précieux. Une idée pour économiser
: écrire un script qui recherche dans toute mon arborescence tous les
fichiers qui n'ont pas été accédés depuis un temps `T` et dont la taille
est supérieure à `MIN`, et les compresser avec le programme `gzip`. `T`
et `MIN` sont des constantes définies au début du script avec des
valeurs judicieusement choisies.

> Un tel script pourrait être lancé une fois par semaine (par ex. via le
> démon `cron`, voir [exercice 13](#exercice-13--tâches-périodiques)).

## Exercice 10

> Tableaux, fonctions

1.  Définissez un tableau indexé avec les données suivantes :
    `red, yellow, green, black, white`.
2.  Utilisez une boucle `for` pour parcourir ce tableau et afficher
    chaque couleur.
3.  Utilisez une boucle `while` pour parcourir ce tableau et afficher
    chaque couleur avec son indice.
4.  Créez une fonction qui compte le nombre de lettres dans chaque
    couleur et affiche ce nombre.

## Exercice 11 : Grep

> Les solutions se trouvent toutes dans les pages de `man` des commandes
> en question. On suppose donc connues les commandes de `less`, qui
> servent à se déplacer dans les pages de `man`... et la commande
> servant à chercher un mot. Testez les commandes sur des fichiers et
> répertoires d'essai pour vous faire la main et comprendre ce qui se
> passe.

Pour chaque question, **proposer une commande** à appliquer au fichier
`sonnets.txt` (contant les sonnets de Shakespeare) ou aux fichiers
contenus dans le dossier `sonnets` (1 sonnet par fichier) fourni avec
l'exercice.

1.  Quelles sont les options de `grep` qui permettent d'obtenir des
    lignes de contexte (qui précèdent et/ou suivent la ligne où figure
    le mot) ?
2.  Comment faire apparaître le numéro de la ligne où figure le mot
    recherché ?
3.  Que se passe-t-il (affichage) quand on demande également des lignes
    de contexte ?
4.  Comment faire pour afficher le nombre d'occurrences du mot recherché
    ?
5.  Comment faire pour que `grep` ignore la casse des caractères
    (différence entre majuscules et minuscules) dans sa recherche ?
6.  Comment faire pour que `grep` ne recherche que les lignes où figure
    le mot tel quel, et non pas ses variantes ?
7.  Comment faire pour faire apparaître non pas les lignes où figurent
    le mot, mais les noms des fichiers ?
8.  Comment faire pour chercher plusieurs mots à la fois en faisant
    apparaître les numéros des lignes ?
9.  Chercher toutes les lignes commençant par un caractère (majuscule et
    minuscule).
10. Chercher toutes les lignes finissant par un groupe de caractères.
11. Chercher toutes les lignes commençant par «B», «E» ou «Q».

## Exercice 12 : Créer et gérer un service avec `systemd`

> Se familiariser avec `systemd`, savoir créer, inspecter et utiliser un
> service

L'objectif est de créer un script *shell* basique qui sera exécuté comme
un service par `systemd`. Ce service va simplement écrire un message
dans un fichier de *log* à chaque fois qu'il est démarré.

1.  **Créer** un script shell `foo.sh` et le placer dans l'endroit
    approprié sur le système de fichiers UNIX. **Justifier ce choix**.
    Le script contient une instruction qui écrit
    `"Le service foo a démarré à cette date : dd/mm/YYYY HH:mm:ss"` dans
    un fichier de log situé également dans l'endroit approprié sur le
    système de fichiers UNIX. Pour éditer le fichier de script, utiliser
    l'éditeur `vi` `nano` ou `emacs`;
2.  **Rendre** le script exécutable;
3.  **Créer** un fichier de configuration du service dans
    `/etc/systemd/system/foo.service`. Voici le *template* d'un fichier
    *unit* de `systemd` :

``` ini
[Unit]
Description=?
After=network.target

[Service]
ExecStart=?
Type=?

[Install]
WantedBy=multi-user.target
```

En vous aidant de la documentation officielle (`man systemd.ini`) ou de
[cet article publié par
RedHat](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/using_systemd_unit_files_to_customize_and_optimize_your_system/assembly_working-with-systemd-unit-files_working-with-systemd#assembly_working-with-systemd-unit-files_working-with-systemd),
**remplacer** les valeurs `?` par les valeurs appropriées.

4.  **Recharger** la configuration de `systemd` avec la commande
    `systemctl daemon-reload` pour prendre en compte le nouveau service
    défini par le fichier `foo.service`.
5.  **Démarrer** le service. **Vérifier** qu'il a fonctionné.
6.  **Activer** le service au démarrage. **Redémarrer** la VM et
    vérifier qu'il a fonctionné.
7.  **Désactiver** le service au démarrage.
8.  **Créer** un deuxième service `bar` qui contient une instruction qui
    écrit
    `"Le service bar a démarré à cette date : dd/mm/YYYY HH:mm:ss"` dans
    un fichier de log situé également dans l'endroit approprié sur le
    système de fichiers UNIX. **Ce service doit toujours s'exécuter
    toujours avant le service `foo`**.
9.  **Afficher** les logs du service `foo`. **Afficher-les ensuite au
    format JSON**. Pour cela, trouver une option de `journalctl` qui
    pourrait vous être utile (`man journalctl`)
10. **Désactiver** les deux services.

## Exercice 13 : tâches périodiques

> crontab, anacron

1.  Le processus `crond` est-il lancé au démarrage ? actif ?
2.  Quelle est la différence entre `cron` et `anacron` ?
3.  Préparer les tâches suivantes :
    1.  `root` fait enregistrer de 9h à 17h, les jours ouvrables :
        1.  toutes les heures, dans `/var/log/processus.txt`, tous les
            processus qui tournent sur la machine;
        2.  tous les 15 minutes, dans `/var/log/qui.txt`, tous les
            utilisateurs connectés.
    2.  L'utilisateur `john` ajoute toutes les 5 minutes un message
        `"Bonjour"` suivi de la date, dans le fichier `/tmp/bonjour.txt`
    3.  Sauvegarde quotidienne des répertoires personnels. Il s'agit
        d'effectuer une tâche quotidienne de sauvegarde globale des
        répertoires personnels présents dans `/home` dans un répertoire
        `var/save/` à créer :
        1.  **Écrire** la commande d'archivage compressé de `/home/*`
            dans un fichier `home.tgz` à placer dans `/var/save`;
        2.  La sauvegarde doit être quotidienne. À l'aide de la commande
            `date`, **écrire** un script permettent l'archivage du 12
            nov dans un fichier nommé `home.12nov.tgz`;
        3.  Automatiser cette tâche avec `cron`, à 1h du matin.

## Exercice 14 : RGPD

Pour se confirmer au RGPD, une société souhaite mettre en place une
solution automatisée qui anonymise et archive les données sensibles
présentes dans leur base relationnelle en production. La société veut
générer des rapports sur leur activité commerciale tous les ans (le 22
décembre à 4h du matin).

La base de données MySQL contient, entre autres, des informations sur
les clients (nom, prénom, email, adresse, mot de passe) et leurs
factures (montant total, date).

1.  [D'après votre travail de veille](#références), **identifier** **les
    données personnelles** ainsi que **la ou les durées de
    conservation** adéquates. **Identifier un processus** adéquat pour
    conformer le système au RGPD.

2.  **Mettre en place la solution** à l'aide de tâches cron et de
    scripts shell. Pour cela, produire une base de données minimale avec
    un jeu de données de test (avec MySQL par exemple).

3.  **Mettre en place la génération automatique de rapports** (CA TTC
    total par an et par mois, CA TTC moyen annuel et mensuel) couvrant à
    la fois les données *actives* (en production) et les données
    *archivées* (base intermédiaire). Les rapports peuvent concerner
    n'importe quelle période passée.

### Références

-   [Guide RGPD du
    développeur](https://www.cnil.fr/fr/guide-rgpd-du-developpeur)
-   [L'anonymisation de données
    personnelles](https://www.cnil.fr/fr/technologies/lanonymisation-de-donnees-personnelles)
-   [CNIL : Les durées de conservation des
    données](https://www.cnil.fr/fr/passer-laction/les-durees-de-conservation-des-donnees)

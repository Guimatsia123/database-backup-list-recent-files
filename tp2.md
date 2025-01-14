# Travail pratique 2

## Identification

- Cours      : Utilisation et administration des systèmes informatiques
- Sigle      : INF1070
- Session    : Automne 2023
- Groupe     : <020>
- Enseignant : <Abendi Moussa>
- Auteur     : <MARIUS GUIMATSIA AKALONG> (<GUIM27309006>)


## Solution de l'exercice 0

### État de l'exercice: résolu

La solution de l'exercice 0 se trouve dans le script `remise/archiver_tp`.

Pour archiver le travail pratique, on se place à la racine du projet et on entre la commande

```sh
remise/archiver_tp
```

qui produit l'archive `tp2.tgz`. 
Ensuite, il suffit de déposer le fichier `tp2.tgz` sur Moodle!


## Solution de l'exercice 1

### État de l'exercice: résolu.
* Explication 
Tout d'abord il faut préciser que `$#` est une variable spéciale de bash qui permet de vérifier 
si les variables ont été passé en paramètre a la fonction ou pas.
Ainsi, : 
- l'instruction suivante `if [ $# -ne 1 ]` permet de vérifier si une un paramètre est paase en argument ou pas.
Si aucun paramètre n'est passe en argument alors le message suivant s'affiche: `Fournir un nom en paramètre`.
- Par contre, si une variable est passé en paramètre alors:
- `if [ -d "$1" ];` On vérifie si le premier paramètre en argument est un repertoire. Si c'est le cas rien est fait.
- Sinon, on recupere le repertoire parent dans lequel on desire creer un repertoire avec la commande  "$0" `dirname "$1"` 
puis on creer le repertoire avec le nom passer en argument grace a `mkdir "$1"`

NB: `$0` permet de specifier le nom du script et ``dirname "$1"`` permet de recuperer le repertoire dans lequel on va creer le repertoire

* Exemple d'appels
- ./mystere
Fournir un nom en paramètre
- ./mystere folder1
Va creer le repertoire folder1 dans le repertoire courant.
- ./mystere folder1/new_folder
Va creer le repertoire mew_folder dans le repertoire folder1.
- ./mystere folder1. ne fera rien puisque le repertoire folder1 existe deja. 



## Solution de l'exercice 2

### État de l'exercice: résolu.

1. Cherchons toutes les lignes commençant par a ou A

la solution est '^[aA].*' 

le symbole `^` permet de préciser par quel caractère le mot qu'on recherche doit commencer. l'option`[]` donne une liste de caractères et finalement `.*` Précise que le mot peut être suivi de 0 ou plusieurs caractères.


Exemple: 
marius@GUIM27309006:~/regex$ cat lettre_a 
Cas valide
abréger
Alexandre
Cas valide
INF1070

marius@GUIM27309006:~/regex$ cat lettre_a  | grep -E '^[aA].*'
abréger
Alexandre


2. Pour Chercher toutes les lignes finissant par rs.
La solution est '.*rs$'. 
En effet ce reg ex va chercher toutes les lignes commençant par 0 ou plusieurs caractères et finissant par un `rs` le symbole `$` permet de préciser les caractères de fin.

Exemple d'utilisation: 
`marius@GUIM27309006:~/regex$ cat rs`
Cas valide
    ```
 Abattoirs
 Abatteurs
    ```
Cas invalide
ABDOFESSIERS

`marius@GUIM27309006:~/regex$ cat rs | grep '.*rs$'`
 Abattoirs
 Abatteurs


3. Cherchons toutes les lignes commençant par une majuscule et contenant au moins un chiffre.
La solution est '^[A-Z].*[0-9]+' (ERE)
En effet `^[A-Z]` rechercher les mots commençant par une majuscule. Ensuite, `[0-9] +` recherche chiffre avec possibilité de répétition.

`marius@GUIM27309006:~/regex$ cat majuscule_chiffres | grep -E '^[A-Z].*[0-9]+' `
INF1070   
Mgl7760

4. Pour Chercher toutes les lignes commençant par B, E ou Q.
la solution est '^[BEQ].*'  

Exemple d'utilisation 

`marius@GUIM27309006:~/regex$ cat BQR`
Cas valide
Baba Bébé Ecchymotique  Eczéma Quadrangulaire
Cas invalide
École
bébé  
quatre`

`marius@GUIM27309006:~/regex$ cat BQR | grep -E '^[BEQ].*' `
Baba Bébé Ecchymotique  Eczéma Quadrangulaire

5. Chercher toutes les lignes ne finissant pas par un signe de ponctuation suivants : point, virgule, point-virgule, deux-points, point d'interrogation, point d'exclamation.
la solution est '.*[^\.,:?!;]$'

En effet, l'option `[^]` permet de chercher les lignes qui n'ont pas les caractères  lister dans les crochets.

Exemple d'utilisation :
`marius@GUIM27309006:~/regex$ cat ponctuations `

Cas valide
Ceci est une démo Salut Comment  
`et ensuite
`Belle finission
Cas invalide
`Ceci est une démo. 
`Salut !
`Comment ?
`et ensuite :
`Belle finission;

`marius@GUIM27309006:~/regex$ cat ponctuations | grep -E '.*[^\.,:?!;]$' `
Cas valide
Ceci est une démo Salut Comment  
`et ensuite
`Belle finission
Cas invalide

6. Cherchons tous les mots dont la seconde lettre est un r
La solution est '^.r.*'

Exemple d'utilisation:

`marius@GUIM27309006:~/regex$ cat 2r`
Cas valide
Arbre` 
arachide` 
Arabes` 
Cas invalide
Bonjour` 
Université` 
Montréal` 

`marius@GUIM27309006:~/regex$ cat 2r | grep -E '^.r.*' `
Arbre` 
arachide` 
Arabes` 



7. Cherchons tout numéro de téléphone correspondant à l'une des formes valides 


La solution '^\+?9{1,2}?\s*?\(?9{3}\)?9{3}[ .-](9{4})'

En effet, l'option `\+?9{1,2}` capture les ligne commencant par + (optionnel) suivi de un ou deux 9. `\s.*` pour preciser zero ou plusieurs espaces. `9{3}` suivi de 3 chiffres et fini par  `9{4}` 4 neuf 

`marius@GUIM27309006:~/regex$ cat numbers `
Cas valide
(999)999-9999
+9(999)999-9999

+99(999)999-9999  
9(999)999-9999 
99(999)999-9999 
(999)999.9999
    
(999)999 9999  
99 (999)999-9999  
+99 (999)999-9999  
999999-9999



+9 999 999-9999-9
8(999)999 9999
+9(999)999 999
+9(999)999 999990
449 999 0290


`marius@GUIM27309006:~/regex$ cat numbers | grep -E '^\+?9{1,2}?\s*?\(?9{3}\)?9{3}[ .-](9{4})'`
(999)999-9999
+9(999)999-9999
+99(999)999-9999  
9(999)999-9999 
99(999)999-9999 
(999)999.9999
(999)999 9999  
99 (999)999-9999  
+99 (999)999-9999  
999999-9999





8. Cherchons toutes les dates valides sous la forme AAAA-MM-DD ou AAAA-M-D ou AAAA-MM-D ou AAAA-M-DD considérant que :

La solution '^\d{4}-(1[012])-(0?[1-9]|(0?[1-9]))|([12][0-9])|(3[01])$' 


9. Chercher et ne capturer que les schémas (https ou ftp) des URLs dont l'autorité est de la forme


la solution '(https|ftp)://(.)\2{2}\.[[:alnum:]]{1,}\.[[:alpha:]]{2,3}(.*?)'

On va rechercher les urls commencant soit par `(https|ftp)` https ou ftp suivi de '//' puis de deux caracteres `(.)\2` puis d'un point puis d'au moins un caratere alphanumerique
[[:alnum:]]{1,}. Finalement l'option `(.*?)` est une extension Perl qui permet de stopper l'avarice. 

Exemple d'utilisation: 

`marius@GUIM27309006:~/regex$ cat http` 
Cas valide

https://www.welovedevs.com (seulement https est capturé)  
ftp://ttt.fileZilla.com    (seulement ftp est capturé)
Cas invalide
http://www.welovedevs.com  (aucune capture)
https://abc.download.net  (aucune capture)
ghttps://.www.ebay.com


`marius@GUIM27309006:~/regex$ cat http | grep -P '(https|ftp)://(.)\2{2}\.[[:alnum:]]{1,}\.[[:alpha:]]{2,3}(.*?)'`
https://www.welovedevs.com (seulement https est capturé)  
ftp://ttt.fileZilla.com    (seulement ftp est capturé)


## Solution de l'exercice 3

### État de l'exercice: résolu.



La solution de l'exercice 5 se trouve dans le script `tp2/recent/recent`.

#Description du script 
On transforme tout les repertoires se trouvant dans PATH en tableau de repertoire en se basant sur le separateur :. 
On parcourt chaque repertoire du tableau transformer precedement.

Pour chaque repertoire, on verifie si y a des fichiers executable si oui on les affiches.

#Scenarios de test

#Scenario 1: 
`marius@GUIM27309006:~/Bureau/tp2/lsexec$ ./lsexec` 
/usr/local/bin/lsexec
/usr/sbin/a2disconf
/usr/sbin/a2dismod
/usr/sbin/a2dissite
/usr/sbin/a2enconf
/usr/sbin/a2enmod
[...]
/snap/bin/firefox.geckodriver
/snap/bin/geckodriver
/snap/bin/snap-store
/snap/bin/snap-store.ubuntu-software
/snap/bin/snap-store.ubuntu-software-local-file

#Scenario 2:

`marius@GUIM27309006:~/Bureau/tp2/lsexec$ PATH=~ ./lsexec`
/home/marius/ave
/home/marius/backup-dbs
/home/marius/exe
/home/marius/exempl
/home/marius/exemple
[..]
/home/marius/recherche.sh
/home/marius/script.sh
/home/marius/test_backup
/home/marius/test_conditions
/home/marius/testons
/home/marius/test_script

#Scenario 3:
`marius@GUIM27309006:~/Bureau/tp2/lsexec$ PATH=~/Bureau/ ./lsexec`


## Solution de l'exercice 4

### État de l'exercice: résolu, partiellement résolu ou non résolu
 
# Création d'une nouvelle image à sauvegarder dans DockerHub

1. La commande suivante me permet de  télécharger l’image officielle d’ubuntu sur mon ordinateur

`sudo docker pull ubuntu`

Using default tag: latest
latest: Pulling from library/ubuntu
5e8117c0bd28: Pull complete 
Digest: sha256:8eab65df33a6de2844c9aefd19efe8ddb87b7df5e9185a4ab73af936225685bb
Status: Downloaded newer image for ubuntu:latest
docker.io/library/ubuntu:latest


2. création et execution du container container-admin

`sudo docker run --name containers-admin -it ubuntu`

root@4842e4579a81:/# 


3.  Installation des outils suivants dans le conteneur containers-admin 
On fait d'abord la mis a jour avec la commande `apt-get update`, ensuite on fait `apt-get install`  pour installer les outils sur le container. Ensuite la commande `which` permet d'avoir la version de chaque outils installer.

`apt-get update ; apt-get install -y lsb-release curl gpg gcc libjemalloc-dev pkg-config make ; which lsb-release; which curl; which gpg; which gcc; which libjemalloc-dev; which pkg-config; which make`

Get:1 http://security.ubuntu.com/ubuntu jammy-security InRelease [110 kB]
Get:2 http://archive.ubuntu.com/ubuntu jammy InRelease [270 kB]
Get:3 http://security.ubuntu.com/ubuntu jammy-security/multiverse amd64 Packages [44.0 kB]
Get:4 http://archive.ubuntu.com/ubuntu jammy-updates InRelease [119 kB]
Get:5 http://archive.ubuntu.com/ubuntu jammy-backports InRelease [109 kB]
Get:6 http://archive.ubuntu.com/ubuntu jammy/multiverse amd64 Packages [266 kB]
Get:7 http://archive.ubuntu.com/ubuntu jammy/universe amd64 Packages [17.5 MB]
Get:8 http://security.ubuntu.com/ubuntu jammy-security/restricted amd64 Packages [1512 kB]

[...]
Setting up libfile-fcntllock-perl (0.22-3build7) ...
Setting up gcc (4:11.2.0-1ubuntu1) ...
Setting up pkg-config (0.29.2-1ubuntu3) ...
Setting up gnupg (2.2.27-3ubuntu2.1) ...
Processing triggers for libc-bin (2.35-0ubuntu3.4) ...
Processing triggers for ca-certificates (20230311ubuntu0.22.04.1) ...
Updating certificates in /etc/ssl/certs...
0 added, 0 removed; done.
Running hooks in /etc/ca-certificates/update.d...
done.
/usr/bin/curl
/usr/bin/gpg
/usr/bin/gcc
/usr/bin/pkg-config
/usr/bin/make


4. Installation et compilation de redis depuis le fichier source. 
On installe d'abord l'outil `wget` avec la commande `apt-get install`
- La commande suivante `wget https://download.redis.io/redis-stable.tar.gz` permet de telecharger la dernier version du fichier source de redis.

--2023-12-11 02:56:05--  https://download.redis.io/redis-stable.tar.gz
Resolving download.redis.io (download.redis.io)... 45.60.131.1
Connecting to download.redis.io (download.redis.io)|45.60.131.1|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 3477620 (3.3M) [application/octet-stream]
Saving to: 'redis-stable.tar.gz'

redis-stable.tar.gz                                100%[================================================================================================================>]   3.32M  10.3MB/s    in 0.3s    

2023-12-11 02:56:06 (10.3 MB/s) - 'redis-stable.tar.gz' saved [3477620/3477620]

- Pour compiler redis , j'extrais d'abord le fichier avec la commande `tar -xzvf redis-stable.tar.gz` , Puis je me place dans le repertoire avec la commande `cd redis-stable`,
finalement je fais la commande `make` pour compiler. 
CC adlist.o
    CC quicklist.o
    CC ae.o
    CC anet.o
    CC dict.o
    CC server.o
    CC sds.o
    CC zmalloc.o
    CC lzf_c.o
    CC lzf_d.o
    CC pqsort.o
    CC zipmap.o
    CC sha1.o
    CC ziplist.o
    CC release.o
    CC networking.o
    CC util.o
    CC object.o
    CC db.o
    CC replication.o
    CC rdb.o
    CC t_string.o
    CC t_list.o
    CC t_set.o
    CC t_zset.o
    CC t_hash.o
    CC config.o
    CC aof.o
    CC pubsub.o
    CC multi.o
    CC debug.o
    CC sort.o
    CC intset.o
    CC syncio.o
    CC cluster.o
    CC crc16.o
    CC endianconv.o
    CC slowlog.o
    CC eval.o
    CC bio.o
    CC rio.o
    CC rand.o
    CC memtest.o
    CC syscheck.o
    CC crcspeed.o
    CC crc64.o
    CC bitops.o
    CC sentinel.o
    CC notify.o
    CC setproctitle.o
    CC blocked.o
    CC hyperloglog.o
    CC latency.o
    CC sparkline.o
    CC redis-check-rdb.o
    CC redis-check-aof.o
    CC geo.o
    CC lazyfree.o
    CC module.o
    CC evict.o
    CC expire.o
    CC geohash.o
    CC geohash_helper.o
    CC childinfo.o
    CC defrag.o
    CC siphash.o
    CC rax.o
    CC t_stream.o
    CC listpack.o
    CC localtime.o
    CC lolwut.o
    CC lolwut5.o
    CC lolwut6.o
    CC acl.o
    CC tracking.o
    CC socket.o
    CC tls.o
    CC sha256.o
    CC timeout.o
    CC setcpuaffinity.o
    CC monotonic.o
    CC mt19937-64.o
    CC resp_parser.o
    CC call_reply.o
    CC script_lua.o
    CC script.o
    CC functions.o
    CC function_lua.o
    CC commands.o
    CC strl.o
    CC connection.o
    CC unix.o
    CC logreqres.o
    LINK redis-server
lto-wrapper: warning: using serial compilation of 31 LTRANS jobs
    INSTALL redis-sentinel
    CC redis-cli.o
    CC redisassert.o
    CC cli_common.o
    CC cli_commands.o
    LINK redis-cli
lto-wrapper: warning: using serial compilation of 4 LTRANS jobs
    CC redis-benchmark.o
    LINK redis-benchmark
lto-wrapper: warning: using serial compilation of 2 LTRANS jobs
    INSTALL redis-check-rdb
    INSTALL redis-check-aof

Hint: It's a good idea to run 'make test' ;)

make[1]: Leaving directory '/redis-stable/src'

- Pour copier l'exécutable redis-cli dans le répertoire /usr/local/bin, je fais la commande `sudo make install` 
cd src && make install
make[1]: Entering directory '/redis-stable/src'
    CC Makefile.dep

Hint: It's a good idea to run 'make test' ;)

    INSTALL redis-server
    INSTALL redis-benchmark
    INSTALL redis-cli
make[1]: Leaving directory '/redis-stable/src'

- Pour afficher la version de redis redis-cli, j'installe d'abord  redis-server `apt-get install redis-server`  puis je demarre le serveur avec la commande `service redis-server start` finalement, `redis-cli INFO server` permet d'avoir la version de redis-cli.

 `root@4842e4579a81:/redis-stable/src# service redis-server start`
Starting redis-server: redis-server.


# Server INFO
redis_version:6.0.16
redis_git_sha1:00000000
redis_git_dirty:0
redis_build_id:a3fdef44459b3ad6
redis_mode:standalone
os:Linux 6.2.0-37-generic x86_64
arch_bits:64
multiplexing_api:epoll
atomicvar_api:atomic-builtin
gcc_version:11.2.0
process_id:9652
run_id:54a335bb46f607775522fb0d26bb856195ec60a5
tcp_port:6379
uptime_in_seconds:121
uptime_in_days:0
hz:10
configured_hz:10
lru_clock:7765358
executable:/usr/bin/redis-server
config_file:/etc/redis/redis.conf
io_threads_active:0


`root@4842e4579a81:/redis-stable/src#  which redis-cli`
/usr/local/bin/redis-cli


#Sauvegarde, sur DockerHub du containers-admin comme une nouvelle image containers-admin:v1

-On se connecte a notre compte creer sur docker hub avec la commande `sudo docker login` Puis on va rentrer le username et le mot de passe.
marius@GUIM27309006:~$ docker login
Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.
Username: marius237
Password: 
WARNING! Your password will be stored unencrypted in /root/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded

- Puis on push notre image avec la commande `sudo docker push marius237/containers-admin:v1`
marius@GUIM27309006:~$ sudo docker push marius237/containers-admin:v1
The push refers to repository [docker.io/marius237/containers-admin]
8ceb9643fb36: Mounted from library/ubuntu 
v1: digest: sha256:3062b8dd74436199b41b46ebddc3bae5f75f4dae46fe7da0f69bdb48bf43ba3a size: 529

Finalement le lien est [docker.io/marius237/containers-admin]



## Solution de l'exercice 5

### État de l'exercice: résolu.

La solution de l'exercice 5 se trouve dans le script `tp2/recent/recent`.

1. Description du script `recent`

On vérifie si le nombre d'arguments passés en argument est dans l'intervalle 2 et 5
Si l'utilisateur a saisir deux paramètres alors 
On vérifie si le premier argument est `-f` et si le l'argument 2 est un répertoire
On récupère le chemin complet du fichier recently-used.xbel  ou se trouve le répertoire
On vérifie si le fichier recently-used.xbel existe
On utilise grep pour extraire les lignes qui contiennent les balises <bookmark href="">
Par défaut le nombre de fichiers à afficher est n=10
 On extrait les noms de fichier qui sont dans les tags href et les dates dans les tags visited
On affiche le résultat en console. 
 Si le nombre d'arguments en paramètres est 4 alors on parcourt la liste des arguments 
 On vérifie si les arguments saisies sont les bons c'est a dire `-f` `-n` un nombre entier et un chemin vers le fichier  recently-used.xbel
 Si les arguments ne sont pas `-f`  ou `-n` on renvoi le code d'erreur 4
 S'il y a pas de valeur numérique on renvoie le code d'erreur 2
 Si tous les arguments saisies sont bons alors on lance la recherche des fichiers récemment visités
 On lance la recherche des n fichiers récents visites. n étant le nombre passé en argument après l'option `-n 4` par exempleDécrire votre solution ici.


2. Scenarios de test:

#Scenario 1:

Usage minimal, avec l'option -f et un chemin vers un fichier recently-used.xbel.
Ça affiche donc les 10 derniers fichiers ouverts par défaut. Le plus récent est en premier.

`marius@GUIM27309006:~/Bureau/tp2/recent$ ./recent -f recently-used.xbel`
lISTE DES FICHIERS RECEMMENTS VISITES
Nom de fichier , Date de visite
/home/marius/Bureau/tp2/recent/recent,   2023-12-10T21:02:09.101931Z
/home/marius/Bureau/tp2/recent,   2023-12-10T20:25:39.711928Z
/home/marius/T%C3%A9l%C3%A9chargements/README.md,   2023-12-10T17:15:39.259775Z
/home/marius/test_backup,   2023-12-10T15:15:00.768924Z
/home/marius/lsexec,   2023-12-10T15:07:19.153324Z
/home/marius/Bureau/tp2/backup/backup-dbs,   2023-12-08T05:08:20.161417Z
/home/marius/Bureau/tp2/backup,   2023-12-08T04:13:36.594768Z
/home/marius/recent_ok,   2023-12-08T01:35:48.392021Z
/home/marius/.cache/.fr-y4R6LP/script/Question,   2023-12-08T01:08:21.075748Z
/home/marius/.cache/.fr-dWEY5p/script/ListeFich,   2023-12-08T01:07:56.909007Z

`marius@GUIM27309006:~/Bureau/tp2/recent$ ./recent -f recently-used.xbel -n 5`
lISTE DES FICHIERS RECEMMENT VISITES
Nom de fichier , Date de visite
/home/marius/Bureau/tp2/recent/recent,   2023-12-10T21:02:09.101931Z
/home/marius/Bureau/tp2/recent,   2023-12-10T20:25:39.711928Z
/home/marius/T%C3%A9l%C3%A9chargements/README.md,   2023-12-10T17:15:39.259775Z
/home/marius/test_backup,   2023-12-10T15:15:00.768924Z
/home/marius/lsexec,   2023-12-10T15:07:19.153324Z

#Scenario 2: 

Valeur de retour pour moins de 2 arguments ou plus que 5 arguments.

`marius@GUIM27309006:~/Bureau/tp2/recent$ ./recent `
./recent:Le nombre d'argument n'est pas correct 
`marius@GUIM27309006:~/Bureau/tp2/recent$ echo $?`
1
`marius@GUIM27309006:~/Bureau/tp2/recent$ ./recent -f`
./recent:Le nombre d'argument n'est pas correct 
marius@GUIM27309006:~/Bureau/tp2/recent$ echo $?
1
`marius@GUIM27309006:~/Bureau/tp2/recent$ ./recent recently-used.xbel`
./recent:Le nombre d'argument n'est pas correct 
`marius@GUIM27309006:~/Bureau/tp2/recent$ echo $?`
1

#Scenario 3:

Valeur de retour pour l'option -f et le chemin pour un fichier recently-used.xbel.
`marius@GUIM27309006:~/Bureau/tp2/recent$ ./recent -f fail`
./recent : le fichier est non valide ou introuvable

`marius@GUIM27309006:~/Bureau/tp2/recent$ echo $?`
2

`marius@GUIM27309006:~/Bureau/tp2/recent$ ./recent -n 4`
./recent : le fichier est non valide ou introuvable

`marius@GUIM27309006:~/Bureau/tp2/recent$ echo $?`
2
`marius@GUIM27309006:~/Bureau/tp2/recent$ ./recent -f /home/moot`
./recent : le fichier est non valide ou introuvable
`marius@GUIM27309006:~/Bureau/tp2/recent$ echo $?`
2

#Scenario 4:

`marius@GUIM27309006:~/Bureau/tp2/recent$ ./recent -f recently-used -n` 
./recent :  n doit être un nombre entier strictement supérieur à 0

`marius@GUIM27309006:~/Bureau/tp2/recent$ echo $?`
3

`marius@GUIM27309006:~/Bureau/tp2/recent$ ./recent -f recently-used -n 10x`
./recent : saisir un entier strictement supérieur à 0

`marius@GUIM27309006:~/Bureau/tp2/recent$ echo $?`
3

`marius@GUIM27309006:~/Bureau/tp2/recent$ ./recent -f recently-used -n a7`
./recent : saisir un entier strictement supérieur à 0

#Scenario 5:
Les options -f et -n sont incorrects

`marius@GUIM27309006:~/Bureau/tp2/recent$ ./recent -f recently-used -x 2`
./recent : option inconnue 
`marius@GUIM27309006:~/Bureau/tp2/recent$ echo $?`
4
`marius@GUIM27309006:~/Bureau/tp2/recent$ ./recent -d recently-used -x 4`
./recent : option inconnue 
`marius@GUIM27309006:~/Bureau/tp2/recent$ echo $?`
4


## Solution de l'exercice 6

### État de l'exercice: résolu.

Décrire votre solution ici.

La solution de l'exercice 6 se trouve dans le script `tp2/backup/backup-dbs`.

I. Creation du script backup-dbs

1. Description du script `backup-dbs` 

On vérifie si l'utilisateur a passé un argument en paramètre
-On vérifie si cet argument est un répertoire.
-On récupère la date de sauvegarde. et on crée les répertoires de sauvegarde `./sql` et `./mybackups`
-On récupère le répertoire parent du répertoire passé en argument.
-On crée les répertoires de sauvegarde `./sql` et `./mybackups`
-On parcourt les répertoires si on trouve un fichier avec l'extension `.db` alors on procède à sa sauvegarde, sa compression au format `.tar.gz` et finalement on supprime tout les fichier `.sql` se trouvant dans les répertoires `sql/` et `mybackups`
-On récupère le nom du fichier à sauvegarder on lui enlève l'extension .db et on renomme le fichier au format db1-YYYYMMDDHHMM.sql
-On exporte le fichier sous le nom qu'on a  créé précédemment dans le répertoire sql.
-Création de l'archive .tar.gz avec la commande tar dans le répertoire de sauvegarde mybackups
-Si aucune base de donnée ne se trouve dans le répertoire passé en argument on renvoi un message d'erreur avec code 1
-Si l'utilisateur a fourni plus d'un argument en paramètre ou pas du tout on renvoi un message d'erreur avec code 1
-On récupère le nom du fichier à sauvegarder on lui enlève l'extension .db et on renomme le fichier au format db1-YYYYMMDDHHMM.sql
-On exporte le fichier sous le nom qu'on a  créé précédemment dans le répertoire sql.
-Création de l'archive .tar.gz avec la commande tar dans le répertoire de sauvegarde mybackups
-Si aucune base de donnée ne se trouve dans le répertoire passé en argument on renvoi un message d'erreur avec code 1
-Si l'utilisateur a fourni plus d'un argument en paramètre ou pas du tout on renvoie un message d'erreur avec code 1er en argument.
-On crée les répertoires de sauvegarde `./sql` et `./mybackups`
-On parcourt les répertoires si on trouve un fichier avec l'extension `.db` alors on procède à sa sauvegarde, sa compression au format `.tar.gz` et finalement on supprime tout les fichier `.sql` se trouvant dans les répertoires `sql/` et `mybackups`
-On récupère le nom du fichier à sauvegarder on lui enlève l'extension .db et on renomme le fichier au format db1-YYYYMMDDHHMM.sql
-On exporte le fichier sous le nom qu'on a  créé précédemment dans le répertoire sql.
-Création de l'archive .tar.gz avec la commande tar dans le répertoire de sauvegarde mybackups
-Si aucune base de donnée ne se trouve dans le répertoire passé en argument on renvoi un message d'erreur avec code 1
-Si l'utilisateur a fourni plus d'un argument en paramètre ou pas du tout on renvoi un message d'erreur avec code 1

2. Scenarios de test:

#Scenario 1:
`marius@GUIM27309006:~/Bureau/tp2/backup$ ./backup-dbs` 
Usage: ./backup-dbs QLITE3_DBS_DIR

#Scenario 2:
`marius@GUIM27309006:~/Bureau/tp2/backup$ ./backup-dbs nodbs/ mydbs/`
Usage: ./backup-dbs QLITE3_DBS_DIR

#Scenario 3:
`marius@GUIM27309006:~/Bureau/tp2/backup$ ./backup-dbs nodbs/ `
Aucune base de données à sauvegarder dans le répertoire nodbs/ 

#Scenario 4:
`marius@GUIM27309006:~/Bureau/tp2/backup$ ./backup-dbs mydbs/`
Export de books.db sous format SQL ...
Archivage et compression dans ./mybackups/books-202312101204.sql.tar.gz ...
Export de uqam.db sous format SQL ...
Archivage et compression dans ./mybackups/uqam-202312101204.sql.tar.gz ...
Supression des fichiers SQL de toutes les bases de données ...

II- Mécanisme de planification de tâches (CRON) 

1. On ouvre le fichier crontab avec la commande `crontab -e`
2. Edition 
# Sauvegarde toute les minutes
* * * * * ~/Bureau/tp2/backup/backup-dbs

# Sauvegarde toute les 15 minutes
*/15 * * * * ~/Bureau/tp2/backup/backup-dbs

# Sauveagarde toute les heures
# La sauvegarde aura lieu a la minute 0 de chaque heure
0 * * * * ~/Bureau/tp2/backup/backup-dbs

# Sauveagrde toute les 4 heures
#La sauvegarde se fera a la minute 0 de chaque heure
0 */4 * * * ~/Bureau/tp2/backup/backup-dbs

# Sauvegarde tous les jours a 23h59
59 23 * * * ~/Bureau/tp2/backup/backup-dbs

3. Finalement on fait `crontab -l` pour enregistrer et quitter.





# À propos de ce livre

## Licence

Le petit livre sur Redis est fourni sous la licence Attribution-NonCommercial 3.0 Unported. Vous ne devriez pas avoir
payé pour l'obtenir.

Vous êtes libres de copier, distribuer, modifier ou afficher ce livre. Cependant, je demande à ce que vous attribuiez
toujours ce livre à mon, Karl Seguin, et que vous ne l'utilisiez pas dans un but commercial.

Vous pouvez accéder au *texte complet* de **la licence à l'adresse**: 

<http://creativecommons.org/licenses/by-nc/3.0/legalcode>

## À propos de l'auteur

Karl Seguin est un développeur avec de l'expériences dans différents domaines et technologies. Il est un contributeur
actifs dans de nombreux projets Open-Source, un écrivain technique et un speaker de manière occasionnelle. Il a écrit de
nombreux articles, ainsi que certains outils en lien avec Redis. Redis propulse son service gratuit de classement et de
statistique pour les développeurs occasionnels de jeux : [mogade.com](http://mogade.com/).

Karl a écrit [The Little MongoDB Book](http://openmymind.net/2011/3/28/The-Little-MongoDB-Book/), le livre gratuit et
populaire à propos de MongoDB.

Son blog peut-être trouvé à l'adresse : <http://openmymind.net> et il tweete sur le compte :
[@karlseguin](http://twitter.com/karlseguin)

## Traduction

La traduction a été réalisée par trois personnes : [David Loureiro](http://twitter.com/DavidLoureiroFr),
[Haikel Guémar](http://twitter.com/hguemar) et [Augustin Ragon](http://twitter.com/augustin82).

### David Loureiro

David Loureiro, après des études en mathématiques appliquées et ensuite de management a créé la société SysFera avec
[Eddy Caron](http://twitter.com/CaronEddy) et [Frédéric Desprez](http://twitter.com/desprez64).
SysFera est une société d'édition de logiciels avec une expertise forte dans la mise en place de solutions Open-Source
pour la gestion et l'exploitation as a Service d'applications et d'infrastructures.
Président de SysFera, le champ d'expertise de David Loureiro couvre les domaines du Cloud Computing, de l'Open-Source,
du HPC, et du SaaS, entre autres.

### Haikel Guémar

TODO

### Augustin Ragon

TODO

## Remerciements

Un remerciement tout particulier à [Perry Neal](https://twitter.com/perryneal) pour m'avoir loué ses yeux, son esprit
et sa passion. Tu m'as fourni une aide inestimable. Merci à toi.

## Dernière version

La dernière version des sources du livre original se trouve à l'adresse suivante :
<http://github.com/karlseguin/the-little-redis-book>
Vous pourrez trouver la dernière version des sources de la traduction française à l'adresse suivante :
<http://github.com/dloureiro/the-little-redis-book>

# Introduction

Au cours de ces deux dernières années, les outils et technologies utilisés pour le requêtage et la persistence des
données ont évolué à un rythme effréné. Bien qu'il soit certain que les bases de données relationnelles ne disparaitront
pas, on peut également affirmer que l'écosystème de la gestion de données ne sera plus jamais le même.

Pour ma part, de toutes les solutions et outils qui ont émergés, Redis est de loin, le plus intéressant. Pourquoi ?
Premièrement, parce qu'il est extrêmement simple d'apprentissage. Être à l'aise avec Redis est une question d'heures.
Deuxièment, il réponds à un ensemble spécifiques de cas d'utilisation tout en restant générique. Où veut-on en venir ?
Redis n'essaie pas d'être une solution fourre-tout pour tout type de données. Au fur et à mesure que vous prenez en main
Redis, il devient de plus en plus évident de distinguer ce qu'il peut faire ou non. Et quand il peut, il se révèle d'une
grande aide pour le développeur.

Bien que vous pouvez construire un système complet à l'aide de Redis uniquement, je pense que la plupart des
utilisateurs trouveront qu'il complémente assez bien leur système de gestion de données plus génériques - que ce soit
une base de données relationnelle classique, une base orienté documents ou autre. C'est le genre de solution que vous
utiliserez pour mettre en oeuvre des fonctionnalités particulières. Dans un sens, il est similaire à un moteur
d'indexation. Vous ne construirez pas votre application toute entière autour de Lucence. Mais quand vous avez besoin
d'un moteur de recherche robuste, vous profiterez d'une bien meilleur expérience - que ce soit pour vous et vos
utilisateurs. Bien entendu, les similitudes entre Redis et les moteurs d'indexation s'arrêtent là.

L'objectif de ce bouquin est de mettre en place les bases nécessaires pour que vous puissez maitriser Redis. Nous nous
concentrons sur l'apprentissage des cinq structures de données de Redis et étudierons les différentes approches de
modélisation des données. Nous toucherons également quelques mots à propos de l'administration et des techniques de
débogage.


# Mise en route

Nous apprenons tous différemment: certains aiment mettre les mains dans le cambouis, d'uatres aiment regarder des
vidéos, et certains autres aiment lire. Rien ne vous aidera mieux à comprendre Redis qu'expérimenter par vous même.
Redis est facile à installer et est livré avec un simple shell qui vous donnera tout ce dont nous avons besoin. Prenons
un peu de temps pour l'installer et le faire fonctionner sur notre ordinateur.

## Sur Windows

Redis lui-même n'est pas officiellement supporté sur Windows, mais il y a cependant certaines options possibles. Vous ne
devriez pas les faire tourner en production, mais je n'ai jamais perçu de limitation pendant le développement.

Un port par Microsoft Open Technologies, Inc. peut être trouvé à l'adresse suivante :
<https://github.com/MSOpenTech/redis>. Au moment de l'écriture de ce livre, cette solution n'est pas prête pour être
utilisée sur des systèmes en production.

Une autre solution, disponible depuis un certain temps, peut être trouvée à l'adresse suivante :
<https://github.com/dmajkic/redis/downloads>. Vous pouvez télécharger la version la plus à jour (qui devrait être en
haut de la liste). Extrayés le fichier zip et, en fonction de votre architecture, ouvrez le répertoire `64bit` ou
`32bit`.

## Sur *nix ou MacOSX

Pour les utilisateurs d'*nix ou de Mac, compiler et générer Redis depuis les sources est la meilleure option. Les
instructions, ainsi que la dernière version, sont disponibles à l'adresse suivante : <http://redis.io/download>. Au
moment de l'écriture de ce livre, la dernière version est la 2.6.2; pour installer cette version, vous devez exécuter:

	wget http://redis.googlecode.com/files/redis-2.6.2.tar.gz
	tar xzf redis-2.6.2.tar.gz
	cd redis-2.6.2
	make

(De manière alternative, Redis est disponible à travers différents gestionnaires de paquets. Par exemple, les
utilisateurs de MacOSX disposant de Homebrew peut simplement taper `brew install redis`.)

Si vous compilez et générez Redis depuis les sources, les binaires produits seront placés dans le répertoire `src`.
Naviguez jusqu'au répertoire `src` en tapant `cd src`.

## Lancer et se connecter à Redis

Si tout a fonctionné, les binaires de Redis devraient être à votre disposition. Redis possède un grand nombre
d'exécutables. Nous allons nous concentrer sur le serveur Redis et sur la ligne de commande Redis (un client DOS-like).
Démarrons le serveur. Sous Windows, double-cliquez sur `redis-server`. Sous *nix/MacOSX lancez `./redis-server`.

Si vous lisez le message de démarrage, vous devriez vour un avertissement indiquant que le fichier `redis.conf` ne peut
pas être trouvé. Redis va utiliser les paramètres par défaut en lieu et place, ce qui est correct pour ce que nous
allons faire.

Ensuite, lancez la console Redis, soit en double-cliquant sur `redis-cli` (Windows), soit en lançant `./redis-cli`
(*nix/MacOSX). Ceci va vous connecter au serveur tournant en local sur le port par défaut (6379).

Vous pouvez tester que tout fonctionne en entrant `info` dans l'interface en ligne de commande. Vous devriez normalement
voir un ensemble de paires clé-valeur fournissant un ensemble important d'éléments d'information sur le statut du
serveur.

Si vous avez des problème avec la mise en place ci-dessus, je suggère que vous recherchiez de l'aide dans le groupe de
support officiel de Redis : [official Redis support group](https://groups.google.com/forum/#!forum/redis-db).

# Drivers Redis

Comme vous allez très bientôt l'apprendre, l'API de Redis est mieux décrite par un ensemble de fonctions. Ceci amène un
sentiment de simplicité et un aspect assez procédure quand on l'utilise. Ceci signifie notamment que, que vous utilisiez
la ligne de commande, ou un driver pour votre langage préféré, les choses seront tout à fait similaires. C'est pourquoi
vous ne devriez pas avoir de problèmes durant la lecture, si vous préférez utiliser un langage de programmation. Si vous
le souhaitez, rendez vous sur la page des clients Redis [client page](http://redis.io/clients),
et téléchargez le driver approprié.

# Chapitre 1 - Les bases

Qu'est-ce qui rends Redis spécial ? Quelle classe de problèmes résout-il ? Quels sont les points d'achoppement que les
développeurs doivent faire attention lorsqu'ils l'utilisent ? Avant de pouvoir répondre à toutes ces questions, nous
devons comprendre ce qu'est Redis.

Redis est souvent décrit comme un moteur de stockage clé/valeur en mémoire persistent. Je ne crois pas que ça soit une
description exacte. En effet, Redis stocke toutes les données en mémoire (nous en saurons plus dans quelques instants),
et il persiste bien les données sur disque, mais il est bien plus qu'un simple moteur de stockage clé/valeur. Il est
essentiel de dépasser cette idée reçue sous peine de réduire votre perspective de Redis et des problèmes qu'il est
capable de résoudre.

En réalité, Redis expose cinq structures de données différentes, une seule d'entre elle peut être définie comme une
structure clé/valeur traditionnelle. En comprenant ces cinq structures de données, leur fonctionnements, leurs
interfaces et ce que vous pouvez modéliser avec, constitue la clé pour comprendre Redis. Mais commençons d'abord par
nous pencher sur la signifaction de ce qu'est l'interface d'une structure de données.

Si nous devions appliquer ce concept de structure de données au monde relationnel, nous pourrions affirmer que les
bases de données exposent une unique structure de données : les tables. Les tables sont à la fois complexe et flexible.
Il n'y a pas grand-chose que l'on ne puisse modéliser, stocker ou manipuler avec les tables. Cependant, leur nature
générique n'est pas sans inconvénients. Plus particulièrement, tout n'est pas aussi simple ou rapide, que ce que cela
devrait être. Et si, plutôt que d'avoir une seule structure de données unique, nous utilisions des structures
spécialisées ? Certes, il y aura des choses que l'on ne pourra pas faire (ou pas aisément du moins), mais ne
gagnerions-nous pas en simplicité et rapidité ?

Utiliser des structures de données spécifiques pour des problèmes spécifiques ? Mais n'est-ce pas notre façon d'agir
lorsque nous codons ? Nous n'utilisons pas d'une table de hachage pour chaque donnée, ni n'utilisons une variable
scalaire. C'est pour moi ce qui caractérise l'approche de Redis. Si vous devez gérer des scalaires, listes, listes de
hachages ou ensembles, pourquoi ne pas les stocker en tant que scalaires, listes, listes de hachages ou ensembles ?
Pourquoi vérifier l'existence d'une variable devrait être plus complexe que d'appeler `exists(key)` ou plus lent que
0(1) (recherche à temps constants qui ne ralentira pas quelque soit la quantité de données stockée) ?


# Les briques de bases

## Bases de données

Redis propose le même concept de base de données qui vous est familier. Une base de données contient un ensemble de
données. Le cas d'utilisation typique d'une base de données est de regrouper l'ensemble des données propre à une
application et de les cloisonner vis à vis des autres applications.

Dans Redis, les bases de données sont simplement identifiés par un numéro, la base de données par défaut étant `0`. Si
vous souhaitez utiliser une base différente, vous pouvez le faire via la commande `select`. Dans l'interface en ligne de
commande, tapez `select 1`. Redis devrait vous renvoyer un message `OK` et votre invite de commande devrait changer en
quelque chose ressemblant à `redis 127.0.0.1:6379[1]>`.Si vous souhaitez revenir à la base par défaut, tapez tout
simplement dans l'invite `select 0`...

## Commandes, Clés et Valeurs

Bien que Redis soit plus qu'une base clé/valeur, chacune des cinq structures de données de Redis possède au moins une
clé et une valeur. Il est impératif de comprendre les clés et les valeurs avant de passer à autre chose.

Les clés sont le mécanisme permettant d'identifier une données. Nous manipulerons beaucoup les clés par la suite, mais
il est déjà bon de savoir qu'une clé ressemblera à `users:leto`. On peut également s'attendre à ce qu'une telle clé
pointer vers des informations à propos d'un utilisateur nommé `leto`. Les deux-points n'ont pas de signification
particulière pour Redis, mais l'utilisation d'un séparateur est une pratique commune pour l'organisation des clés.

Lesvaleurs représentent les données associées à une clé. Elles peuvent être de n'importe quel type. Parfois, vous y
stockerez des chaines des caractères, parfois des entiers, parfois des objets sérialisés (en JSON, XML ou tout autre
format). La plupart du temps, Redis traite les données comme étant un tableau d'octets et ne se préoccupe pas de leurs
types. Notons que les différents pilotes gérent la sérialisation de manière différente (certains vous en laisse la
gestion), donc nous nous limiterons uniquement aux chaines de caractères, entiers et JSON pour la suite.

Mettons les mains à la pate. Tapez la commande suivante :

	set users:leto "{name: leto, planet: dune, likes: [spice]}"

C'est la structure basique pour toute commande Redis. D'abord, nous avons la commande, ici `set`. Ensuite, nous avons
les paramètres associés. La commande `set`  prends deux paramètres : la clé que nous avons définie et la valeur que nous
lui affectons. La majorité des commandes prennent en paramètre une clé (et quand c'est le cas, c'est souvent le premier
paramètre). Pouvez-vous deviner comment récupérer cette valeur ? Il est probable que vous essayeriez(mais ne vous
inquiétez-pas si vous n'étiez pas sûr !) :

	get users:leto

Allez-y et testez d'autres combinaisons. Les clés et les valeurs sont les concepts fondamentaux, et les commandes `get`
et `set` sont les outils de base pour les manipuler. Créez plus d'utilisateurs, essayez d'autres types de clés et des
valeurs différentes.

## Requêtes

Au fur et à mesure, que nous avancerons, deux choses deviendront claires. Du point de vue de Redis, les clés sont tout
et les valeurs rien. Autrement dit, Redis ne vous permets pas de faire des requêtes à propos des valeurs d'un objet.
Dans l'exemple précédent, nous ne pouvons pas retrouver le(s) utilisateur(s) habitant la planète `dune`.

Pour beaucoup, c'est un point critique. Nous vivons dans un monde où les requêtes sont tellement puissantes et flexibles
que l'approche prônée par Redis semble primitive et abbérente. Ne vous laissez pas perturber par ceci. Rappelez-vous que
Redis n'est pas une solution universelle à tout vos problèmes. Il y aura des problèmes que vous ne pourrez pas résoudre
avec Redis (entre autre à cause des requêtes limitées). Mais considérez que vous trouverez d'autres façons de modéliser
vos données.

Nous verrons quelques exemples concrets au fur et à mesure que nous avancerons, mais il est essentiel de comprendre cet
aspect bien réel de Redis. Cela nous aide à comprendre pourquoi les valeurs stockées peuvent être de tout type - Redis
n'ayant jamais besoin de les lire ou de les traityer. Cela nous aide aussi à modéliser nos données dans cette
perspective.


## Mémoire et persistence

Nous avons donc mentionné auparavant que Redis est une base en mémoire persistente. Par rapport à la persistence, par
défaut, Redis réalise des clichés de la base sur disque en fonction du nombre de clés modifiées. Vous pouvez configurer
que toutes les X clés modifiées, la base soit sauvegardée tout les Y secondes. Par défaut, Redis sauvegarde la base
toutes les 60 secondes si 1000 ou plus clés ont été modifiés ou toutes les 15 minutes si 9 ou moins ont été modifiées.

En alternative (ou en complément), Redis peut fonctionner en mode ajout. À chaque modification de clé, un fichier en
ajout est mis à jour sur le disque. Dans certains cas, il est acceptable de perdre 60 secondes de données, en
contrepartie de meilleures performances dans le cas d'une panne matérielle ou logicielle. Dans certains cas, la perte
des données est préjudiciable. Redis vous donne ce choix. Dans le chapitre 6, nous verrons qu'il existe une troisième
option qui est de déléguer la persistence à un esclave.

Vis à vis de la mémoire, Redis conserve toutes vos données en mémoire. La conséquence évidente de ceci est que la RAM
est le premier facteur de coût matériel pour un serveur Redis.

J'ai l'impression que certains développeurs n'ont plus en tête le peu d'espace que les données peuvent occuper. Les
oeuvres complètes de William Shakespeare prennent à peu près 5.5 Mo. Pour le passage à l'échelle, les autres solutions
tendent à être limités par les I/O ou la CPU. Limitation (RAM ou I/O) qui exigeront de vous de passer à l'échellle sur
plusieurs machines en fonction des données que vous devrez stocker et traiter. À moins que vous stockiez des fichiers
multimédias très large, le stockage des données en mémoire n'est pas un problème. Pour les applications où cela peut
poser problèmes, vous devrez certainement faire un compromis pour être limité par les I/O au lieu de la mémoire.

Par ailleurs, Redis gère également la mémoire virtuelle. Néanmoins, cette fonctionnalité a été considéré comme un échec
(par les propres développeurs de Redis) et a été dépréciée.

(Notons que le fichier de 5.5Mo contenant les oeuvres complètes de Shakespeare peuvent être compressé à environ 2Mo.
Redis ne compresse pas automatiquement les données, mais comme il gére des tableaux d'octets, rien ne vous empêche de le
faire vous-même.)


## Récapitulons

Nous avons abordés un certain nombre de sujets. La dernière chose que j'aimerais avant d'entrer dans les détails est de
récapituler tout ces sujets. Plus précisement, les limitations des requêtes, les structures de données et la manière
dont Redis stocke les données en mémoire.

Quand vous mettez ces trois points ensemble, vous obtenez quelque chose de fantastique : la rapidité. Certains penseront
"Bien évidemment, Redis est rapide car tout est en mémoire." Mais c'est qu'une partie de l'explication. La véritable
raison pour laquelle Redis surpasse les autres solutions sont ses structures de données spécialisées.

Comment plus rapide ? Cela dépends d'un grand nombre de facteurs - quels sont les commandes utilisées, le type des
données, etc. Mais les performances de Redis sont de l'ordre de plusieurs dizaines de milliers ou centaines de milliers
d'opérations **par seconde**. Vous pouvez lancer `redis-benchmark` (qui est dans le même répertoire que `redis-server`
et `redis-cli`) pour vous en rendre compte.

J'ai eu l'occasion de porter un code utilisant un modèle traditionnel à Redis. Un test de chargement que j'avais écrit
prenait près de 5 minutes à s'exécuter en utilisant le modèle relationnel. Il a fallu 150ms avec Redis. Vous n'aurez pas
toujours un gain aussi massif, mais cela devrait vous donnez une idée de ce dont on parle.

Il est essentiel de comprendre cet aspect de Redis parce qu'il impacte votre façon d'interagir avec. Les développeurs
avec une expérience SQL ont souvent tendance à minimiser les interrogations à la base de données. C'est une bonne
pratique avec n'importe quelle solution, Redis inclus. Cependant, étant donné que nous gérons des structures de données
plus simples, nous devons parfois interroger le serveur Redis à plusieurs reprises pour arriver à nos fins. Ces manières
d'accèder aux données peuvent sembler peu naturelles au départ, mais elles ont un coût négligeable par rapport aux gains
en performances brutes obtenus.

## Dans ce chapitre

Bien que nous avons à peine eu l'occasion de jouer avec Redis, nous avons déjà couvert un champs très large de sujets.
Ne vous inquiétez pas si certains points restent confus - comme les requêtes. Nous irons droit au but lors du chapitre
suivant et la plupart de vos questions y trouveront j'espère une réponse.

Les point principaux abordés dans ce chapitre sont :

* Les clés sont des chaines de caractères identifiant des données (valeurs)

* Les valeurs sont des tableaux d'octets qui ne sont pas traités par Redis

* Redis expose (et utilise en interne) uniquement cinq structures de données

* Ensemble, tout les points ci-dessus font de Redis une base rapide et simple d'utilisation mais pas adaptée à tout les
scénarios

# Chaptre 2 - Les Structures de Données 

Il est temps jeter un coup d'oeil aux cinq structures de données de Redis. Nous allons expliquer ce qu'est chaque
structure de données, quelles sont les méthodes disponibles et pour quel type de fonctionnalité ou de donnée vous allez
les utiliser.

Les seules construction du langage que nous avons vu jusque là sont les commandes, les clés et les valeurs. Jusqu'ici
rien de concret à propos des structures de données. Quand nous utilisons la commande `set`, comment Redis peut-il savoir
quelle structure de données utiliser? Il s'avère que chaque commande est spécifique à une structure de données. Par
exemple, quand vous utilisez `set`, vous êtes en train de stocker une valeur dans une structure de données de type
string. Quand vous utilisez `hset`, vous êtes en train de stocker un hash. Compte tenu de la taille restreinte du
vocabulaire de Redis, cela devrait être gérable.

**[Le site web de Redis](http://redis.io/commands) a une très bonne documentation de référence. Il n'est pas ici
question de répéter le travail qu'ils ont déjà réalisé. Nous allons seulement couvrir les commandes les plus importantes
 nécessaires pour la compréhension des différentes structures de données.**

Il n'y a rien de plus important que de prendre du plaisir en essayant de tester les concepts. Vous pouvez toujours
supprimer toutes les données de votre base de données en entrant `flushdb`, alors ne soyez pas timide et essayer de
tenter des choses un peu folles!

## Les Strings

Les Strings sont les structures de données disponibles dans Redis les plus basiques., Quand nous pensez à une paire
clée-valeur, vous pensez à des strings. Ne soyez pas perplexe par rapport au nom, comme toujours, votre valeur peut
être n'importe quoi. Je préfère les appeler des 'scalaires', mais ce n'est peut-être que moi.

Nous avons déjà vu un cas classique d'usage des strings, pour le stockage d'instances d'objets via des clées. C'est
quelque chose que vous risquez d'utiliser très souvent :

	set users:leto "{name: leto, planet: dune, likes: [spice]}"

De manière aditionnelle, Redis vous permet de réaliser certaines opérations très classiques. Par exemple `strlen <key>`
peut être utilisée pour récupérer la longueur d'une valeur associée à une clée; `getrange <key> <start> <end>` retourne
la sous-chaîne correspondante de la valeur associée à la clée fournie; `append <key> <value>` ajoute la valeur à une
valeur existante (ou la créée si elle n'existait pas déjà). Allez-y et essayez ces opérations. Voici ce que j'obtiens :

	> strlen users:leto
	(integer) 42

	> getrange users:leto 27 40
	"likes: [spice]"

	> append users:leto " OVER 9000!!"
	(integer) 54

Maintenant, vous devriez vous dire : c'est super, mais ça n'a pas de sens. Vous ne pouvez pas récupérer une partie
d'une donnée JSON ou ajouter une valeur simplement. Vous avez raison, la leçon à tirer ici est que certaines commandes,
et plus particulièrement avec les structures de données de type strings, ne font sens pour que des types de données
spécifiques.

Plus haut nous avons appris que Redis ne tient aucun compte des valeurs. La plupart du temps c'est vrai. Cependant,
quelques commandes relatives aux strings sont spécifiques à certains types ou structures de données. Comme vague
exemple, je vois notamment l'usage des fonctions déjà citées comme `append` et `getrange` qui sont utiles dans quelques
cas de sérialisation efficace en terme d'espace mémoire. Comme exemples plus concret, je vous donne les commandes
`incr`, `incrby`, `decr` et `decrby`. Ces commandes incrémentent ou décrémentent la valeur d'une string:

	> incr stats:page:about
	(integer) 1
	> incr stats:page:about
	(integer) 2

	> incrby ratings:video:12333 5
	(integer) 5
	> incrby ratings:video:12333 3
	(integer) 8

Comme vous pouvez l'imaginer, les strings Redis sont parfaites pour les statistiques. Essayez d'incrémenter `users:leto`
 (une valeur non-entière) et voyez ce qui arrive (vous devriez avoir une erreur).

Un exemple plus avancé est celui des commandes `setbit` et `getbit`. Il y a un [superbe article](http://blog.getspool
.com/2011/11/29/fast-easy-realtime-metrics-using-redis-bitmaps/) sur la façon dont Spool utilise ces deux commandes
pour répondre de manière efficace à la question "combien de visiteurs uniques avons nous eu aujourd'hui?". Pour 128
millions d'utilisateurs, un ordinateur portable génère la réponse en moins de 50 ms et n'utilise seulement que 16MB
de mémoire.

Il n'est pas important que vous compreniez la manière dont fonctionnent les bitmaps,
ou la façon dont Spool les utilise, mais il est plutôt important que vous compreniez que les strings de Redis sont
plus puissantes que ce qu'elles semblaient initialement. Toujours est-il que les cas d'usage les plus fréquents sont
ceux qui ont été donnés plus haut : stockage d'objets (complexes ou non) et compteurs. Enfin,
comme il est très rapide de récupérer des valeurs grâce à leurs clés, les strings sont souvent utilisées pour cacher
des données.

## Les Hashs
Les Hashs sont un bon exemple montrant qu'appeler Redis une base de données clée-valeur n'est pas correct. Vous voyez,
pour plusieurs raisons, les Hashs sont comme les Strings. La différence importante est qu'ils fournissent un niveau
supplémentaire d'indirection: un champ. Ainsi, les équivalents de `set` et `get` sont :

	hset users:goku powerlevel 9000
	hget users:goku powerlevel

Nous pouvons aussi associer différents champs à la fois, récupérer plusieurs champs à la fois, récupérer tous les
champs et leurs valeurs, lister tous les champs, ou supprimer un champ spécifique:

	hmset users:goku race saiyan age 737
	hmget users:goku race powerlevel
	hgetall users:goku
	hkeys users:goku
	hdel users:goku age

Comme vous pouvez le voir, les Hashs nous donnent plus de contrôle sur les valeurs que les Strings. Plutôt que de
stocker un utilisateur comme une simple valeur sérialisée, nous pouvons utiliser un Hash afin d'en donner une
représentation plus précise. Le bénéfice serait alors la capacité de récupérér et mettre à jour/supprimer des
éléments spécifiques de la donnée, sans avoir à écrire et lire la valeur entière.

Voir les Hashs avec comme perspective celle d'un objet défini complètement, tel qu'un utilisateur,
est la clée de la compréhension de leur fonctionnement. Et il est vrai, que pour des raisons de perfornace,
un contrôle plus fin peut être utile. Cependant, dans le prochain chapitre, nous allons voire de quelle manière les
Hashs peuvent être utiliser pour organiser vos données et faire des requêtes plus simplement. De mon point de vue,
c'est là que les Hashs deviennent vraiment intéressants.

## Les Listes

Les Listes vous permettent de stocker et manipuler des tableaux de valeurs pour une certaine clée. Vous pouvez
ajouter des valeurs à la liste, récupérer la première ou la dernière valeur et manipuler les valeurs en utilisant les
 index. Les Listes maintiennent un ordre et possèdent des opérations sur les index efficaces. Nous pouvons avoir une
 Liste `newusers` qui traque les utilisateurs nouvellement enregistrés sur notre site :

	lpush newusers goku
	ltrim newusers 0 50

Tout d'abord, poussons un nouvel utilisateur sur le haut de la Liste, et ensuite ajuston la liste afin qu'elle ne
contienne que les 50 derniers utilisateurs. Ceci est un pattern classique. `ltrim` est une opération en O(N),
où N est le nombre de valeurs à supprimer. Dans ce cas, où nous ajustons la Liste après chaque insertion,
l'opération aura une opération constante en O(1) (car N sera toujours égal à 1).

Ceci est aussi la première fois que nous voyons une valeur correspondant à une clée qui référence une valeur dans une
 autre. Si nous souhaitons avoir le détail des 10 derniers utilisateurs, nous devons réaliser l'opération suivante :

	keys = redis.lrange('newusers', 0, 10)
	redis.mget(*keys.map {|u| "users:#{u}"})

Le code ci-dessus est écrit en Ruby et il montre le type de multiples aller-retour à réaliser dont nous avons parlé
auparavant.

Bien sûr, les Listes ne sont pas uniquement bonnes pour le stockage de références vers d'autres clées. Les valeurs
peuvent être n'importe quoi. Vous pouvez utiliser les Listes afin de stocker les logs ou traquer les chemins qu'un
utilisateur va suivre sur un site. Si vous êtes en train de construire un jeu, vous pourriez utiliser les Listes pour
 suivre les actions mises en queue par un utilisateur.

## Les Sets

Les Sets sont utilisés pour stocker des valeurs uniques et fournissent un ensemble d'opération en lien avec les
ensembles, comme les unions. Les Sets en sont pas ordonnés, mais ils fournissent un ensemble d'opération efficaces
sur les valeurs. Une liste d'amis est l'exemple classique de l'usage d'un Set :

	sadd friends:leto ghanima paul chani jessica
	sadd friends:duncan paul jessica alia

En fonction du nombre d'amis que peut avoir l'utilisateur, nous pouvons dire en O(1) si un utilisateur userX est un
ami ou non de l'utilisateur userY :

	sismember friends:leto jessica
	sismember friends:leto vladimir

De plus, nous pouvons voir que deux ou plusieurs personnes partagent les mêmes amis :

	sinter friends:leto friends:duncan

et même stocker le résultation comme une nouvelle clée :

	sinterstore friends:leto_duncan friends:leto friends:duncan

Les ensembles sont parfaits pour tagguer et suivre n'importe quelle propriété d'une valeur pour laquelle des doubles
n'ont pas de sens (ou lorsque nous voulons appliquer des opérations sur les ensembles comme des intersections ou des
unions).

## Les Sorted Sets (Ensembles Triés)

La dernière et plus puissante structure de données et le Sorted Set (ou ensemble trié). Si les Hashs sont comme les
Strings mais avec des champs, les Sorted Sets sont comme les Sets, mais avec un score. Les scores fournissent des
capacités de tri et de classement. Si nous souhaitons une liste classée d'amis,
nous ferons cela de la manière suivante :

	zadd friends:duncan 70 ghanima 95 paul 95 chani 75 jessica 1 vladimir

Vous souhaitez trouver combien d'amis à `duncan` avec un score supérieur ou égal à 90 ?

	zcount friends:duncan 90 100

Ou alors savoir quel est le classement de `chani` ?

	zrevrank friends:duncan chani

Nous utilisons `zrevrank` au lieu de `zrank` dans la mesure où le tri par défaut de Redis est ascendant (mais dans ce
 cas nous souhaitons trier de manière descendante). L'usage le plus évident pour un Sorted Set est un système de
 classement. En réalité cependant, tout ce que vous souhaiteriez trier par une valeur entière, et être capable de
 manipuler efficacement à travers l'utilisation d'un score, pourrait être bien mis en place à travers un ensemble trié.

## Dans ce chapitre

Nous avons pu avoir un aperçu des cinq structures de données de Redis. Une des choses les plus intéressant à propos
de Redis est le fait que vous pouvez souvent faire plus que ce que vous n'auriez imaginé à priori. Tant que vous
comprendrez les cas d'utilisation nominaux , vous pourrez voir que Redis est idéal pour répondre à tout type de
problème., De plus, ce n'est pas parce que Redis expose cinq structures de données et des méthodes variées,
qu'il est nécessaire de toutes les utiliser. Il est commun de mettre en place une fonctionnalité en n'utilisant qu'un
 petit sous-ensemble de commandes.

# Chapitre 3 - Tirer partie des structures de données

Dans le chapitre précédent, nous avons parlé des cinq structures de données de Redis,
et nous avons donné quelques exemples des problèmes qu'elles peuvent aider à résoudre. Il est maintenant temps de
se pencher sur quelques autres, cependant communs, sujets et design patterns.

## La notation en grand O

Tout au long de ce livre, nous avons fait référence à la notation en grand O sous la forme de O(n) ou O(1). La
notation en grand O est utilisée pour expliquer comment les choses évoluent en fonction d'un certain nombre
d'éléments. Dans Redis, cette notation est utilisée pour dire à quelle vitesse une commande opère,
en fonction du nombre d'éléments avec lequel nous avons à travailler.

La documentation de Redis nous donne ainsi la notation grand O pour chacune de ses commandes. Cette documentation
nous donne aussi les facteurs qui vont influencer ces performances. Regardons quelques exemples.

Le plus rapide que l'on puisse faire est O(1) qui est une constante. Que nous ayons à traiter 5 éléments ou 5
millions d'éléments, vous avez la même performance. La commande `sismember`, qui nous dit si une donnée appartientà
un ensemble, est en O(1). `sismember` est une commande puissante, et ses caractéristiques de performance en sont une
bonne raison. Un certain nombre de commandes de Redis sont en O(1).

Une performance logarithmique, ou O(log(N)), est la seconde plus rapidem performance possible car cela signifie qu'il
 est nécessaire de scanner des partitions de moins en moins grande. Utiliser ces approches de types `divide and
 conquer`, un grand nombre d'éléments est rapidement traiter en quelques itérations. `zadd` est une commande en O(log
 (N)), où N est le nombre d'éléments déjà dans l'ensemble.

Ensuite nous avons les commandes avec des performances linéaires, ou en O(N). Rechercher une colonne non indéxée dans
 une table est une opération en O(N). C'est par exemple le cas de la commande `ltrim`. Cependant,
 dans le cas de `ltrim`, N n'est pas nombre d'éléments dans la liste, mais plutôt celui des éléments supprimés.
 Ainsi, utiliser `ltrim` pour enlever un éléments d'une liste de millions d'éléments peut-être plus rapide
 qu'utiliser 10 éléments d'une liste en contenant des milliers. (Les deux commandes risque d'être tellement rapides
 que vous ne verez pas la différence quand vous les chronométrerez).

`zremrangebyscore` qui supprime des éléments d'un Sorted Set avec un score entre un minimum et un maximum possède une
 complexité en O(log(N)+M). C'est un mix. En lisant la documentation nous pouvons voir que N est le nombre total
 d'éléments dans l'ensemble et M et le nombre d'éléments à supprimer. En d'autres mots,
 le nombre d'éléments qui vont être supprimées va problablement être plus significatif, en terme de performance,
 que le nombre total d'éléments dans l'ensemble.

La commande `sort`, dont nous allons parler plus en détail dans le prochain chapitre,
possède une complexité en O(N+M*log(M)). D'un point de vue de ses caractéristiques de performance,
vous pouvez probablement dire que c'est l'une des commandes de Redis les plus complexes.

Il y a un certain nombre d'autres complexités, les deux restantes étant O(N^2) et O(C^N). Plus N est grand,
plus les performances seront dégradées par rapport à des N plus petits. Aucune des méthodes de Redis n'ont ce type de
 complexité.

Il est important de pointer le fait que la notation en grand O traite du pire cas possible. Quand nous disons que
quelque chose possède une complexité en O(N), nous pouvons trouver la donnée tout de suite ou il pourrait s'agir du
dernier éléments possible.

## Pseudo-Requête avec des clées multiples

Une situation classique à laquelle vous allez être confrontés est de vouloir faire des requêtes la valeur avec
différentes clées. Par exemple, vous pouvez vouloir récupérer un utilisateur par son e-mail (pour la première fois où
 ils se connectent) et aussi par id (après qu'ils se soient connectés). Une solution horrible est de dupliquer votre
 objet utilisateur avec deux chaînes :

	set users:leto@dune.gov "{id: 9001, email: 'leto@dune.gov', ...}"
	set users:9001 "{id: 9001, email: 'leto@dune.gov', ...}"

Ceci est mauvais car c'est un cauchemar à gérer et ce que cela prend deux fois plus de mémoire.

Il serait bien si Redis laissez faire des liens entre différentes clées, mais ce n'est pas le cas (et il ne le fera
probablement jamais). Un driver important dans le développement de Redis est de garder le code et une API propre et
simple. L'implémentation interne de clées liées (il y a un grand nombre de choses à faire avec les clées que nous
n'avons pas encore abordé pour l'instant) ne vaut le coup quand vous considérer le fait que Redis fournit déjà une
solution : les Hashs.

Utiliser un Hash, permet de supprimer le besoin de duplication:

	set users:9001 "{id: 9001, email: leto@dune.gov, ...}"
	hset users:lookup:email leto@dune.gov 9001

Ce que nous faisons ici est d'utiliser le champs comme un pseudo index secondaire et de référencer un seul objet
utilisateur. Pour récupérer un utilisateur par id, nous réalisons un `get` classique :

	get users:9001

Pour récupérer un utilisateur par e-mail, nous réalisons un `hget` suivi par un `get` (en Ruby) :

	id = redis.hget('users:lookup:email', 'leto@dune.gov')
	user = redis.get("users:#{id}")

C'est quelque chose que nous allons être amenés à faire assez souvent. Pour moi, c'est dans ce type de situation que
les Hashs ont vraiment un intérêt, mais ce n'est pas un cas d'utilisation si évident tant que nous ne l'avons pas
rencontré.

## Index et Références

Nous avons vu un ensemble d'exemples où nous avions une valeur en référencer une autre. Nous avons vu cela avec notre
 exemple de liste, et nous avons vu cela dans la section précédente quand nous avons utilisée des Hashs pour faire
 des requêtes de manière plus simple. Tout ceci revient finalement à gérer manuellement vos index et les références
 entre valeurs. Il faut être honnête, je pense que nous pouvons dire que c'est un point négatif,
 tout particulièrement quand vous considérez avoir à mettre à jour/gérer/supprimer ces références manuellement. Il
 n'y a pas de soluiton magique pour résoudre ce problème dans Redis.

Nous avons déjà vu comment les ensembles sont souvent utilisés pour implémenter manuellement des index :

	sadd friends:leto ghanima paul chani jessica

Chaque membre de cet ensemble est une référence à une valeur contenue par une String Redis contenant les détails pour
 chaque utilisateur. Et que se passe-t-il si `chani` change son nom ou supprime son compte? Il fera alors
 peut-être sens de garder une trace des relations inverses :

	sadd friends_of:chani leto paul

Mis à part les coûts de maintenance, si vous êtes un peu comme moi, vous pourriez peut-être reculer devant le surcoût
 en terme de mémoire et de calcul impliqué par ces valeurs indexées supplémentaires. Dans la prochaine section,
 nous allons parler des manières de réduire les coûts de performance impliqués par des opération supplémentaires
 (nous en avons rapidement parlé dans le premier chapitre).

Si cependant vous y pensez quand même, les bases de données relationnelles ont les mêmes surcoûts. Les index occupent
 de la mémoire, doivent être scannés, ou idéalement recherchez et finalement les enregistrements correspondant
 peuvent être récupérés. Le surcoût est ainsi soigneusement abstrait (et ils réalisent un grand nombre
 d'optimisations en terme de calcul afin de rendre tout ça aussi efficace que possible).

Encore, avoir à gérer manuellement des références avec Redis est un inconvénient. Mais il est nécessaire de réaliser
des tests pour avoir des informations sur les implications en terme de performance et de calcul de ces opérations. Je
 pense que vous trouverez que ce n'est pas un vrai problème.

## Allers-retours et pipelining

Nous avons déjà mentionné le fait que faire des appels fréquents au serveur est un pattern classique avec Redis. Dans
 la mesure ou vous allez le faire souvent, il est important de prêter un oeil attentif aux fonctionnalités à utiliser
  pour l'utiliser au mieux.

Tout d'abord, un certain nombre de commande acceptent un ou plusieurs arguments ou ont des commandes proches qui
prennet plusieurs paramètres en entrée Nous avons vu `mget` plus tôt, qui prend plusieurs clées et retourne les
valeurs correspondantes :.

	keys = redis.lrange('newusers', 0, 10)
	redis.mget(*keys.map {|u| "users:#{u}"})

Ou la commande `sadd` qui ajoute un ou plusieurs membres à un ensemble :

	sadd friends:vladimir piter
	sadd friends:paul jessica leto "leto II" chani

Redis supporte aussi le pipelining. Normalement, quand un client envoie une requête à Redis,
il attend la réponse avant d'envoyer une autre requête. Avec le pipelining, vous pouvez envoyer plusieurs requêtes
sans avoir à attendre les réponses correspondantes. Ceci réduit le surcoût de communication et peut permettre des
gains significatifs de performance.

Il est important de noter que Redis va utiliser la mémoire pour mettre en queue les commandes,
c'est donc une bonne manière de les exécuter en batch. La taille du batch que vous allez utiliser va être fonction du
 nombre de commande à exécuter, et plus spécifiquement, du nombre de paramètres. Mais,
 si vous souhaiter exécuter des commandes avec des clées de 50 caractères chacune,
 vous pouvez probablement les rasembler sous forme de batch par milliers voire même dizaines de milliers.

La façon dont vous exécuterez des commandes dans un pipeline va dépendre du driver Redis que vous allez utiliser. En
Ruby, vous passez un bloc à la méthode `pipelined` :

	redis.pipelined do
	  9001.times do
		redis.incr('powerlevel')
	  end
	end

Comme vous pouvez l'imaginer, le pipelining peut vraiment accélérer l'exécution de commande au sein d'un batch!

## Transactions

Chaque commande Redis est atomique, et ceci inclus d'ailleurs celles qui réalisent plusieurs opérations à la fois. En
 plus, Redis support les transactions quand de multiples commandes sont réalisées.

Vous ne le savez peut-être pas, mais Redis est en fait mono-threadé, ce qui est la raison pour laquelle chaque
commande est garantie atomique. Pendant qu'une opération est exécutée, aucune autre commande ne peut être lancée.
(Nous parlerons brièvement des aspects scalabilité dans un prochain chapitre). Ceci est particulièrement utile quand
on sait que certaines commandes réalisent plusieurs opérations à la fois. Par exemple :

`incr` est essentiellement un `get` suivi d'un `set`

`getset` fixe une nouvelle valeur et retourne l'original

`setnx` vérifie d'abord que la clée existe, et ne la fixe que si la valeur n'existait pas

Même si ces commandes sont utiles, vous allez inévitablement devoir effectuer plusieurs commandes à la fois en tant
que groupe atomique. Vous pouvez réaliser cette action en appelant tout d'abord la commande `multi`,
suivie par toutes les commandes que vous souhaitez réaliser au sein de votre transaction,
et enfin il est nécessaire d'exécuter `exec` pour effectivement exécuter les commandes ou alors `discard` pour tout
arrêter et ne pas exécuter les commandes.
Quelles sont les garanties fournies par Redis quand des transactions sont exécutées?

* Les commandes seront exécutées dans l'ordre

* Les commandes seront exécutées comme une seule opération atomique (sans qu'aucun autre commande d'un client
ne soit exécutées pendant la transaction)

* Toutes les opérations de la transaction ou aucun ne sera exécutée

Vous pouvez, et devriez, tester ceci dans l'interface en ligne de commande. Notez aussi qu'il n'y a aucune raison
pour ne pas combiner pipelining et transactions.

	multi
	hincrby groups:1percent balance -9000000000
	hincrby groups:99percent balance 9000000000
	exec

Enfin, Redis vous permet de définir une clée (ou plusieurs) à observer et le cas échéant,
réaliser une transaction sur cette clée (ou plusieurs) ont changé. Ceci est utilisé quand vous avez besoin de
récupérer des valeurs et exécuter du code basé sur ces valeurs, le temps à travers une transaction. Avec le code
ci-dessus, nous ne seront pas capable d'implémenter notre propre commande `incr` dans la mesure où elle sont
exécutées ensemble une fois que `exec` iest appelée. Depuis du code, nous ne pouvons pas faire :

	redis.multi()
	current = redis.get('powerlevel')
	redis.set('powerlevel', current + 1)
	redis.exec()

Ce n'est pas la façon dont fonctionne Redis. Cependant, si nous ajoutons un appel à `watch` concernant `powerlevel`,
nous pouvons faire la chose suivante :

	redis.watch('powerlevel')
	current = redis.get('powerlevel')
	redis.multi()
	redis.set('powerlevel', current + 1)
	redis.exec()

Si un autre client change la valeur de `powerlevel` après que nous aillons appelé `watch` sur elle,
notre transaction va échouer. Si aucun client ne change cette valeur, le `set` va fonctionner. Nous pouvons exécuter
ce code dans une boucle jusqu'à ce qu'il fonctionne.

## Anti-Pattern de clée

Dans le prochain chapitre, nous allons parler des commandes qui ne sont pas spécifiquement reliées aux structures de
données. Certains d'entre elles permettent l'administration de Redis ou sont des outils de débuggage. Mais il y en a
une dont je souhaiterais tout particulièrement parler : la commande `keys`. Cette commande prend un pattern et
retourne les clées correspondantes. Cette commande semble bien adaptée pour un grand nombre de tâches,
mais elle ne doit jamais être utilisée en prodution. Pourquoi? Parce qu'elle réalise une recherche linéaire sur
toutes les clées pour trouver les bonnes. Ou dit simplement, elle est lente.

De quelle manière est ce qu'elle est testée et utilisée? Disons que vous soyez en train de développer un système de
bug tracker. Chaque compte aura un `id` et vous pourriez décider de stocker chaque bug dans une String avec une
clée qui pourrait être `bug:account_id:bug_id`. Si vous souhaitez trouver tout les bugs associés à un compte (afin
de les afficher ou pour supprimer le compte correspondant), vous pourriez être tentés (comme je l'ai été)
d'utiliser la commande `keys` :

	keys bug:1233:*

La meilleure solution est d'utiliser un Hash. De la même manière que nous pouvons utiliser les Hashs pour fournir un
moyen d'exposer des index secondaires, nous pouvons aussi les utiliser pour organiser nos données :

	hset bugs:1233 1 "{id:1, account: 1233, subject: '...'}"
	hset bugs:1233 2 "{id:2, account: 1233, subject: '...'}"

Pour récupérer les ids de tout les bugs pour un compte, nous avons simplement à exécuter la commande `hkeys
bugs:1233`. Pour supprimer un bug spécifique, nous pouvons effectuer l'appel suivant `hdel bugs:1233 2` et pour
supprimer un bug, nous pouvons supprimer la clée via l'appel `del bugs:1233`.

## Dans ce chapitre

Ce chapitre, combiné avec le précédent, vous a donné un certain nombre d'idées sur les façons d'utiliser Redis pour
réaliser des choses de manière très puissante. Il y a un certain nombre de patterns que vous utiliser utiliser pour
construire toute sorte de choses, mais la vraie clée est la compréhension des structures de données fondamentales et
d'obtenir une idée sur la manière dont elles peuvent être utilisées pour réaliser des choses que nous n'auriez pas
imaginé dans un premier temps.

# Chapter 4 - Beyond The Data Structures

Tandis que les cinq structures de données de Redis en forme les fondations, il y a un certain nombre d'autres
commandes qui ne sont pas liées spécifiquement à une structure de données spécifique. Nous en avons déjà vu un nombre
 significatif : `info`, `select`, `flushdb`, `multi`, `exec`, `discard`, `watch` et `keys`. Ce chapitre va se pencher
  sur d'autres tout aussi importantes.

## Expiration

Redis permet de marquer des clées comme devant expirer à une certaine date. Vous pouvez leur donner une date absolue
sous la forme d'un timestamp Unix (le nombre de secondes depuis le premier janvier 1970) ou un temps à vivre en
secondes. C'est une commande intervenant sur les clées, et c'est pour cela que la structure de données sous-jacente
n'a pas d'importance.

	expire pages:about 30
	expireat pages:about 1356933600

La première commande va supprimer la clée (et donc la valeur associée) après 30 secondes. La seconde va faire la même
 chose à 12:00 le 31 décembre 2012.

Ceci fait de Redis le moteur de cache idéal. Vous pouvez savoir le temps restant à vivre (ou Time To Live en anglais)
 grâce à la commande `ttl` et vous pouvez lever l'expiration sur une clée grâce à la commande `persist` :

	ttl pages:about
	persist pages:about

Enfin, il y a une commande spéciale pour les String `setex` qui vous permet de fixer une String,
et de spécifier un temps restant à vivre en une seule commande atomique (ceci est plutôt fait pour une histoire de
simplicité qu'autre chose) :

	setex pages:about 30 '<h1>about us</h1>....'

## Publication and Souscriptions

Les Listes Redis ont des commandes `blpop` et `brpop` qui retournent et suppriment le premier (ou dernier)
éléments de la Liste ou bloque jusqu'à qu'il y en ai un de disponible. Ces commandes peuvent être utilisées pour
mettre en place une simple queue.

Au-delà de cela, Redis possède un support de grande qualité pour la publication de messages et la souscription à des
channels. Vous pouvez essayer cela vous-même en ouvrant une seconde fenêtre sur l'interface en ligne de commande.
Dans la première fenêtre souscrivez à un channel (nous l'appelerons `warnings`) :

	subscribe warnings

La réponse est l'information prouvant la souscription. Maintenant, dans l'autre fenêtre,
publiez un message ver le channel `warnings` :

	publish warnings "it's over 9000!"

Si vous retournez à votre première fenêtre, vous devriez avoir reçu le message envoyé au channel `warnings`.

Vous pouvez souscrire à de multiples channels (`subscribe channel1 channel2 ...`),
souscrire à un pattern de channel (`psubscribe warnings:*`) et enfin utiliser les commandes `unsubscribe` et
`punsubscribe` pour arrêter d'écouter à un ou plusieurs channel, ainsi qu'aux patterns de channel.

Enfin, il est important de noter que la commande `publish` retourne la valeur 1. Ceci indique le nombre de clients
qui ont reçu le message.

## Monitor and Slow Log

The `monitor` command lets you see what Redis is up to. It's a great debugging tool that gives you insight into how
your application is interacting with Redis. In one of your two redis-cli windows (if one is still subscribed, you can
either use the `unsubscribe` command or close the window down and re-open a new one) enter the `monitor` command. In
the other, execute any other type of command (like `get` or `set`). You should see those commands, along with their
parameters, in the first window.

You should be wary of running monitor in production, it really is a debugging and development tool. Aside from that,
there isn't much more to say about it. It's just a really useful tool.

Along with `monitor`, Redis has a `slowlog` which acts as a great profiling tool. It logs any command which takes
longer than a specified number of **micro**seconds. In the next section we'll briefly cover how to configure Redis, for
now you can configure Redis to log all commands by entering:

	config set slowlog-log-slower-than 0

Next, issue a few commands. Finally you can retrieve all of the logs, or the most recent logs via:

	slowlog get
	slowlog get 10

You can also get the number of items in the slow log by entering `slowlog len`

For each command you entered you should see four parameters:

* An auto-incrementing id

* A Unix timestamp for when the command happened

* The time, in microseconds, it took to run the command

* The command and its parameters

The slow log is maintained in memory, so running it in production, even with a low threshold, shouldn't be a problem.
By default it will track the last 1024 logs.

## Sort

One of Redis' most powerful commands is `sort`. It lets you sort the values within a list, set or sorted set (sorted
sets are ordered by score, not the members within the set). In its simplest form, it allows us to do:

	rpush users:leto:guesses 5 9 10 2 4 10 19 2
	sort users:leto:guesses

Which will return the values sorted from lowest to highest. Here's a more advanced example:

	sadd friends:ghanima leto paul chani jessica alia duncan
	sort friends:ghanima limit 0 3 desc alpha

The above command shows us how to page the sorted records (via `limit`), how to return the results in descending order
(via `desc`) and how to sort lexicographically instead of numerically (via `alpha`).

The real power of `sort` is its ability to sort based on a referenced object. Earlier we showed how lists, sets and
sorted sets are often used to reference other Redis objects. The `sort` command can dereference those relations and
sort by the underlying value. For example, say we have a bug tracker which lets users watch issues. We might use a set
to track the issues being watched:

	sadd watch:leto 12339 1382 338 9338

It might make perfect sense to sort these by id (which the default sort will do), but we'd also like to have these
sorted by severity. To do so, we tell Redis what pattern to sort by. First, let's add some more data so we can actually
see a meaningful result:

	set severity:12339 3
	set severity:1382 2
	set severity:338 5
	set severity:9338 4

To sort the bugs by severity, from highest to lowest, you'd do:

	sort watch:leto by severity:* desc

Redis will substitute the `*` in our pattern (identified via `by`) with the values in our list/set/sorted set. This
will create the key name that Redis will query for the actual values to sort by.

Although you can have millions of keys within Redis, I think the above can get a little messy. Thankfully `sort` can
also work on hashes and their fields. Instead of having a bunch of top-level keys you can leverage hashes:

	hset bug:12339 severity 3
	hset bug:12339 priority 1
	hset bug:12339 details "{id: 12339, ....}"

	hset bug:1382 severity 2
	hset bug:1382 priority 2
	hset bug:1382 details "{id: 1382, ....}"

	hset bug:338 severity 5
	hset bug:338 priority 3
	hset bug:338 details "{id: 338, ....}"

	hset bug:9338 severity 4
	hset bug:9338 priority 2
	hset bug:9338 details "{id: 9338, ....}"

Not only is everything better organized, and we can sort by `severity` or `priority`, but we can also tell `sort` what
field to retrieve:

	sort watch:leto by bug:*->priority get bug:*->details

The same value substitution occurs, but Redis also recognizes the `->` sequence and uses it to look into the specified
field of our hash. We've also included the `get` parameter, which also does the substitution and field lookup, to
retrieve bug details.

Over large sets, `sort` can be slow. The good news is that the output of a `sort` can be stored:

	sort watch:leto by bug:*->priority get bug:*->details store watch_by_priority:leto

Combining the `store` capabilities of `sort` with the expiration commands we've already seen makes for a nice combo.

## In This Chapter

This chapter focused on non-data structure-specific commands. Like everything else, their use is situational. It isn't
uncommon to build an app or feature that won't make use of expiration, publication/subscription and/or sorting. But
it's good to know that they are there. Also, we only touched on some of the commands. There are more, and once you've
digested the material in this book it's worth going through the [full list](http://redis.io/commands).

# Chapter 5 - Lua Scripting

Redis 2.6 includes a built-in Lua interpreter which developers can leverage to write more advanced queries to be executed within Redis. It wouldn't be wrong of you to think of this capability much like you might view stored procedures available in most relational databases.

The most difficult aspect of mastering this feature is learning Lua. Thankfully, Lua is similar to most general purpose languages, is well documented, has an active community and is useful to know beyond Redis scripting. This chapter won't cover Lua in any detail; but the few examples we look at should hopefully serve as a simple introduction.

## Why?

Before looking at how to use Lua scripting, you might be wondering why you'd want to use it. Many developers dislike traditional stored procedures, is this any different? The short answer is no. Improperly used, Redis' Lua scripting can result in harder to test code, business logic tightly coupled with data access or even duplicated logic.

Properly used however, it's a feature that can simplify code and improve performance. Both of these benefits are largely achieved by grouping multiple commands, along with some simple logic, into a custom-build cohesive function. Code is made simpler because each invocation of a Lua script is run without interruption and thus provides a clean way to create your own atomic commands (essentially eliminating the need to use the cumbersome `watch` command). It can improve performance by removing the need to return intermediary results - the final output can be calculated within the script.

The examples in the following sections will better illustrate these points.

## Eval

The `eval` command takes a Lua script (as a string), the keys we'll be operating against, and an optional set of arbitrary arguments. Let's look at a simple example (executed from Ruby, since running multi-line Redis commands from its command-line tool isn't fun):

    script = <<-eos
      local friend_names = redis.call('smembers', KEYS[1])
      local friends = {}
      for i = 1, #friend_names do
        local friend_key = 'user:' .. friend_names[i]
        local gender = redis.call('hget', friend_key, 'gender')
        if gender == ARGV[1] then
          table.insert(friends, redis.call('hget', friend_key, 'details'))
        end
      end
      return friends
    eos
    Redis.new.eval(script, ['friends:leto'], ['m'])

The above code gets the details for all of Leto's male friends. Notice that to call Redis commands within our script we use the `redis.call("command", ARG1, ARG2, ...)` method.

If you are new to Lua, you should go over each line carefully. It might be useful to know that `{}` creates an empty `table` (which can act as either an array or a dictionary), `#TABLE` gets the number of elements in the TABLE, and `..` is used to concatenate strings.

`eval` actually take 4 parameters. The second parameter should actually be the number of keys; however the Ruby driver automatically creates this for us. Why is this needed? Consider how the above looks like when executed from the CLI:
  
    eval "....." "friends:leto" "m"
    vs
    eval "....." 1 "friends:leto" "m"

In the first (incorrect) case, how does Redis know which of the parameters are keys and which are simply arbitrary arguments? In the second case, there is no ambiguity.

This brings up a second question: why must keys be explicitly listed? Every command in Redis knows, at execution time, which keys are going to needed. This will allow future tools, like Redis Cluster, to  distribute requests amongst multiple Redis servers. You might have spotted that our above example actually reads from keys dynamically (without having them passed to `eval`). An `hget` is issued on all of Leto's male friends. That's because the need to list keys ahead of time is more of a suggestion than a hard rule. The above code will run fine in a single-instance setup, or even with replication, but won't in the yet-released Redis Cluster.

## Script Management

Even though scripts executed via `eval` are cached by Redis, sending the body every time you want to execute something isn't ideal. Instead, you can register the script with Redis and execute it's key. To do this you use the `script load` command, which returns the SHA1 digest of the script:

    redis = Redis.new
    script_key = redis.script(:load, "THE_SCRIPT")

Once we've loaded the script, we can use `evalsha` to execute it:

    redis.evalsha(script_key, ['friends:leto'], ['m'])

`script kill`, `script flush` and `script exists` are the other commands that you can use to manage Lua scripts. They are used to kill a running script, removing all scripts from the internal cache and seeing if a script already exists within the cache.

## Libraries

Redis' Lua implementation ships with a handful of useful libraries. While `table.lib`, `string.lib` and `math.lib` are quite useful, for me, `cjson.lib` is worth singling out. First, if you find yourself having to pass multiple arguments to a script, it might be cleaner to pass it as JSON:

    redis.evalsha ".....", [KEY1], [JSON.fast_generate({gender: 'm', ghola: true})]

Which you could then deserialize within the Lua script as:

    local arguments = cjson.decode(ARGV[1])

Of course, the JSON library can also be used to parse values stored in Redis itself. Our above example could potentially be rewritten as such:

      local friend_names = redis.call('smembers', KEYS[1])
      local friends = {}
      for i = 1, #friend_names do
        local friend_raw = redis.call('get', 'user:' .. friend_names[i])
        local friend_parsed = cjson.decode(friend_raw)
        if friend_parsed.gender == ARGV[1] then
          table.insert(friends, friend_raw)
        end
      end
      return friends

Instead of getting the gender from specific hash field, we could get it from the stored friend data itself. (This is a much slower solution, and I personally prefer the original, but it does show what's possible).

## Atomic

Since Redis is single-threaded, you don't have to worry about your Lua script being interrupted by another Redis command. One of the most obvious benefits of this is that keys with a TTL won't expire half-way through execution. If a key is present at the start of the script, it'll be present at any point thereafter - unless you delete it.

## Administration

The next chapter will talk about Redis administration and configuration in more detail. For now, simply know that the `lua-time-limit` defines how long a Lua script is allowed to execute before being terminated. The default is generous 5 seconds. Consider lowering it.

## In This Chapter

This chapter introduced Redis' Lua scripting capabilities. Like anything, this feature can be abused. However, used prudently in order to implement your own custom and focused commands, it won't only simplify your code, but will likely improve performance. Lua scripting is like almost every other Redis feature/command: you make limited, if any, use of it at first only to find yourself using it more and more every day. 

# Chapitre 6 - Administration

Notre dernier chapitre est dédié à certains aspects de l'administration d'un serveur Redis. Ce n'est en aucun cas un manuel complet sur l'administration de Redis. Au mieux, nous répondrons à certaines de vos interrogations que la plupart des nouveaux utilisateurs de Redis se posent communément.

## Configuration

Quand vous démarrez pour la première fois, le service Redis, il vous avertit que le fichier `redis.conf` est indisponible. Ce fichier permet de configurer différents paramètres de Redis. Un fichier `redis.conf` bien documenté est disponible avec chaque version de Redis. Ce fichier d'exemple contient les options de configuration par défaut, il est donc utile de comprendre à la fois ce que ces paramètres font et quelles sont les valeurs par défaut. Vous pourrez le trouver à l'url suivante: <https://github.com/antirez/redis/raw/2.4.6/redis.conf>.

**Ceci est le fichier de configuration de Redis 2.4.6. Vous devrez remplacer le "2.4.6" dans l'url précédente avec le numéro de version correspondant. Vous pourrez retrouver le numéro de version en récupérant la première valeur retournée par la commande `info`.**

Puisque le fichier est bien documenté, nous ne verrons pas le détails des paramètres.

En plus de pouvoir configurer Redis à l'aide du fichier `redis.conf`, la commande  `config set` peut être utilisé pour définir des valeurs individuelles. Nous nous sommes déjà servi de cette possibilité en définissant le paramètre `slowlog-log-slower-than` à 0.

Il y a également la commande  `config get` qui affiche la valeur d'un paramètre. La commande supporte la correspondance par motif. Donc si nous souhaitons configurer l'ensemble des paramètres de logs, nous vouvons faire:

	config get *log*

## Authentification

Redis peut être configurer pour requérir un mot de passe. Cela est possible en configurant `requirepass` (soit à l'aide du fichier `redis.conf` ou de la commande `config set`. Quant `requirepass` est défini (avec le mot de passe requis), les clients doivent s'authentifier à l'aide de la commande `auth password`.

Dès qu'un client est authentifié, il peut utiliser n'importe quelle commande sur toutes les bases. Cela inclut la commande `flushall` qui supprime toutes les clés de toutes les bases. À travers la configuration, vous pouvez renommer les commandes pour réaliser une sécurité par obscurité:

	rename-command CONFIG 5ec4db169f9d4dddacbfb0c26ea7e5ef
	rename-command FLUSHALL 1041285018a942a4922cbf76623b741e

Ou bien désactiver la commande en redéfinissant son nom en chaîne vide.

## Limitations de taille

Dès que vous utilisez Redis, vous vous demander peut-être "Combien de clés puis-je avoir ?" Mais également, combien de champs peut avoir les tables de hachages (notamment quand vous les utiliser pour structurer vos données), ou bien combien d'élèments peuvent contenir les listes et les ensembles ? Les limites pratiques par instances sont pour chacunes d'environ plusieurs centaines de millions.

## Réplication

Redis offre le support de la réplication, ce qui signifie que dès que vous écrivez sur une instance Redis (maitre), une ou plusieurs instances (esclaves) sont automatiquement mises à jour par le maitre. Pour configurer un esclave, vous pouvez utiliser soit la clé de configuration `slaveof` ou la commande éponyme (les instances en cours sans cette clé configuré sont soient maitres ou peuvent l'être).

La réplication permet de protéger vos données en les copiant sur différents serveurs. La réplication peut servir à améliorer les performances puisque l'on peut répartir les lectures entre les esclaves. Ils peuvent répondre avec des données légèrement en retard, mais pour la plupart des applications, c'est un compromis acceptable.

Malheureusement, la réplication Redis n'offre pas encore de basculement automatique. Si le maitre tombe en panne, un esclave doit être promu manuellement. Les outils de haute disponibilité classique qui se base sur le monitoring et les scripts permettant le basculement sont un mal nécessaire si vous souhaitez obtenir de la haute disponibilité avec Redis.

## Sauvegarde

Pour sauvegarder Redis, il suffit de copier un cliché de la base sur un support quelconque (S3, FTP, etc.). Par défaut, Redis sauvegarde son cliché dans un fichier nommé `dump.rdb`. VOus pouvez copier ce fichier à n'importe quel moment à l'aide de `scp`, `ftp` ou `cp` (ou autre).

Il n'est pas inhabituel de désactiver à la fois les clichés et le fichier en mode ajout sur le maitre et laisser un esclave gérer ceci. Cela permet de réduire la charge sur le maitre et vous permet de configurer des paramètres de sauvegarde plus aggressifs sur l'esclave sans nuire aux performances du système.

## Passage à l'échelle et Redis Cluster

La réplication est le premier outil qu'un site en croissance a à sa disposition. Certaines commandes sont plus coûteuses que d'autres (par ex. `sort`) et répartir leurs exécution sur un esclave peut aider à un système à conserver un temps de réponse correct pour traiter les requêtes entrantes.

Au-delà, passer à l'échelle Redis revient à distribuer vos clés sur plusieurs instances Redis (qui peuvent tourner sur la même machine, souvenez-vous, Redis est mono-thread). Pour le meoment, c'est une problématique que vous devrait gérer (bien qu'un certain nombre de pilotes Redis fournissent des algorithmes de hachage cohérent). Penser vos données en termes de distribution horizontale, n'entre pas dans le scope de cet ouvrage. C'est également quelque chose dont vous n'aurez pas à vous soucier avant longtemps, mais vous devriez en prendre connaissance quelque soit la solution retenue.

La bonne nouvelle est que cette problématique est sensé être résolu par le prochain Redis Cluster. Non seulement, il offrira le passage à l'échelle horizontal (incluant la redistribution des données), mais ils fournira le basculement automatique pour la haute-disponibilité.

La haute-disponibilité et le passage à l'échelle est quelque chose qui peut être réalisé très rapidement, du moment que vous êtes d'accord pour y cnsacrer du temps et quelques efforts. Plus tard, Redis Cluster devrait vous faciliter la vie.

## Dans ce chapitre

Étant donné le nombre de projets et sites utilisant déjà Redis, il n'y a aucun doute sur le fait que Redis soit mature, et cela depuis un bon moment. Néanmoins, une partie de l'outillage notamment ceux associés à la sécurité et la haute-disponibilité sont encore jeunes. Nous espérons que la sortie prochaine de Redis Cluster devrait résoudre certaines de ces problématiques.


# Conclusion

De bien des manières, Redis aide à simplifier la façon dont on traite les données. Il élimine une bonne part de complexité et d'abstraction disponibles dans d'autres systèmes. Dans certains cas, cela fait de Redis un mauvais choix. Dans d'autres, il donne l'impression que Redis est taillé sur mesure pour gérer nos données.

Au final, cela revient à ce que je disais au début: Redis est simple à apprendre. Il y a de nombreuses autres technologies, et il est difficile de se décider sur laquelle investir du temps pour les apprendre. Quand vous mettez en relation les avantages offerts par Redis et sa simplicité, je crois que c'est l'un des meilleurs investissements que vous puissiez vous et votre équipe réaliser.


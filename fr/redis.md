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

# Chapitre 4 - Au-delà des structures de données

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

## Commandes monitor et slowlog

La commande `monitor` vous laisse observer de quoi est capable Redis. Il s'agit d'une outil de débuggage fantastique qui vous permet d'avoir une vision sur la façon dont votre application interagit avec Redis.

Dans l'une de vos deux fenêtres avec la cli de Redis (si l'une d'entre elles est toujours inscrite, vous pouvez soit utiliser la commande `unsubscribe` soit fermer la fenêtre et en réouvrir une nouvelle) entrez la commande `monitor`. Dans l'autre fenêtre, exécutez n'importe quel autre type de commande (comme `get` ou `set`). Vous devriez voir ces commandes, ainsi que leurs paramètres, dans la première fenêtre.

Vous devriez être prudent en exécutant la commande `monitor` en production, il s'agit vraiment d'un outil de debugging et de développement. En dehors de ça, il n'y a pas grand choses à dire à son propos. Il s'agit d'un outil réellement utile.

À côté de la commande `monitor`, Redis dispose d'une commande nommée `slowlog` qui peut agir comme un excellent outil de profiling. Il loggue toute commande durant plus d'un certain nombre de **micro**secondes. Dans la prochaine section, nous allons brièvement couvrir la manière de configurer Redis, mais pour l'instant nous allons configurer Redis pour que toutes les commandes soient logguées en entrant :

	config set slowlog-log-slower-than 0

Ensuite, entrez quelques commandes. Vous pouvez ensuite récupérer toutes les logs ou seulement les plus récentes de la manière suivante :

	slowlog get
	slowlog get 10

Vous pouvez aussi récupérer le nombre d'éléments dans les *slow log* en entrant `slowlog len`

Pour chaque commande que vous avez entré, vous devriez voir quatre paramètres :

 * Un id auto-incrémenté
 * Un timestamp Unix correspondant au moment où elle a été exécutée
 * La durée, en microsecondes, que la commande a pris pour s'exécuter
 * La commande et ses paramètres

Les *slow log* sont maintenues en mémoire, ainsi, exécuter cette commande en production, même avec un seuil bas, ne devrait pas poser de problèmes. Par défaut, la commande suivra les 1024 derniers logs.

## Commande `sort`

La commande `sort`est l'une des plus puissantes de Redis. Elle vous permet de trier des valeurs au sein d'une liste, d'un ensemble ou d'un ensemble trié (ces ensembles sont triés par score et non en fonction des membres au sein de cet ensemble). Dans sa forme la plus simple, elle nous permet de réaliser l'opération suivante :

	rpush users:leto:guesses 5 9 10 2 4 10 19 2
	sort users:leto:guesses

qui retournera les valeurs triées de manière ascendante. Voici un exemple plus avancé :

	sadd friends:ghanima leto paul chani jessica alia duncan
	sort friends:ghanima limit 0 3 desc alpha

La commande ci-dessus nous montre comment paginer les entrées triées (via `limit`), comment retourner les résultats en ordre descendant
(via `desc`) et comment les trier de manière lexicographique et non numérique (via `alpha`).

Le vrai pouvoir de la commande `sort` réside dans sa capacité à trier en se basant sur un objet de référence. Nous avons pu montrer un peu plus tôt comment les listes, les ensemblent, ainsi que les ensembles triés sont souvent utilisés pour référencer d'autres objets Redis. La commande `sort` peut déréferencer ces relations et trier en fonction des valeurs sous-jacentes. Disons, par exemple, que nous avons un bug tracker qui permet aux utilisateurs de suivre les problèmes. Nous pourrions utiliser un ensemble pour traquer les problèmes suivi actuellement :

	sadd watch:leto 12339 1382 338 9338

Cela pourrait complètement faire sens de trier ces problèmes en fonction de leurs ids (ce que le tri par défaut réalise), mais nous pourrions aussi vouloir les trier par sévérité. Pour réaliser cette tâche, nous pouvons dire à Redis quel pattern doit être utilisé pour le tri. Tout d'abord, ajoutons des valeurs supplémentaires afin d'obtenir des résultats qui aient du sens :

	set severity:12339 3
	set severity:1382 2
	set severity:338 5
	set severity:9338 4

Pour trier les bugs par sévérité, de la plus haute à la plus basse, vous devez donc faire :

	sort watch:leto by severity:* desc

Redis substituera l'étoile `*` dans notre pattern (identifié via `by`) par la valeur dans notre liste/ensemble/ensemble trié. Cela créera la clée qui sera utilisée par Redis pour réalier sa requête avec les réelles valeurs dans son tri.

Bien que vous puissiez avoir plusieurs millions de clées dans Redis, je pense que ce qui a été énoncé ci-dessus peut-être un peu confus. Heureusement `sort` peut aussi fonctionner sur les hash et leurs champs. Au lieu d'avoir un ensemble de clées de niveau 1, vous pouvez vous services des hashes :

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

Non seulement tout est mieux organisé, vous pouvez trier par `severity` ou `priority`, mais vous pouvez aussi dire à `sort` quel champs doit être récupéré :

	sort watch:leto by bug:*->priority get bug:*->details

La même substitution de valeur est réalisée, mais Redis reconnaît aussi la flèche `->` et l'utiliser pour récupérer le champs en question au sein de votre hash. Nous avons aussi inclu le paramètre  `get`, qui réalise aussi une substitution and la recherche de champs afin de récupérer les détails des bugs.

Sur de grands ensembles, `sort` peut être lente. La bonne nouvelle est que la sortie peut être stockée :

	sort watch:leto by bug:*->priority get bug:*->details store watch_by_priority:leto

Ainsi, en combinant les capacités de `store` et de `sort` avec les commandes d'expiration que nous avons déjà vu, nous pouvons avoir des résultats plutôt intéressants.

## Dans ce chapitre

Ce chapitre s'est concentré sur les commandes non spécifiques aux structures de données. Comme tout le reste leur usage est circonstanciel . Ce n'est pas rare de construire une application ou une fonctionnalité qui ne fasse pas usage de l'expiration, des mécanismes de publication/souscription ou des tris. Mais il est bon de les connaître et de savoir qu'elles existent. De plus, nous n'avons fait qu'aborder certaines des commandes disponibles. Il y en a d'autres, et une fois que vous aurez digéré le contenu de ce livre, il est intéressant de parcourir la [liste complète](http://redis.io/commands).

# Chapitre 5 - Scripting en Lua

Redis 2.6 intègre un interpréteur Lua que les développeurs peuvent utiliser pour écrire des requêtes plus avancées qui pourront être exécutées par Redis. Il ne serait pas faux de voir cette capacité comme une manière de disposer des procédures stockées disponibles pour les bases de données relationnelles.

L'aspect le plus contraignant avec cette fonctionnalité est de devoir apprendre Lua. Heureusement, Lua est très proche de la plupart des langages classiques, est très bien documenté, possède une communauté active et est utile en dehors du scripting avec Redis. Ce chapitre ne convrira pas Lua en détail; mais les quelques exemples que nous aborderons pourront, je l'espère, servir d'introduction rapide.

## Pourquoi ?

Avant de regarder de quelle manière utiliser le scripting en Lua, vous pourriez vous demander pourquoi vous souhaiteriez l'utiliser. Un certain nombre de développeurs n'aime pas les procédures stockées traditionelles, est ce que ceci est différent ? La réponse courte est non. Mal utilisé, le scripting en Lua de Redis peut aboutir à un code plus difficile à tester, à une logique business grandement couplée avec les données voire même une logique qui sera dupliquée.

Correctement utilisé par contre, il s'agit d'une fonctionnalité qui peut simplifier le code et améliorer les performances. Chacun de ces bénéfices sont largement atteints grâce au rassemblement de plusieurs commande, à côté d'une logique simple, au sein d'un fonction ad hoc cohérente. Le code est rendu plus simple car chaque invocation à un script Lua est exécuté sans interruption et fourni ainsi une méthode simple et claire pour créer ses propres commandes atomiques (ce qui élimine notamment l'usage de la commande `watch`, un peu emcombrante il faut l'avouer). Ceci peut améliorer la performance en supprimant le besoin de retourner des résultats intermédiares - le résultat final pouvant être calculé au sein du script.

Les exemples des sections suivantes illustreront mieux ces points.

## Eval

La commande `eval` prend un script Lua en entrée (en tant que chaîne de caractères), les clées sur lesquelles le script va opérer, et un ensemble optionnel d'arguments arbitraires. Regardons un exemple simple (cet exemple est écrit en Ruby, dans la mesure où des commandes Redis sur plusieurs lignes ne sont pas très fun à exécuter depuis la ligne de commande):

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

Le code ci-dessus récupère les détails de tous les amis masculins de Leto. Il faut noter que pour réaliser les commandes vers Redis au sein de notre script, nous utilisons la méthode `redis.call("command", ARG1, ARG2, ...)` method.

Si vous n'êtes pas famillier avec Luad, vous devriez étudier avec soin chaque ligne de ce script. Il est par exemple utile de savoir que `{}` créé une `table` vide (qui peut correspondre à un tableau ou un dictionnaire), `#TABLE` récupère le nombre d'éléments dans ,  et `..` sert à la concaténation de chaînes.

La commande `eval` prend effectivement quatres paramètres. Le second paramètre doit être le nombre de clées; cependant le driver Ruby de Redis rempli pour nous la valeur de manière automatique. Pourquoi est-ce nécessaire? Considérez par exemple la manière dont ceci est exécuté depuis la ligne de commande :

    eval "....." "friends:leto" "m"
    vs
    eval "....." 1 "friends:leto" "m"

Dans le premier ca (incorrect), comment Redis peut-il savoir quel est le paramètre correspondant au nombre de clées et qu'est ce qui correspond aux paramètres arbitraires ? Dans le second cas, il n'y a pas d'ambiguités.

Ceci amène une seconde question : pourquoi les clées doivent elles être explicitement listées ? Chaque commande Redis sait, à l'exécution, quelles sont les clées qui vont être nécessaires. L'idée est que ceci puisse permettre l'existance de futurs outils, comme Redis Cluster, pour la distribution de requêtes sur de multiples serveurs Redis. Vous pourriez avoir noté que notre exemple ci-dessus lis les clées de manière dynamique (sans avoir à les passer à la commande `eval`). La commande `hget` est appelée pour tous les amis masculins de Leto. Ceci provient du fait que le fait de devoir disposer des clées en avance est plus une suggestion qu'un règle stricte. Le code ci-dessus s'exécutera sans problèmes sur une installation mono-instance, tout comme avec de la réplication, mais ne fonctionnera pas avec la version disponible de Redis Cluster.

## Gestion de scripts

Même si les scripts exécutés avec `eval` sont mis en cache par Redis, envoyer le corps de ce script à chaque fois que vous souhaitez exécuter quelque chose, n'est pas idéal. Au lieu de cela, il est possible d'enregistrer le script avec Redis et de l'exécuter avec sa clée. Pour réaliser cette opération, vous devez utiliser la commande `script load` qui retournera un hash (au sein digest du terme) SHA1 du script :

    redis = Redis.new
    script_key = redis.script(:load, "THE_SCRIPT")

Une fois que nous aurons chargé le script, nous pourrons utiliser la commande `evalsha` pour l'exécuter :

    redis.evalsha(script_key, ['friends:leto'], ['m'])

Les autres commandes `script kill`, `script flush` et `script exists` vous permettrons de gérer entièrement les scripts chargés. Elles sont utilisée pour tuer un script en cours d'exécution, supprimer tous les scripts inclus dans le cache, et savoir si un script existe déjà dans le cache de Redis.

## Bibliothèques

L'implémentation de Lua livré avec Redis dispose d'une certain nombre de bibliothèques très utiles. Alors que `table.lib`, `string.lib` et `math.lib` sont très utiles, pour moi, `cjson.lib` sort tout particulièrement du lot. Tout d'abord, si vous retrouvez à devoir passer plusieurs arguments à un script, il peut être plus pratique et propre de passer ces éléments au format JSON :

    redis.evalsha ".....", [KEY1], [JSON.fast_generate({gender: 'm', ghola: true})]

Qui peuvent ensuite être désérialisés au sein du script en Lua de la manière suivante :

    local arguments = cjson.decode(ARGV[1])

La bibliothèque JSON peut bien sûre être utilisée pour parser les valeurs stockées dans Redis lui-même. Notre exemple ci-dessus pourrait d'ailleurs être réécrit de la manière suivante :

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

Au lieu de récupérer le genre d'un champ spécifique d'un hash, nous pourrions le récupérer des données de l'ami stocké (C'est une solution bien plus lente, et je préfère personnellement la solution originale, mais celle-ci permet de montrer ce qu'il est possible de faire).

## Atomicité

Dans la mesure ou Redis est mono-processus, vous n'avez pas à vous préoccuper du fait que votre script Lua soit interrompu par une autre commande Redis. L'un des bénéfices les plus évident est que les clées avec un TTL n'expirerons pas à mi-chemin de l'exécution du script. Si une clée est présente au début du script, elle sera présente en tout point de celui-ci, sauf si vous la supprimez.

## Administration

Le prochain chapitre parlera plus en détail de l'administration de Redis et de sa configuration. Pour l'instant il est suffisant de savoir que `lua-time-limit` défini le temps qu'un script écrit en Lua peut prendre pour s'exécuter avant d'être arrêté. La valeur par défaut de 5 secondes est plutôt généreuse. Il vous est conseillé de la diminuer.

## Dans ce chapitre

Ce chapitre a introduit les capacités de scripting en Lua de Redis. Tout comme n'importe quelle fonctionnalité, elle peut être utilisé avec abus. Cependant, utilisée prudemment afin d'implémenter vos propres commandes, elle vous permettra non selement de simplifier votre code, mais aussi d'améliorer la performance. Le scripting en Lua est comme n'importe quelle autre commande/fonctionnalité de Redis: vous en faites un éventuel usage, limité au départ, seulement pour vous retrouver à l'utliser de plus en plus chaque jour. 

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


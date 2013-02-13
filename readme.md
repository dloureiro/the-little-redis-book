## À propos ##

Le petit livre sur Redis est un livre gratuit d'introduction à Redis.

Le livre a été écrit par [Karl Seguin](http://openmymind.net), avec l'assistance de [Perry Neal](http://twitter.com/perryneal).

Cette traduction française a été réalisée par [David Loureiro](http://twitter.com/DavidLoureiroFr), [Augustin Ragon](http://twitter.com/augustin82) et [Haikel Guémar](http://twitter.com/hguemar)

Si vous aimez ce livre, peut-être aimerez-vous [Le petit livre sur MongoDB](https://github.com/dloureiro/the-little-mongodb-book), traduction française réalisée par les mêmes personnes de [The Little MongoDB Book](http://openmymind.net/2011/3/28/The-Little-MongoDB-Book/), la version originale, aussi écrite par [Karl Seguin](http://openmymind.net), avec l'assistance de [Perry Neal](http://twitter.com/perryneal).  

## Licence ##
Ce livre est librement distribué sous la licence [Attribution-NonCommercial 3.0 Unported license](<http://creativecommons.org/licenses/by-nc/3.0/legalcode>).

## Version originale et Traductions ##

* [Version originale -Anglais](https://github.com/dloureiro/the-little-redis-book)
* [Russe](https://github.com/kondratovich/the-little-redis-book)
* [Italien](https://github.com/sandroconforto/the-little-redis-book) - [pdf](https://github.com/sandroconforto/the-little-redis-book/raw/master/book/redisIt.pdf)
* [Chinois](https://github.com/JasonLai256/the-little-redis-book)
* [Japanais](https://github.com/craftgear/the-little-redis-book/)

## Formats ##
Ce livre est rédigé en [Markdown](http://daringfireball.net/projects/markdown/) et converti en PDF en utilisant [Pandoc](http://johnmacfarlane.net/pandoc/).

Le template TeX fait usage de [Lena Herrmann's JavaScript highlighter](http://lenaherrmann.net/2010/05/20/javascript-syntax-highlighting-in-the-latex-listings-package).

Les formats Kindle et ePub sont fournis par [Pandoc](http://johnmacfarlane.net/pandoc/). 

## Génération des livres ##
Les paquets listés ci-dessous sont pour Ubunti. Si vous utilisez un autre Système d'Exploitation, les noms devraient être similaires.

### PDF

#### Dependances

Paquets:

* `pandoc`
* `texlive-xetex`
* `texlive-latex-extra`
* `texlive-latex-recommended`

Vous devriez aussi avec les polices Microsfts installées. Vous pouvez cependant changer les polices dans le fichier `common/pdf-template.tex` si vous le souhaitez.

#### Construction

Exécutez `make en/redis.pdf`.

### ePub

#### Dependances

Paquets:

* `pandoc`

#### Construction

Exécutez `make en/redis.epub`.

### Mobi

#### Dependances

Paquets:

* `pandoc`

Vous devez aussi avoir [KindleGen](http://www.amazon.com/gp/feature.html?ie=UTF8&docId=1000765211) d'installé.

#### Construction

Lancez `make en/redis.mobi`.

## Image de titre ##
Un PSD de l'image de titre est inclus. La police utilisée est [Comfortaa](http://www.dafont.com/comfortaa.font).

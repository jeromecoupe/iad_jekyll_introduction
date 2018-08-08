# Introduction à Jekyll

## Introduction

[Jekyll](http://jekyllrb.com) est un générateur de sites statiques écrit en Ruby et créé par [Tom Preston Werner](http://tom.preston-werner.com/). Le code source du projet est [disponible sur Github](https://github.com/jekyll/jekyll).

Jekyll vous permet de développer des sites basés sur des templates dynamiques (codés avec [Liquid](http://liquidmarkup.org/)) et des fichiers de contenus (YAML / Markdown / HTML / JSON / CSV). Jekyll n'utilise pas de base de données. Sur base de ces fichiers, Jekyll va générer un site entièrement statique que vous pourrez ensuite déployer sur n'importe quel serveur web ou sur [Github Pages](https://pages.github.com/).

Le workflow le plus répandu consiste à générer son site localement et à mettre en ligne les fichiers statiques ainsi générés.

Les avantages des générateurs de sites statiques peuvent être résumés comme suit:

- Tout est basé sur des fichiers. Votre site peut être entièrement géré par un système de version control comme Git
- De simples fichiers statiques sont incroyablement faciles à héberger sur n'importe quelle infrastructure et rendent votre site très rapide à charger.
- Votre infrastructure serveur reste très simple et offres moins de possibilités aux hackers qu'un site dynamique.

## Installation

Pour installer Jekyll, il nous faut d'abord installer Ruby.

### Installation de Ruby

Ruby est installé par défaut sur les Mac. Voici cependant une marche à suivre pour installer Ruby ou mettre à jour la version dont vous disposez.

Si vous travaillez sur Windows, utiliser [Ruby Installer](http://rubyinstaller.org) et sans doute le plus facile. Il suffit ensuite d'installer la gem Jekyll comme spécifié en infra.

#### Installation de Homebrew

Homebrew est un outil vous permettant d'installer facilement des packages Unix sur votre Mac.

`ruby -e "$(curl -fsSL https://raw.github.com/mxcl/homebrew/go)"`

Faites ensuite un petit `brew doctor` pour vérifier que tout est en ordre. Si vous avez un problème de paths, [voici la solution](http://stackoverflow.com/questions/10343834/homebrew-wants-me-to-amend-my-path-no-clue-how).

#### Installation de RVM

La méthode conseillée sur mac est d'installer RVM (Ruby Version Manager) qui vous permettra facilement de gérer différentes versions de Ruby sur votre machine (ce qui peut s'avérer pratique, faites-moi confiance).

`\curl -#L https://get.rvm.io | bash -s stable --ruby`

Cette commande installera la dernière version stable de RVM, ainsi que la dernière version stable de Ruby. N'oubliez pas de redémarrer votre terminal pour que les changements soient effectifs.

#### Mise à Jour de RVM

Pour être certain de disposer de la dernière version de RVM, utilisez la commande suivante :

`rvm get stable`

#### Mise à jour de Ruby

Chercher quelle est la dernière version stable de Ruby disponible :

`rvm list known`

Vous obtiendrez une longue liste de versions. Installez la dernière en date à l'aide de la commande suivante (remplacez la version par la dernière en date) et ... répondez "oui" à toutes les questions posées.

`rvm install 2.2.3`

Vous pouvez ensuite choisir quelle version de Ruby vous voulez utiliser :

`rvm list` et `rvm --default use 2.2.3`

### Installation des gems Bundler et Jekyll

Bundler est un outil de gestion des dépendances en Ruby. Il vous permet de spécifier quelle version d'une gem vous voulez utiliser pour un projet et d'exécuter toutes vos commandes sur la version de la gem spécifiée dans votre Gemfile. Bundler est maintenant la manière conseillée d'installer Jekyll.

Pour installer Bundler et Jekyll, vous n'avez qu'à utiliser la commande suivante:

`gem install bundler jekyll`

Si vous souhaitez simplement installer Jekyll, tapez simplement `gem install jekyll`.

### Mise à jour de Jekyll

Si vous n'installer pas Bundler, `gem update jekyll` vous permettra de mettre à jour votre version de Jekyll lorsqu'une nouvelle version est disponible.

Si bundler est installé, il suffit de modifier la version de Jekyll spécifiée dans votre Gemfile et de taper `bundle update`

## Utilisation

### Mise en place

Pour utiliser Jekyll, il vous suffit de créer un dossier dans votre environnement de développement local et d'utiliser la commande suivante :

`jekyll new mondossiersite/` ou `jekyll new .` pour installer Jekyll dans le dossier courant.

Jekyll devrait vous créer une arborescence de dossiers et de fichiers. L'installation par défaut de Jekyll utilise des themes et de plugins. Nous n'en utiliserons pas ici. Il vous faudra donc reproduire précisément cette arborescence de dossiers et de fichiers:

- **_config.yml**: fichier de configuration principal de votre site Jekyll (supprimer les options liées au thèmes et aux plugins (voir infra))
- **_includes**: contient vos includes (créer le dossier)
- **_layouts**: contient vos fichiers de layout (créer le dossier)
- **_posts**: dossier contenant vos blogposts (supprimer les fichiers créés lors de l'installation)
- **_sass**: contient vos fichiers .scss
- **css**: contient votre fichier .scss maître qui importe les autres
- **about.md**: supprimer le fichier
- **index.md**: supprimer le fichier
- **index.html**: la homepage de votre site (faire une page HTML simple type 'hello world')
- **Gemfile**: configuration de Bundler
- **Gemfile.lock**: fichier utilisé par Bundler pour manager vos dépendances

Pour ne pas utiliser de thèmes ni de plugins, il vous faudra commenter (en ajoutant un caractère '#') les lignes suivantes dans votre Gemfile

```
# gem "minima", "~> 2.0"
```
et

```
# gem "jekyll-feed", "~> 0.6"
```

Veuillez aussi supprimer les lignes liées aux thèmes et aux plugins de votre fichier `_config.yaml`:

```
theme: minima
```

### Concepts et commandes de base

Jekyll fonctionne en parcourant cette arborescence pour générer un site statique sur base des fichiers et dossiers trouvés. Ce site statique est généré dans le répertoire `_site` par défaut.

- Jekyll va traiter tous les fichiers contenant un YAML Front Matter (y compris un YAML Front Matter vide), les sauver temporairement et les rendre utilisables par Liquid et les tags Jekyll.
- les dossiers et fichiers dont le nom commence par un underscore ne seront pas transférés dans le dossier `_site`, tandis que les dossiers et fichiers dont le nom ne commence pas par un underscore seront transférés dans le dossier `_site`.

Une fois dans le dossier de votre projet, les commandes `jekyll build` ou `bundle exec jekyll build` est utilisée pour générer votre site. Si vous souhaitez que ce site soit généré à nouveau automatiquement dès que Jekyll détecte des changements dans votre projet, utilisez la commande `jekyll build --watch`.

Vous pouvez également utiliser `jekyll serve` ou `bundle exec jekyll serve` pour vous permettre de visualiser votre site sur un serveur de développement à l'adresse `http://localhost:4000/` (la régénération automatique est activée par défaut).

Depuis Jekyll 3.0.0, vous pouvez utiliser `jekyll build --incremental` pour régénérer votre site de façon incrémentale et gagner du temps, ainsi que `jekyll build --profiler` pour faire tourner un profiler vous montrant le temps de build pour chaque ressource.

### Configuration

Les [options de configuration globales](http://jekyllrb.com/docs/configuration/#global-configuration) de votre site sont stockées dans des variables définies dans le fichier `_config.yml`.

L'ensemble des variables définies dans le fichier `_config.yml` sont accessibles dans vos templates via l'objet `site` en syntaxe pointée. Vous pouvez par exemple accéder à la variable `title` de votre `config.yaml` en utilisant `site.title`

Vous pouvez définir vos propres variables dans ce fichier. Elles seront alors accessibles dans vos templates via l'objet `site` en syntaxe pointée. Par exemple, si vous définissez une variable `test` dans le fichier `_config.yml`, vous pourrez y accéder dans vos templates en utilisant `site.test`.

Certaines de ces options de configuration sont implicites ([spécifiées par défaut](http://jekyllrb.com/docs/configuration/)) mais peuvent être surdéterminées si besoin est.

## Créer votre data structure

Jekyll vous propose différentes façons de créer vos données et de les structurer. Les trois outils principaux à votre disposition sont :

- **pages**: permettent de gérer contenus isolés, sans aucun lien logique avec d'autres (homepage, contact, etc.)
- **collections**: permettent de gérer des contenus liés entre eux de façon logique à travers des séries de fichiers HTML ou Markdown. Les documents d'une collection peuvent éventuellement générer un fichier lorsque le site est généré (`output: true`) et également avoir une URL spécifique (structure spécifiée à l'aide de la variable `permalink`). A partir de Jekyll 3.0.0 les posts sont une collection ayant un statut un peu particulier (cf infra).
- **data**: permet de gérer des contenus structurés dans des formats tels que YAML ou JSON. Ces données ne généreront pas de fichiers propres avec des URLs distinctes.

### Pages

[Les pages](http://jekyllrb.com/docs/pages/) sont de simples fichiers HTML ou Markdown. Vous pouvez utiliser leur YAML Front Matter pour créer vos propres variables ou pour utiliser les variables fournies par défaut par Jekyll.

Vous pouvez accéder à l'ensemble de ces variables via l'objet `page` en syntaxe pointée. Par exemple, vous pouvez accéder dans le corps de la page à une variable `test` contenue dans son YAML Front Matter en utilisant `page.test`.

### Collections

[Les collections](http://jekyllrb.com/docs/collections/) permettent de créer différents types de contenus liés entre eux de façon logique. Pour créer une collection, il suffit de créer un dossier nommé `_macollection` et puis de définir `macollection` dans votre fichier `_config.yml`. Le nom du dossier sans tenir compte du caractère `_` doit être identique au nom de la collection spécifié dans le fichier `_config.yml`.

```yaml
collections:
  macollection:
```

Vous pouvez spécifier pour chaque collection si les documents qui la composent vont générer un output (un fichier propre) après traitement par Jekyll. Vous pouvez définir l'URL de ces fichiers via la variable `permalink` (voir infra). Les fichiers d'une collection vont également générer automatiquement une variable `date` si vos noms de fichiers commencent par une date.

```yaml
collections:
  macollection:
    output: true
    permalink: /collection/:year/:title
```

Des caractéristiques communes pour tous les documents d'une collection peuvent être définies via des [variables YAML par défaut](http://jekyllrb.com/docs/configuration/#Front Matter-defaults) (voir infra).

#### Posts : une collection particulière

[Les posts](http://jekyllrb.com/docs/posts/) viennent du fait que Jekyll a été conçu à la base comme un outil de blogging. Toutes les installations de Jekyll comprennent donc une collection par défaut nommée `_posts` qui est définie implicitement par Jekyll.

Cette collection `posts` possède encore aujourd'hui un statut particulier et des fonctionnalités qui lui sont propres et qui ne sont pas disponibles pour les autres collections. Un exemple est la gestion des categories et des tags, qui n'existe à ce jour que pour les posts.

### YAML Front Matter et variables de page

L'ensemble des variables définies dans les YAML Front Matter sont accessibles dans vos templates via l'objet `page` en syntaxe pointée.

Certaines variables sont propres à Jekyll et peuvent être utilisées pour l'ensemble de vos fichiers :

- **layout :** spécifie le layout à utiliser (voir la section consacrée au layout)
- **permalink :** spécifie le permalink et le chemin à utiliser pour l'output du fichier
- **published :** spécifie si le fichier doit être publié ou pas
- **category / categories :** spécifie les catégories à appliquer au fichier
- **tags :** spécifie les tags à appliquer au fichier

Vous pouvez également définir vos propres variables dans les YAML Front Matter de vos fichiers en utilisant les différents types de données disponibles en YAML : strings, lists, nested lists, etc.

```
---
title: mon titre
summary: ceci est un résumé
maVariable: test
maListe:
  - item 1
  - item 2
  - item 3
---
## Titre de second niveau

Lorem ipsum dolor sit amet, consectetur adipisicing elit. Cumque nobis perspiciatis eligendi nesciunt vel ea sunt illo deserunt earum id quaerat, odit, enim! Earum corporis, numquam iure facilis cumque ratione.
```

#### YAML Front Matter defaults

Plutôt que de définir les caractéristiques communes des fichiers dans leur YAML Front Matters individuels, vous pouvez utiliser votre fichier `_config.yml` et spécifier des [valeurs de YAML Front Matter par défaut](http://jekyllrb.com/docs/configuration/#front-matter-defaults). Via la variable `defaults:`, votre fichier de configuration vous permet de définir des valeurs par défaut pour des variables communes à un ensemble de fichiers.

Ces ensembles sont définis par une liste de défauts. Chacun de ces défauts dans cette liste est défini par deux variables:

1. `scope` qui prend comme valeurs un `path` (obligatoire) et un `type` (optionnel).
2. `values` qui défini vos variables par défaut et leurs valeurs

- **path** : chemin depuis la racine du projet. Une valeur pour path est obligatoire, même si vous utilisez un type. Une valeur vide permet de cibler l'ensemble des fichiers du site.
- **type** : type de fichier. Les types disponibles sont `pages`, `posts`, `nomcollection`.

Vous pouvez ainsi spécifier facilement la valeur par défaut pour les variables `layout`, `category` ou `permalink` propres à Jekyll ou encore pour une variable de votre cru. Ces valeurs par défaut peuvent être surdéterminées par les valeurs dans le YAML Front Matter des fichiers individuels.

```yaml
defaults:
  - scope:
      path: "/about"
    values:
      layout: "default"
      myVariable: myValue
```

or

```yaml
defaults:
  - scope:
      path: "" # all files in your project
      type: "portfolio" # files in the "portfolio" collection
    values:
      layout: "portfolio"
      myVariable: myValue
  - scope:
      path: "" # all files in your project
      type: "posts" # files in the "posts" collection
    values:
      layout: "default"
      myVariable: myValue
```

### Data

Jekyll permet également de définir des [fichiers de données](http://jekyllrb.com/docs/datafiles/). Le dossier `_data` permet de stocker des fichiers structurés en YAML, JSON ou CSV et rendre ces données disponibles pour Liquid et Jekyll. Ces données seront accessibles via l'objet `site.data`. Par exemple, pour accéder aux données du fichier `_data/mesdonnees.json` en Liquid, nous pouvons utiliser l'objet `site.data.mesdonnees`.

## Créer vos templates

Maintenant que nous avons vu comment créer vos données dans Jekyll, voyons comment utiliser ces données structurées dans vos templates.

### Liquid comme langage de templating

Jekyll utilise [Liquid](https://github.com/Shopify/liquid/wiki/Liquid-for-Designers) comme langage de templating. Liquid à été créé par [Shopify](https://www.shopify.com/) pour permettre à ses utilisateurs de créer et d'appliquer leurs propres templates à leurs projets d'e-commerce.

Liquid reste un langage très simple mais qui permet tout de même une grande souplesse d'utilisation. En plus des tags Liquid standards, Jekyll offre aussi quelques [filtres](http://jekyllrb.com/docs/templates/#filters) et [tags](http://jekyllrb.com/docs/templates/#tags) qui lui sont propres.

### Tags

Liquid comporte deux principaux types de tags :

1. Les tags d'affichage `{{ affichage }}` qui permettent d'écrire des variables dans vos templates.
2. Les tags de logique ou d'exécution `{% logique %}`. Par exemple `{% if %}`, `{% endif %}` ou `{% assign mavariable = site.macollection %}`.

### Accéder à vos données

Lorsque Jekyll tourne, il vous créée une série de variables auxquelles vous avez accès via Liquid.

#### Variables globales

- `site`: utilisée principalement pour accéder aux tableaux reprenant vos pages, posts et documents de collections. Egalement utilisé pour accéder aux variables spécifiées dans votre fichier `_config.yml`. Voir plus bas.
- `page`: utilisée principalement pour accéder aux variables spécifiées via les YAML Front Matters de vos fichiers individuels ou via les YAML Front Matter par défaut spécifiées dans votre fichier `_config.yml`.
- `content`: variable spéciale utilisée uniquement dans les fichiers de layout. Cette variable sera remplacée par le contenu du post, de la page ou du document membre d'une collection auquel le fichier de layout est appliqué. Voir plus bas.

#### Variables liées aux collections, pages et data

`site.maCollection` retournera un tableau des documents appartenant à une collection spécifique. `posts` n'est rien d'autre qu'une collection spécialisée que Jekyll créée par défaut. Vous pouvez donc accéder à un tableau de l'ensemble de vos posts en utilisant `site.posts`. Si vous avez défini une collection `projets`, vous pourriez boucler sur les documents de cette collection en utilisant le code suivant:

```Liquid
{% for project in site.projects %}
  <h2>{{ project.title }}</h2>
{% else %}
  <p>Sorry, I cannot find any project</p>
{% endfor %}
```

Les variables définies via les YAML Front Matter (individuels ou par défaut) sont accessibles via une syntaxe pointée dans les boucles `for`.

```Liquid
{% for project in site.projects %}
  <h2><a href="{{ project.url }}">{{ project.title }}</a></h2>
  <p>{{ project.summary }}</p>
{% else %}
  <p>Sorry, I cannot find any project</p>
{% endfor %}
```

La variable `summary` dans le YAML Front Matter de tous les projets, est accessible dans cette boucle `for` en utilisant `project.summary`. La variable `project.url` est automatiquement créée par Jekyll sur base du pattern de permalink défini pour les documents de cette collection, soit via leurs YAML Front Matter individuels, soit via les YAML Front Matter par défaut dans le fichier `_config.yml`.

Jekyll vous donne également accès à d'autres variables globales liées à vos pages, collections et data. Voici sans doute celles que vous utiliserez le plus:

- `site.pages`: un tableau de toutes vos pages.
- `site.collections`: un tableau de toutes vos collections et de [leurs attributs](http://jekyllrb.com/docs/collections/#collections)
- `site.data`: un tableau contenant les données définies dans vos fichiers de données placés dans le dossier `_data` de votre installation. Pour accéder aux données dans un fichier individuel, vous pouvez simplement utiliser `site.data.nomdufichier`.
- `site.categories.CATEGORY`: un tableau de tous les posts appartenant à la catégorie CATEGORY. `site.categories.work` vous donnera accès à un tableau de tous les posts dans la catégorie "work". Ceci est uniquement disponible pour les posts et n'est pas disponible pour les items des autres collections.
- `site.tags.TAG`: un tableau de tous les posts auxquels le tag TAG est appliqué. `site.tags.jekyll` vous donnera accès à un tableau de tous les posts auxquels le tag "jekyll" est appliqué. Ceci est uniquement disponible pour les posts et n'est pas disponible pour les items des autres collections.

#### Variables dans les fichiers individuels

Vous pouvez facilement accéder aux variables définies dans vos YAML Front Matter (individuels ou par défaut) en utilisant la variable `page` et une syntaxe pointée. Si vous avez défini une variable `test` dans le YAML Front Matter d'une page, d'un post ou d'un item de collection, vous pouvez y accéder via `page.test`. Ces variables seront également disponibles dans les fichiers de layout référencés dans vos pages, posts et documents de collections.

En plus des variables que vous définissez, Jekyll créée [automatiquement certaines variables](http://jekyllrb.com/docs/variables/#page-variables) pour vos pages, posts et documents de collection. Voici les principales:

- `page.title`: le titre du post, de la page ou du document appartenant à une collection
- `page.content`: le contenu de la page. Ce que la variable spéciale `{{ content }}` affichera dans un fichier de layout.
- `page.date`: la date assignée au post ou au document d'une collection. Peut être spécifiée soit en utilisant le nom de fichier, soit via les variables de YAML Front Matter (individuels ou par défaut)
- `page.url`: l'URL de la page, du post ou du document appartenant à une collection, précédée d'un slash. Par exemple: `/2008/12/14/my-post.html`.
- `page.next`: le post suivant par rapport à la position du post dans le tableau `site.posts`. Retourne `nil` pour la dernière entrée.
- `page.previous`: le post précédent par rapport à la position du post dans le tableau `site.posts`. Retourne `nil` pour la première entrée.

#### Variables de configuration

La variable `site` peut également être utilisée pour accéder aux valeurs des variables définies dans votre fichier `_config.yml`. Si vous avez défini une variable `test` dans votre fichier `_config.yml`, vous pouvez y accéder en utilisant simplement `site.test`.

### Rester DRY: includes et layouts

#### Layouts

Jekyll vous permet de travailler à l'aide de fichiers de layout. Ceux-ci sont par défaut stockés dans le directory `_layout`. Un template enfant utilise un template parent et toutes les variables disponibles dans le template enfant le sont aussi dans le template de layout. La variable spéciale `{{ content }}` est remplacée dans le fichier de layout par le contenu du fichier enfant.

Pour utiliser un layout, il suffit de spécifier le nom de fichier du layout à utiliser dans le YAML Front Matter du fichier enfant. Jekyll ira chercher le fichier de layout dans le dossier `_layouts`. Vous pouvez modifier cela dans votre fichier `config.yaml` via la variable `layouts_dir:  ./_layouts`.

Voici à quoi cela ressemble :

**enfant**: *index.html*
```
---
layout: default
title: Mon premier fichier Jekyll
---

<h1>{{ page.title }}</h1>
<p>Hello world !</p>

```

**parent**: *_layouts/default.html*
```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>{% if page.title %}{{ page.title }} - {% endif %}{{ site.title }}</title>
</head>
<body>
  {{ content }}
</body>
</html>
```

Vous pouvez également imbriquer différents layouts. Si vous avez besoin d'un layout particulier pour un certain type d'items par exemple, vous pouvez travailler de la façon suivante.

**enfant**: *_posts/2015-12-10-my-first-blogpost.md*
```
---
layout: blogpost
title: Le titre de mon blogpost
---

Le contenu de mon blogpost
```

**template parent 1**: *_layouts/blogpost.html*
```
---
layout: default
---
<h1>{{ page.title }}</h1>
{{ content }}
```

**template parent 2**: *_layouts/default.html*
```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>{% if page.title %}{{ page.title }} - {% endif %}{{ site.title }}</title>
</head>
<body>
  {{ content }}
</body>
</html>
```

#### Includes

Liquid et Jekyll vous permettent également d'utiliser des includes pour stocker les morceaux de code appelés à se répéter. Par défaut, Jekyll cherchera vos fichiers includes dans le répertoire `_includes`. Cela peut être modifié dans votre fichier `_config.yml` via la variable `includes_dir: ./_includes`.

```Liquid
{% include sidebar.html %}
```

Vous pouvez également passer des variables à vos includes.

```Liquid
{% include sidebar.html searchWidget="true" %}
{% include sidebar.html searchWidget=page.search %}
```

Pour les récupérer dans le cadre de votre fichier include, il suffit d'utiliser `include.mavariable`

```Liquid
{% if include.searchWidget == "true" %}
  ... code du search widget ...
{% endif %}
```

### Loops et structures de contrôle

Liquid vous permet d'utiliser des boucles `for`, ce qui est particulièrement utile pour traiter vos objets et autres tableaux. Par exemple, pour afficher l'ensemble des `posts` de votre site, il suffit d'utiliser la syntaxe suivante:

```Liquid
{% for item in site.posts %}
  {{ item.title }}
{% endfor %}
```

Si vous souhaitez vous limiter aux deux derniers posts, vous pouvez utiliser le paramètre `limit`.

```Liquid
{% for item in site.posts limit:2 %}
 {{ item.title }}
{% endfor %}
```

Si vous souhaitez omettre les deux premiers posts de votre boucle, vous pouvez utiliser le paramètre `offset`.

```Liquid
{% for item in site.posts offset:1 %}
 {{ item.title }}
{% endfor %}
```

Liquid vous permet également d'utiliser les structures de contrôle classiques telles que [`if`](https://github.com/shopify/liquid/wiki/Liquid-for-Designers#if--else) ou [`case`](https://github.com/shopify/liquid/wiki/Liquid-for-Designers#case-statement) dans le cadre de vos templates.

```Liquid
{% if user.age > 18 %}
  <p>Would you like a beer ?</p>
{% else %}
  <p>We have orange juice, limonade, etc.</p>
{% endif %}
```

Couplées à une boucle `for` et aux [variables de loop](https://github.com/shopify/liquid/wiki/Liquid-for-Designers#for-loops), les structures de contrôle vous permettent d'avoir un code HTML propre.

```Liquid
{% for item in site.posts %}
  {% if foorloop.first %}<ul>{% endif %}
    <li>{{ item.title }}</li>
  {% if foorloop.last %}</ul>{% endif %}
{% else %}
  <p>No blogposts found</p>
{% endfor %}
```

### Les tags `assign` et `capture`

Commençons par le tag `assign` qui vous permet simplement de créer une variable et d'y d'assigner une valeur.

```Liquid
{% assign blogpostsPerTitle = site.posts | sort: 'title' %}

{% for item in blogpostsPerTitle reversed %}
  {% if foorloop.first %}<ul>{% endif %}
    <li>{{ item.title }}</li>
  {% if foorloop.last %}</ul>{% endif %}
{% else %}
  <p>No blogposts found</p>
{% endfor %}
```

Dans ce cas précis, combiner un classement alphabétique sur le titre et le paramètre `reversed` n'est possible qu'en faisant les choses en deux étapes en utilisant `assign`. Voici également une autre application combinant plusieurs filtres et utilisant le paramètre `limit`. Grâce à `assign`, le code reste très lisible.

```Liquid
{% assign blogpostsPerTitle = site.posts | sort: 'title' | reverse %}

{% for item in blogpostsPerTitle limit:2 %}
  {% if foorloop.first %}<ul>{% endif %}
    <li>{{ item.title }}</li>
  {% if foorloop.last %}</ul>{% endif %}
{% else %}
  <p>No blogposts found</p>
{% endfor %}
```

`capture` permet, comme son nom l'indique, de capturer plusieurs chaînes de caractères et de les stocker dans une variable.

```Liquid
{% capture fullName %}{{ item.name | capitalize }} {{ item.surname | capitalize }}{% endcapture %}
```

Cette fonctionnalité peut être très utile dans certaines situations, par exemple pour créer une archive de vos posts classés par année:

```Liquid
{% assign allPosts = site.posts | sort: 'post.date' %}
{% for item in allPosts %}

  {% if forloop.first %}<ul>{% endif %}

  {% capture currentYear %}{{ item.date | date: "%Y" }}{% endcapture %}

    {% if postYear != currentYear %}
      {% if forloop.index != 1 %}</ul>{% endif %}
      <h2>{{ currentYear }}</h2>
      <ul>
    {% endif %}

    <li>{{ item.title }}</li>

    {% capture postYear %}{{ item.date | date: "%Y" }}{% endcapture %}

  {% if forloop.last %}</ul>{% endif %}

{% endfor %}
```

### Filtres: `sort`, `group_by`, `where` et `where_exp`

Liquid possède [une série de filtres](https://github.com/shopify/liquid/wiki/Liquid-for-Designers#standard-filters) qui vous permettent de transformer des chaînes de cractères, des chiffres, des tableaux et des objets de diverses façons.

Jekyll possède également quelques filtres qui lui sont propres. Parmi eux, trois sont essentiels au niveau de la manipulation de tableaux ou de hashes.

- `sort` : permet, comme nous venons de le voir, de trier un tableau ou un hash à l'aide d'une de ses valeurs.
- `group_by` : permet de grouper un tableau ou un hash par l'une de ses variables ou keys.
- `where` : permet de sélectionner les éléments d'un tableau ou d'un hash pour lesquels la key spécifiée à la valeur reseignée.
- `where_exp`: permet de sélectionner les éléments d'un tableau ou d'un hash si l'expression renseignée est vraie.

Voici quelques exemples d'applications pour les filtres `group_by` et `where` :

`group_by` et des boucles imbriquées permettent par exemple de grouper vos posts par l'une de leurs propriétés. Par exemple, si chacun de vos posts possède une variable `postType` dans son YAML Front Matter, voici comment créer une archive par type.

```Liquid
<h2>Archive par type</h2>

{% assign postsByTypes = site.posts | group_by: "postType" %}

{% for type in postsByTypes %}
  <h3>{{ type.name }}</h3>
  {% for item in type.items %}
    {% if forloop.first %}<ul>{% endif %}
      <li>{{ item.title }}</li>
    {% if forloop.last %}</ul>{% endif %}
  {% endfor %}
{% endfor %}
```

Le filtre `where` va permettre de filtrer les éléments d'un array, par exemple pour n'afficher que les posts écrit par un auteur. Pour que cela fonctionne, il faut évidemment qu'une variable `author` soit définie dans le YAML Front Matter de chacun de vos posts.

```Liquid
<h2>Post par auteur</h2>

{% assign postsByAuthor = site.posts | where:"author","Gengis Khan" %}

{% for item in postsByAuthor %}
  {% if forloop.first %}<ul>{% endif %}
    <li>{{ item.title }}</li>
  {% if forloop.last %}</ul>{% endif %}
{% endfor %}
```

Le filtre `where` ne vous permet de que de tester une égalité stricte, tandis que le filtre `where_exp` vous offre quant à lui d'avantage de possibilités.

```liquid
{% assign projectsByYear = site.projects | where_exp:"item", "item.projectYear == 2014" %}

{% for item in projectsByYear %}
  {% if forloop.first %}<ul>{% endif %}
    <li>{{ item.title }}</li>
  {% if forloop.last %}</ul>{% endif %}
{% endfor %}
```

```liquid
{% assign recentProjects = site.projects | where_exp:"item", "item.projectYear > 2014" %}

{% for item in recentProjects %}
  {% if forloop.first %}<ul>{% endif %}
    <li>{{ item.title }}</li>
  {% if forloop.last %}</ul>{% endif %}
{% endfor %}
```

```liquid
{% assign codeProjects = site.projects | where_exp:"item", "item.projectTags contains 'code'" %}

{% for item in codeProjects %}
  {% if forloop.first %}<ul>{% endif %}
    <li>{{ item.title }}</li>
  {% if forloop.last %}</ul>{% endif %}
{% endfor %}
```

### Data et fichiers YAML, JSON, CSV

Comme dit plus haut, Jekyll vous permet de manipuler à l'aide de Liquid des fichiers de données structurées. Voici un exemple de navigation créée sur base d'un fichier YAML.

**nav**: *_data/nav.yaml*
```yaml
- navLabel: 'Home'
  navLink: '/'

- navLabel: 'Blog'
  navLink: '/blog/'

- navLabel: 'Portfolio'
  navLink: '/work/'

- navLabel: 'Contact'
  navLink: '/contact/'
```

**nav**: *_includes/mainnav.html*
```Liquid
{% assign navData = site.data.nav %}

{% for item in navData %}

  {% if page.currentNav | downcase == item.navLabel | downcase %}
    {% assign currentClass = "  mainnav__item--current" %}
  {% else %}
    {% assign currentClass = "" %}
  {% endif %}

  {% if forloop.first %}<ul class="mainnav">{% endif %}
    <li class="mainnav__item{{ currentClass }}"><a href="{{ item.navLink }}">{{ item.navLabel }}</a></li>
  {% if forloop.last %}</ul>{% endif %}

{% endfor %}
```

## Jekyll et GitHub Pages

Jekyll est aussi le moteur de Github Pages, un outil qui permet de gérer et d'héberger des sites statiques. Tous les repositories GitHub permettent de faire tourner Jekyll. La marche à suivre est différente selon qu'il s'agit d'un projet ou d'une page de user ou d'organisation mais cela reste assez simple.

Le duo Jekyll et GitHub Pages vous permet de disposer d'un environnement collaboratif, d'un hébergement gratuit et d'un site entièrement géré via Git.

## Ressources

- [Jekyll](http://jekyllrb.com/) : le site officiel
- [Jekyll Talk](https://talk.jekyllrb.com/) : le forum officiel pour poser vos question, discuter Jekyll, etc.
- [Installing Jekyll](http://davidensinger.com/2013/03/installing-jekyll/) - David Ensinger : tout ce dont vous avez besoin pour installer Jekyll sur un Mac
- [Liquid for Designers](https://github.com/Shopify/liquid/wiki/Liquid-for-Designers) : une bonne introduction aux principaux tags et filtres de Liquid. N'oubliez pas de regarder également les [filtres](http://jekyllrb.com/docs/templates/#filters) et [tags](http://jekyllrb.com/docs/templates/#tags) propres à Jekyll.
- [Jekyll: much more than a static site generator](http://webstoemp.com/blog/jekyll-more-than-a-blog-generator/) - Jérôme Coupé : Un blogpost par votre serviteur sur les nouveautés dans Jekyll 2.0
- [Using Jekyll and GitHub Pages for Our Site](https://developmentseed.org/blog/2011/09/09/jekyll-github-pages/) - Young Hahn : sans doute l'une des meilleures introduction à Jekyll sur le plan de la philosophie de travail. Inclus également quelques perspectives techniques.
- [Jekyll and CMS-less websites with Young Hahn and Dave Cole](http://5by5.tv/webahead/54) (Podcast) - Jen Simmons, Young Hahn, Dave Cole : épisode du podcast "The Web Ahead" consacré à Jekyll et à d'autres *file-based CMS*.
- [Intro to Jekyll](https://www.youtube.com/watch?v=O7NBEFmA7yA) (Video) - Johan Ronsse : une bonne introduction à Jekyll comme outil de prototypage.
- [Jekyll From Scratch - Getting Started](http://pixelcog.com/blog/2013/jekyll-from-scratch-introduction/), [Jekyll From Scratch - Core Architecture](http://pixelcog.com/blog/2013/jekyll-from-scratch-core-architecture/) et [Jekyll From Scratch - Extending Jekyll](http://pixelcog.com/blog/2013/jekyll-from-scratch-extending-jekyll/) - Mike Greiling : introduction très complète à tous les aspects de Jekyll.
- [Get Started With GitHub Pages (Plus Bonus Jekyll)](https://24ways.org/2013/get-started-with-github-pages/) - Anna Debenham : bonne introduction au duo Github Pages et Jekyll
- [Build A Blog With Jekyll And GitHub Pages](http://www.smashingmagazine.com/2014/08/build-blog-jekyll-github-pages/) - Barry Clark : une  introduction centrée sur un blog.
- [Front-end style guides with Jekyll](http://webstoemp.com/blog/front-end-style-guides-jekyll/) - Jérôme Coupé : utilisation de Jekyll pour réaliser facilement des styles guides.
- [Static Sites Go All Hollywood](https://www.youtube.com/watch?v=_cuZcnJIjls) - Phil Hawksworth: une bonne introduction aux avantages, inconvénients et workflows liés aux static sites generators.
- [The JAM Stack](https://vimeo.com/163522126) - Mathias Bilmann: discussion en profondeur sur les avantages des sites statiques et leurs possibles évolutions

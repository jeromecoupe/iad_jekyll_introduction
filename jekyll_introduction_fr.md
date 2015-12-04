# Jekyll

## Introduction

[Jekyll](http://jekyllrb.com) est un générateur de sites statiques créé par [Tom Preston Werner](http://tom.preston-werner.com/). Jekyll est un projet open source maintenu par [Parker Moore](https://byparker.com/) et la communauté.

Jekyll vous permet de développer des sites basés sur des templates dynamiques (codés avec [Liquid](http://liquidmarkup.org/)) et des fichiers de contenus (YAML / Markdown / HTML / JSON / CSV). Sur base de ces fichiers, Jekyll va générer un site entièrement statique que vous pourrez ensuite déployer sur n'importe quel serveur web ou sur [Github Pages](https://pages.github.com/).

Jekyll tourne sous [Ruby](https://www.ruby-lang.org). Le workflow le plus répandu consiste à générer son site localement et à mettre en ligne les fichiers statiques ainsi générés.

Si vous utilisez GitHub Pages, il vous suffira de mettre en ligne l'ensemble de vos fichiers, une instance de Jekyll tournant sur Github se chargera de générer votre site pour vous.  

## Installation

Pour installer Jekyll, il nous fait d'abord installer Ruby. Voici une petite marche à suivre.

### Installation de Ruby

Ruby est installé par défaut sur les mac. Voici cependant la procédure à suivre pour installer Ruby ou mettre à jour la version dont vous disposez.

#### Installation de Homebrew

Homebrew est un outil vous permettant d'installer facilement des packages unix sur votre mac.

`ruby -e "$(curl -fsSL https://raw.github.com/mxcl/homebrew/go)"`

Faites ensuite un petit `brew doctor` pour vérifier que tout est en ordre. Si vous avez un problème de paths, [voici la solution](http://stackoverflow.com/questions/10343834/homebrew-wants-me-to-amend-my-path-no-clue-how).

#### Installation de RVM

La méthode conseillée sur mac est d'installer RVM (Ruby Version Manager) qui vous permettra facilement de manager Ruby sur votre machine.

`\curl -#L https://get.rvm.io | bash -s stable --ruby`

Cette commande installera la dernière version stable de RVM, ainsi que la dernière version stable de Ruby. N'oubliez pas de redémarrer votre terminal pour que les changements soient effectifs.

#### Mise à Jour de RVM

Pour être certain de disposer de la dernière version de RVM, utilisez la commande suivante:

`rvm get stable`

#### Mise à jour de Ruby

Chercher quelle est la dernière version stable de Ruby disponible

`rvm list known`

Vous obtiendrez une longue liste de versions. Installez la dernière en date à l'aide de la commande suivante (remplacez la version par la dernière en date) et ... répondez "oui" à toutes les questions posées.

`rvm install 2.2.3`

Vous pouvez ensuite choisir quelle version de Ruby vous voulez utiliser:

`rvm list` et `rvm --default use 2.2.3`

### Installation de la gem Jekyll

Pour installer Jekyll, il vous suffit d'utiliser la commande suivante:

`gem install jekyll`

### Mise à jour de Jekyll

`gem update jekyll` vous permettra de mettre à jour votre version de Jekyll lorsqu'une nouvelle version est disponible.

## Utilisation

### Mise en place

Pour utiliser Jekyll, il vous suffit de créer un dossier dans votre environnement de développement local et d'utiliser la commande suivante:

`jekyll new mondossiersite/` ou `jekyll new .` pour installer Jekyll dans le dossier courant.

Jekyll devrait vous créer l'arborescence suivante (nous y reviendrons dans le détail):

- **_config.yaml**: fichier de configuration principal de votre site Jekyll
- **_includes**: contient vos includes
- **_layouts**: contient vos fichiers de layout
	- **default.html**: layout par défaut
	- **post.html**: layout utilisé pour vos posts
- **_posts**: dossier contenant vos blogposts
- **_sass**: contient vos fichiers .scss
- **css**: contient votre fichier .scss maître qui importe les autres
- **about.md**: contient votre page about
- **index.html**: la homepage de votre site
- **feed.xml**: un template de flux RSS

### Concepts de base et fonctionnement

Jekyll fonctionne en parcourant cette arborescence pour générer un site statique sur base des fichiers et dossiers trouvés. Ce site statique est généré dans le répertoire `_site` par défaut.

- Jekyll va traiter tous les fichiers contenant un YAML Front Matter (y compris un YAML Front Matter vide), les sauver temporairement et les rendre utilisables par Liquid et les tags Jekyll.
- les dossiers et fichiers dont le nom commence par un underscore ne seront pas transférés dans le dossier `_site`, tandis que les dossiers et fichiers dont le nom ne commence pas par un underscore seront transférés dans le dossier `_site`.

La commande `jekyll build` est utilisée pour générer votre site. Si vous souhaitez que ce site soit généré à nouveau automatiquement dès que Jekyll détecte des changements dans votre projet, utilisez la commande `jekyll build --watch`.

Depuis Jekyll 3.0.0, vous pouvez utiliser `jekyll build --incremental` pour régénérer votre site de façon incrémentale et gagner du temps, ainsi que `jekyll build --profiler` pour faire tourner un profiler vous montrant le temps de build pour chaque ressource.

### Configuration

Les [options de configuration globales](http://jekyllrb.com/docs/configuration/#global-configuration) de votre site sont stockées dans des variables définies dans le fichier `_config.yaml`.

L'ensemble des variables définies dans le fichier `config.yaml` sont accessibles dans vos templates via l'objet `site` en syntaxe pointée. Vous pouvez par exemple accéder à la variable `title` de votre `config.yaml` en utilisant `site.title`

Vous pouvez définir vos propres variables dans ce fichier. Elles seront alors accessibles dans vos templates via l'objet `site` en syntaxe pointée. Par exemple, si vous définissez une variable `test` dans le fichier `config.yaml`, vous pourrez y accéder dans vos templates en utilisant `site.test`.

Certaines de ces options de configuration sont implicites ([spécifiées par défaut](http://jekyllrb.com/docs/configuration/)) mais peuvent être surdéterminées si besoin est.

## Créer votre data structure

Jekyll vous propose différentes façons de créer vos données et de les structurer. Les trois outils principaux à votre disposition sont:

- **pages**: permettent de gérer contenus isolés, sans aucun lien logique avec d'autres (homepage, contact, etc.)
- **collections**: permettent de gérer des contenus liés entre eux de façon logique, faisant partie d'un même ensemble. Les fichiers membres d'une collection peuvent éventuellement générer un fichier propre (output: true) et également avoir une URL (permalink).
- **data**: permet de gérer des contenus structurés dans des formats tels que YAML ou JSON.

### Pages

[Les pages](http://jekyllrb.com/docs/pages/) sont de simples fichiers HTML ou Markdown. Vous pouvez utiliser leur YAML Front Matter pour créer vos propres variables ou pour utiliser les variables fournies par défaut par Jekyll.

Vous pouvez accéder à l'ensemble de ces variables via l'objet `page` en syntaxe pointée. Par exemple, vous pouvez accéder dans le corps de la page à une variable `test` contenue dans son YAML Front Matter en utilisant `page.test`.

### Collections

[Les collections](http://jekyllrb.com/docs/collections/) permettent de créer différents types de contenus liés entre eux de façon logique. Pour créer une collection, il suffit de créer un dossier nommé `_macollection` et puis de définir `macollection` dans votre fichier `config.yaml`. Le nom du dossier sans tenir compte du caractère `_` doit être identique au nom de la collection spécifié dans le fichier `_config.yaml`.

```yaml
collections:
	macollection:
```

Vous pouvez spécifier pour chaque collection si les fichiers qui la composent vont générer un output (un fichier propre) après traitement par Jekyll. Vous pouvez définir l'URL de ces fichiers via la variable `permarlink` (voir infra). Les fichiers d'une collection vont également générer automatiquement une variable `date` si vos noms de fichiers commençent par une date.

```yaml
collections:
	macollection:
		output: true
```

Des caractéristiques communes pour tous les fichiers d'une collection peuvent être définies via des [variables YAML par défaut](http://jekyllrb.com/docs/configuration/#Front Matter-defaults) (voir infra).

#### Posts: une collection particulière

[Les posts](http://jekyllrb.com/docs/posts/) viennent du fait que Jekyll a été conçu à la base comme un outil de blogging. Toutes les installations de Jekyll comprennent donc une collection par défaut nommée `_posts` qui est définie implicitement par Jekyll.

### YAML Front Matter et variables de page

L'ensemble des variables définies dans les YAML Front Matter sont accessibles dans vos templates via l'objet `page` en syntaxe pointée.

Certaines variables sont propres à jekyll et peuvent être utilisées pour l'ensemble de vos fichiers:

- **layout:** spécifie le layout à utiliser (voir la section consacrée au layout)
- **permalink:** spécifie le permalink et le chemin à utiliser pour l'output du fichier
- **published:** spécifie si le fichier doit être publié ou pas
- **category / categories:** spécifie les catégories à appliquer au fichier
- **tags:** spécifie les tags à appliquer au fichier

Vous pouvez également définir vos propres variables dans les YAML Front Matter de vos fichiers en utilisant les différents types de données disponibles en YAML: strings, lists, nested lists, etc.

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

Plutôt que de définir les caractéristiques communes des fichiers dans leur YAML Front Matters individuels, vous pouvez utiliser votre fichier `_config.yaml` et spécifier des [valeurs de YAML Front Matter par défaut](http://jekyllrb.com/docs/configuration/#front-matter-defaults). Via la variable `defaults:`, votre fichier de configuration vous permet de définir des valeurs par défaut pour des variables communes à un ensemble de fichiers. Ces ensembles sont définis soit par un `path` (obligatoire) et par un `type` (optionnel).

- **path**: chemin depuis la racine du projet. Une valeur pour path est obligatoire, même si vous utilisez un type. Une valeur vide permet de cibler l'ensemble des fichiers du site.
- **type**: type de fichier. Les types disponibles sont `pages`, `posts`, `nomcollection`

Vous pouvez ainsi spécifier facilement la valeur par défaut pour les variables `layout`, `category` ou `permalink` propres à Jekyll ou encore pour une variable de votre cru. Ces valeurs par défaut peuvent être surdéterminées par les valeurs dans le YAML Front Matter des fichiers individuels.

```yaml
defaults:
	scope:
		path: "/about"
	values:
		permalink: /blog/:year/:title
```

ou

```yaml
defaults:
	scope:
		path: "" # l'ensemble des fichers de votre projet
		type: "portfolio" # les fichiers de la collection portfolio
	values:
		permalink: /blog/:year/:title
		maVariable: maValeur
```

### Data

Jekyll permet également de définir des [fichiers de données](http://jekyllrb.com/docs/datafiles/). Le dossier `_data` permet de stocker des fichiers structurés en YAML, JSON ou CSV et rendre ces données disponibles pour Liquid et Jekyll. Ces données seront accessible via l'objet `site.data`. Par exemple, pour accéder au données du fichier `_data/mesdonnees.json` en liquid nous pouvons utiliser l'objet `site.data.mesdonnees`.

## Créer vos templates

Maintenant que nous avons vu comment créer vos données dans Jekyll, voyons comment utiliser ces données structurées dans vos templates.

### Liquid comme langage de templating

Jekyll utilise [Liquid](https://github.com/Shopify/liquid/wiki/Liquid-for-Designers) comme langage de templating. Liquid à été créé par [Shopify](https://www.shopify.com/) pour permettre à ses utilisateurs d'appliquer leurs propres layouts à leurs projets de e-commerce.

Liquid reste un langage très simple mais qui permet tout de même une grande souplesse d'utilisation. En plus des tags Liquid standards, Jekyll offre aussi quelques [filtres](http://jekyllrb.com/docs/templates/#filters) et [tags](http://jekyllrb.com/docs/templates/#tags) qui lui sont propres.

### Tags

Liquid comporte deux principaux types de tags:

1. Les tags d'affichage `{{ affichage }}` qui permettent d'écrire des variables dans vos templates.
2. Les tags de logique ou d'exécution `{% logique %}`. Par exemple `{% if %}`, `{% endif %}` ou `{% assign mavariable = site.macollection %}`

### Rester DRY: includes et layouts

#### Layouts

Jekyll vous permet de travailler à l'aide de fichiers de layout. Ceux-ci sont par défaut stockés dans le directory `_layout`. Un template enfant utilise un template parent et toutes les variables disponibles dans le template enfant le sont aussi dans le template de layout. La variable spéciale `{{ content }}` est remplacée dans le fichier de layout par le contenu du fichier enfant.

Pour utiliser un layout, il suffit de spécifier le nom de fichier du layout à utiliser dans le YAML Front Matter du fichier enfant. Jekyll ira chercher le fichier de layout dans le dossier `_layouts`. Vous pouvez modifier cela dans votre fichier `config.yaml` via la variable `layouts_dir:  ./_layouts`.

Voici à quoi cela ressemble:

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

Liquid et Jekyll vous permettent également d'utiliser des includes pour stocker les morceaux de code appelés à se répéter. Par défaut, Jekyll cherchera vos fichiers includes dans le répertoire `_includes`. Cela peut être modifié dans votre fichier `_config.yaml` via la variable `includes_dir: ./_includes`.

```liquid
{% include sidebar.html %}
```

Vous pouvez également passer des variables à vos includes.

```liquid
{% include sidebar.html searchWidget="true" %}
{% include sidebar.html searchWidget=page.search %}
```

Pour les récupérer dans le cadre de votre fichier include, il suffit d'utiliser `include.mavariable`

```liquid
{% if include.searchWidget == "true" %}
	... code du search widget ...
{% endif %}
```

### Loops et structures de contrôle

Liquid vous permet d'utiliser des boucles `for`, ce qui est particulièrement utile pour traiter vos objets et autres tableaux. Par exemple, pour afficher l'ensemble des `posts` de votre site, il suffit d'utiliser la syntaxe suivante:

```liquid
{% for item in site.posts %}
	{{ item.title }}
{% endfor %}
```

Si vous souhaitez vous limiter aux deux derniers posts, vous pouvez utiliser le paramètre `limit`.

```liquid
{% for item in site.posts limit:2 %}
 {{ item.title }}
{% endfor %}
```

Si vous souhaitez omettre les deux premiers posts de votre boucle, vous pouvez utiliser le paramètre `offset`.

```liquid
{% for item in site.posts offset:1 %}
 {{ item.title }}
{% endfor %}
```

Liquid vous permet également d'utiliser les structures de contrôle classiques telles que `[if](https://github.com/shopify/liquid/wiki/Liquid-for-Designers#if--else)` ou `[case](https://github.com/shopify/liquid/wiki/Liquid-for-Designers#case-statement)` dans le cadre de vos templates.

```liquid
{% if user.age > 18 %}
	<p>Would you like a beer ?</p>
{% else %}
	<p>We have orange juice, limonade, etc.</p>
{% endif %}
```

Couplées à une boucle `for` et aux [variables de loop](https://github.com/shopify/liquid/wiki/Liquid-for-Designers#for-loops), les structures de contrôle vous permettent d'avoir un code HTML propre.

```liquid
{% for item in site.posts %}
	{% if foorloop.first %}<ul>{% endif %}
		{{ item.title }}
	{% if foorloop.last %}</ul>{% endif %}
{% else %}
	<p>No blogposts found</p>
{% endfor %}
```

### Les tags `assign` et `capture`

Commençons par le tag `assign` qui vous permet simplement de créer une variable et d'y d'assigner une valeur.

```liquid
{% assign blogpostsPerTitle = site.posts | sort: 'title' %}

{% for item in blogpostsPerTitle reversed %}
	{% if foorloop.first %}<ul>{% endif %}
		{{ item.title }}
	{% if foorloop.last %}</ul>{% endif %}
{% else %}
	<p>No blogposts found</p>
{% endfor %}
```

Dans ce cas précis, combiner un classement alphabétique sur le titre et le parametre reversed n'est possible qu'en faisant les choses en deux étapes en utilisant `assign`. Voici également une autre application combinant plusieurs filtres et utilisant le paramètre `limit`. Grâce à `assign`, le code reste très lisible.

```liquid
{% assign blogpostsPerTitle = site.posts | sort: 'title' | reverse %}

{% for item in blogpostsPerTitle limit:2 %}
	{% if foorloop.first %}<ul>{% endif %}
		{{ item.title }}
	{% if foorloop.last %}</ul>{% endif %}
{% else %}
	<p>No blogposts found</p>
{% endfor %}
```

### Filtres: `sort`, `group_by` et `where`

Liquid possède [une série de filtres](https://github.com/shopify/liquid/wiki/Liquid-for-Designers#standard-filters) qui peuvent s'avérer forts utiles et dont la plupart servent à faire de la manipulation de chaînes de caractères.

Jekyl possède quant à lui quelques filtres qui lui sont propres. Parmi eux, trois sont essentiels au niveau de la manipulation de tableaux ou de hashes.

- `sort`: permet, comme nous venons de le voir, de trier un tableau ou un hash à l'aide d'une de ses valeurs.
- `group_by`: permet de grouper un tableau ou un hash par l'une de ses variables ou keys.
- `where`: permet de filtrer les éléments d'un tableau ou d'un hash à l'aide d'une de ses valeurs.

Voici quelques exemples d'applications pour les filtres `group_by` et `sort`:

`group_by` et des boucles imbriquées permettent par exemple de créer facilement une archive de posts par année. Il faut pour cela que chacun de vos posts possède une variable `publication_year` dans son YAML Front Matter.

```liquid
<h2>Yearly archive</h2>

{% assign postsByYears = site.posts | group_by: "publication_year" %}

{% for year in postsByYears %}
  <h3>{{ year.name }}</h3>
  {% for item in year.items %}
		{% if forloop.first %}<ul>{% endif %}
    	<li>{{ item.title }}</li>
		{% if forloop.last %}</ul>{% endif %}
  {% endfor %}
{% endfor %}
```

Le filtre `where` va permettre de filtrer les éléments d'un array, par exemple pour n'afficher que les posts d'une categorie bien définie. Pour que cela fonctionne, il faut évidemment qu'une catégorie soit définie dans chacun de vos posts, soit dans chaque YAML Front Matter, soit via les YAML Front Matter Defaults de votre fichier `_config.yaml`.

```liquid
<h2>Post in category</h2>

{% assign postsInCategory = site.posts | where: 'category','foo' %}

{% for item in postsInCategory %}
	{% if forloop.first %}<ul>{% endif %}
  	<li>{{ item.title }}</li>
	{% if forloop.last %}</ul>{% endif %}
{% endfor %}
```

### Data et fichier YAML

Comme dit plus haut, Jekyll vous permet de manipuler à l'aide e Liquid des fichiers de données structurées. Voici un exemple de navigation créée sur base d'un fichier YAML.

**nav**: *_data/nav.yaml*
```yaml
-
  navLabel: 'Home'
  navLink: '/'
-
  navLabel: 'Blog'
  navLink: '/blog/'
-
  navLabel: 'Portfolio'
  navLink: '/work/'
-
  navLabel: 'Contact'
  navLink: '/contact/'
```

**nav**: *_includes/mainnav.html*
```liquid
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

## Jekyll et Github pages

Jekyll est aussi le moteur de Github pages, un outil qui permet de gérer et d'héberger des sites statiques. Tous les repositories Github permettent de faire tourner Jekyll. La marche à suivre est différenets selon qu'il s'agit d'un projet ou d'une page de user ou d'organisation mais cela reste assez simple.

Le duo Jekyll et Github Pages vous permet de disposer d'un environnement collaboratif, d'un hébergement gratuit et d'un site entièrement gérer via Git.

## Ressources

- [Jekyll](http://jekyllrb.com/): le site officiel
- [Jekyll Talk](https://talk.jekyllrb.com/): le forum officiel pour poser vos question, discuter Jekyll, etc.
- [Installing Jekyll](http://davidensinger.com/2013/03/installing-jekyll/) - David Ensinger: tout ce dont vous avez besoin pour installer Jekyll sur un Mac
- [Liquid for Designers](https://github.com/Shopify/liquid/wiki/Liquid-for-Designers): une bonne introduction aux principaux tags et filtres de Liquid. N'oubliez pas de regarder également les [filtres](http://jekyllrb.com/docs/templates/#filters) et [tags](http://jekyllrb.com/docs/templates/#tags) propres à Jekyll.
- [Jekyll: much more than a static site generator](http://webstoemp.com/blog/jekyll-more-than-a-blog-generator/) - Jérôme Coupé: Un blogpost par votre serviteur sur les nouveautés dans Jekyll 2.0
- [Using Jekyll and GitHub Pages for Our Site](https://developmentseed.org/blog/2011/09/09/jekyll-github-pages/) - Young Hahn: sans doute l'une des meilleures introduction à Jekyll sur le plan de la philosophie de travail. Inclus également quelques perspectives techniques.
- [Jekyll and CMS-less websites with Young Hahn and Dave Cole](http://5by5.tv/webahead/54) (Podcast) - Jen Simmons, Young Hahn, Dave Cole: épisode du podcast "The Web Ahead" consacré à Jekyll et à d'autres file based CMS.
- [Intro to Jekyll](https://www.youtube.com/watch?v=O7NBEFmA7yA) (Video) - Johan Ronsse: une bonne introduction à Jekyll comme outil de prototypage.
- [Jekyll From Scratch - Getting Started](http://pixelcog.com/blog/2013/jekyll-from-scratch-introduction/), [Jekyll From Scratch - Core Architecture](http://pixelcog.com/blog/2013/jekyll-from-scratch-core-architecture/) et [Jekyll From Scratch - Extending Jekyll](http://pixelcog.com/blog/2013/jekyll-from-scratch-extending-jekyll/) - Mike Greiling: introduction très complète à tous les aspects de Jekyll.
- [Get Started With GitHub Pages (Plus Bonus Jekyll)](https://24ways.org/2013/get-started-with-github-pages/) - Anna Debenham: bonne introduction au duo Github pages et Jekyll
- [Build A Blog With Jekyll And GitHub Pages](http://www.smashingmagazine.com/2014/08/build-blog-jekyll-github-pages/) - Barry Clark: une  introduction centrée sur un blog.
- [Front-end style guides with Jekyll](http://webstoemp.com/blog/front-end-style-guides-jekyll/) - Jérôme Coupé: utilisation de Jekyll pour réaliser facilement des styles guides.

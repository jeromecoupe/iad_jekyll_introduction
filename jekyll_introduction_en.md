# Jekyll

## Introduction

[Jekyll](http://jekyllrb.com) is a static site generator developed in Ruby and created originally by [Tom Preston Werner](http://tom.preston-werner.com/). The source code is [available on Github](https://github.com/jekyll/jekyll).

Jekyll allows you to create websites based on dynamic templates (coded with [Liquid](http://liquidmarkup.org/)) and content files written in (YAML / Markdown / HTML / JSON / CSV). There is no database involved. Based on content and template files, Jekyll will generate and entirely static website that can then be deployed on any type of hosting or on [Github Pages](https://pages.github.com/).

The most common workflow is to generate your site locally and to publish the generated static files. The main advantages of a static site generator can be summarized as follows:

- Everything is file based so your entire website can be full version controlled with tools like Git.
- Simple static files are incredibly easy to host and make for fast and performant websites
- Your infrastructure remains simple, cheap and offers less opportunities to hackers

## Installation

To install Jekyll, you must first install Ruby. Here is how I do it.

### Installing Ruby

Ruby is installed by default on OSX. Nonetheless, here is the process I follow to install Ruby or update the version installed on my machine.

If you are running Windows, your best bet is probably [Ruby Installer](http://rubyinstaller.org). After installing it, just install the Jekyll gem like specified below.

#### Install Homebrew

Howebrew is a tool allowing you to easily install unix packages on your mac.

`ruby -e "$(curl -fsSL https://raw.github.com/mxcl/homebrew/go)"`

After that, just run a small `brew doctor` to check everything is in working order. If you have a path problem, [here is a quick solution](http://stackoverflow.com/questions/10343834/homebrew-wants-me-to-amend-my-path-no-clue-how).

#### Install RVM

The next step is to install Ruby Version Manager (RVM) that will allow you to easily manage various versions of Ruby on your machine, which can come in handy (trust me on that).

`\curl -#L https://get.rvm.io | bash -s stable --ruby`

That command will install the latest stable version of RVM as well as the latest stable version of Ruby. Don't forget to restart your terminal for the changes to take effect.

#### Update RVM

Down the road, you can get the latest stable version of RVM by simply typing

`rvm get stable`

#### Update Ruby

Look for the latest stable version of Ruby:

`rvm list known`

That will get you a big long list of versions. Install any version using the following command (replace the version by the one you need) and ... answser "yes" to all the question RVM asks you.

`rvm install 2.2.3`

You can then choose the version of Ruby you want to use amongst the ones RVM has availableon your machine:

`rvm list` and `rvm --default use 2.2.3`

### Install the Jekyll gem

To install Jekyll, you just have to run the following command:

`gem install jekyll`

### Update Jekyll

`gem update jekyll` will allow you to update your verison of Jekyll when a new one is available.

## Usage

### Initialise a Jekyll project

To create a Jekyll project, you just have to create a folder in your local dev environment and use the following command:

`jekyll new myprojectfolder/` or `jekyll new .` to install jekyll in the folder you are currently in.

Jekyll will create the following folders and files for you. We will come back to this in detail later on:

- **_config.yaml**: main configuration file for your Jekyll site
- **_includes**: contains your include files
- **_layouts**: contains your layout files
  - **default.html**: default layout
  - **post.html**: layout used by blogposts
- **_posts**: contains all your blogposts
- **_sass**: contains you .scss files
- **css**: contains your master .scss file importing all the others
- **about.md**: the template of your about page
- **index.html**: the template for the homepage of your site
- **feed.xml**: the template of your RSS feed

### Basic concepts and commands

Jekyll will use these files and folders as a basis to create a fully static website. This static wbeiste is generated in the `_site` folder by default.

- Jekyll will process all files with a YAML Front Matter (including empty YAML Front Matters), amke them available in memory and make them usable by the Liquid templating engine and Jekyll tags.
- Files and folders with a name starting with underscore will not be transfered to the `_site` folder, whicle the others will be after having been processed by the Jekyll pipeline.

When in your project folder, the `jekyll build` command is used to generate your website. If you want the regeneration to happen every time a file is changed, just use the `--watch` flag: `jekyll build --watch`.

You can also use `jekyll serve` to launch a developement server and vie your site at `http://localhost:4000/` (auto-regenaration is on by default).

Since Jekyll 3.0.0 you can use `jekyll build --incremental` to regenerate only the parts of the site that changed instead of the hole thing each time, a big deal for huge websites. You can also use `jekyll build --profiler` and Jekyll will give you a build time for each of your resources to identify possible bottlenecks.

### Configuration

[Global configuration options](http://jekyllrb.com/docs/configuration/#global-configuration) for your site are stored in the variables of the aptly named `_config.yaml` file at the root of your project.

Variables defined in `_config.yaml` can be accessed from your template using the `site` object and dot notation. For example, you can access the `title` variable in your `_config.yaml` file by using `site.title`. You can define your own custom variables in that file. Custom variables are accessed in the same way: you can access your custom `test` variable by using `site.test`.

Several of those global configuration variables are implicit ([their values are specified by default](http://jekyllrb.com/docs/configuration/)) but they can all be overriden if needs be.

## Create your data structure

Jekyll offers you various ways of creating structured data to use in your project. The main three tools at your disposal are:

- **pages**: allow you to create and manage on-off pages having no logical relationships with other items in your site (hompage, contact page, etc).
- **collections**: allow you to create pieces of content that are logically to each other through a series of (html or markdown) files . A collection might be configured to generate separate files for each document in the collection when the site is built via the `output` variable. The URL structure and path for each generated file can be managed via the `permalink` variable (more on those later). As of Jekyll 3.0.0 posts is a collection that has a bit of a special status.
- **data**: allow you to create and manage data through structured data format like JSON, CSV or YAML. These pieces of data will not generate discrete files with distinct URLs.

### Pages

[Pages](http://jekyllrb.com/docs/pages/) are simply HTML or Markdown files. You can use their YAML Front Matter to create your own variables or to use the default variables made available by Jekyll.

You can access all these variables through the `page` object in dot notation. For example, a `test` variable in the YAML Front Matter of a page can be accessed using `page.test`.

### Collections

[Collections](http://jekyllrb.com/docs/collections/) allow you to create logically related data items using a series of HTML or Markdown files. To create a collection, you will need to create a `_mycollection` folder at the root of your project and then to define `mycollection` in your `_config.yaml` file. The name of the folder without the underscore has to match the name of the collection defined in `_config.yaml`.

Given a `_mycollection` folder, the following would define the collection in your `_config.yaml` file:

```yaml
collections:
  mycollection:
```

For each defined collection, you can specify if you want every file in the collection to generate a separate HTML file as an output after Jekyll has done its thing. You can define the URL structure of those files via the `permalink` variable. Each file in a collection will also automatically generate a `date` variable if your filenames start with a date.

```yaml
collections:
  mycollection:
    output: true
    permalink: /collection/:year/:title
```

Common characteristics and variables for all documents in a collection can be defined via [YAML Front Matter Defaults](http://jekyllrb.com/docs/configuration/#Front Matter-defaults) (see infra for detail).

#### Posts: a collection with a special status

[Posts](http://jekyllrb.com/docs/posts/) harken back to the early days of Jekyll, when it was created as a blogging platform. All Jekyll installations now have a default `_posts` that Jekyll defines implicitely.

The `posts` collection has a special status in Jekyll and has functionalities that are not yet available to regular collections. A prime example would be the category and tag management available for posts.

### YAML Front Matter variables

As established earlier variables defined in the YAML Front Matters of your files (collections, posts or pages) can be accessed via the `page` object.

Some variables are specific to Jekyll and can be used in all your files:

- **layout:** specifies the layout to be used with that page or collection document (cf infra)
- **permalink:** specifies the permalink and the path structure for the output file generated by a page or collection document
- **published:** specifies the publish status of the page (can be true or false)
- **category / categories:** spécifies the categories that should be applied to the page or collection document
- **tags:** spécifies the tags that should be applied to the page or collection document

You can also define your own variables in the YAML Front Matter of your files and are allowed to use the different variable types available through YAML: strings, lists, arrays, nested lists, etc.

Example: YAML Front Matter and Markdown file (*myfile.md*)
```
---
title: my title
summary: this is a summary
myVariable: test
myList:
  - item 1
  - item 2
  - item 3
---

## Second level title

Lorem ipsum dolor sit amet, consectetur adipisicing elit. Cumque nobis perspiciatis eligendi nesciunt vel ea sunt illo deserunt earum id quaerat, odit, enim! Earum corporis, numquam iure facilis cumque ratione.
```

#### YAML Front Matter defaults

Rather than defining common variables for files in their individual YAML Front Matters, you can configure [YAML Front Matter defaults](http://jekyllrb.com/docs/configuration/#front-matter-defaults) through your `_config.yaml` file. Via the `default` variable, you can define default variables and common values for groups of files. These groups are defined using their `path` (mandatory) and their `type` (optionnal).

- **path**: path from the root of the project. A `path` value is mandatory, even if you are using a `type` as well. An empty value will target all files in your project.
- **type**: target a file type. Available types are `pages`, `posts` and `collectionname`.

For example, you can specify common values for Jekyll-specific variables like `layout`, `category` or `permalink` or for custom variables you create. These default values can be overridden by values in the YAML Front Matter of each individual files.

```yaml
defaults:
  scope:
    path: "/about"
  values:
    layout: "default"
    myVariable: myValue
```

or

```yaml
defaults:
  scope:
    path: "" # all files in your project
    type: "portfolio" # files in the "portfolio" collection
  values:
    layout: "default"
    myVariable: myValue
```

### Data

Jekyll also allows you to define and use [data files](http://jekyllrb.com/docs/datafiles/). The `_data` folder will let you store structured data files in YAML, JSON or CSV and make their data available to Liquid and Jekyll. The data in these files will be accessible through the `site.data` object. For example, to access the data in the `_data/mydata.json` with Liquid, we would use the `site.data.mydata` object.

## Create your templates

Now that we have seen how to create the data structure of your project with Jekyll, let's see how to use these data in your templates.

### Liquid as a templating language

Jekyll uses [Liquid](https://github.com/Shopify/liquid/wiki/Liquid-for-Designers), a templating language created by [Shopify](https://www.shopify.com/), to allow its users to create and apply their own templates to their e-commerce projects.

Liquid is a very simple templating language but one that can be used in very flexible ways. On top of the standard Liquid tags and filters, Jekyll adds some extra [filters](http://jekyllrb.com/docs/templates/#filters) and [tags](http://jekyllrb.com/docs/templates/#tags) of its own.

### Two types of Liquid tags

There are two main types of tags in Liquid:

1. Output tags `{{ output }}` allowing you to output variables in your templates.
2. Logical or execution tags `{% logic %}`. For example `{% if %}`, `{% endif %}` or `{% assign mavariable = site.macollection %}` all allow you execute actions or add logic to your templates.

### Access your data

When it runs, Jekyll makes a bunch of variables available to you via the Liquid templating system.

#### Global variables

- `site`: Mainly used to access arrays of your pages, posts and collection documents. Also used to access configuration variables set in your `_config.yml`. See below for details.
- `page`: Used to access specific variables specified via the YAML Front Matter or via YAML Front Matter Default. See below for details.
- `content`: Special variable used in layout files, placeholder for the rendered content of the post, page or collection document being wrapped. Not defined in Post or Page files.

#### Variables related to collections, pages and data

`site.collectionName` will return an array of all items belonging to a specific collection. `posts` being nothing else than a collection that Jekyll creates by default, you can access all posts using `site.posts`. For example, if you defined a `projects` collection, you would loop over your projects using the following code:

```Liquid
{% for project in site.projects %}
  <h2>{{ project.title }}</h2>
{% else %}
  <p>Sorry, I cannot find any project</p>
{% endfor %}
```

As you can see, variables defined either via individual YAML Front Matter or via YAML Front Matter defaults can be accessed using dot notation in for loops.

```Liquid
{% for project in site.projects %}
  <h2><a href="{{ project.url }}">{{ project.title }}</a></h2>
  <p>{{ project.summary }}</p>
{% else %}
  <p>Sorry, I cannot find any project</p>
{% endfor %}
```

The `summary` variable defined in the YAML Front Matter of all projects can be accessed using `project.summary`. The `project.url` variable is automatically created by Jekyll based on the permalink pattern defined for items in the projects collection, either via their individual YAML Front Matter or via YAML Front Matter defaults in the `_config.yaml` file.

Jekyll will also give you access to other global variables related to your pages, posts and collections. Here are the ones you will probably be using the most:

- `site.pages`: gives you an array of all your pages
- `site.collections`: gives you an array of all your collections and of [their attributes](http://jekyllrb.com/docs/collections/#collections)
- `site.data`: an array containing the data loaded from the data files located in the `_data` directory. To access the data in an individual file, simply use `site.data.filename`.
- `site.categories.CATEGORY`: an array of all posts in the CATEGORY categories. `site.categories.work` will get you an array of all posts in the "work" category. This only works for posts, not for items in any other collections.
- `site.tags.TAG`: an array of all posts to which the TAG tag is applied. `site.tags.jekyll` will get you an array of all posts to which the "jekyll" tag has been applied. This only works for posts, not for items in any other collections.

#### Variables in pages

As mentioned earlier, custom variables defined via individual YAML Front Matter or via YAML Front Matter defaults can be accessed using dot notation on the `page` variable. So, if a `test` variable is defined in the YAML Front Matter, you can access it in the body of that file using `page.test`. Those custom variables will also be available in any layout file that page references as well.

On top of the variables you create, [Jekyll automatically creates some variables](http://jekyllrb.com/docs/variables/#page-variables) for your posts, pages and collection documents and makes them available to Liquid. Here are the main ones:

- `page.title`: the post or page title
- `page.content`: the content of the page. The part of the page the special `{{ content }}` variable will display in a layout.
- `page.date`: the date assigned to the collection document. Can either be set using the file name or using, YAML Front Matter or YAML Front Matter Default variables.
- `page.categories`: the list of the categories assigned to the post. This only works for posts, not for items in any other collections.
- `page.tags`: the list of the tags assigned to the post. This only works for posts, not for items in any other collections.

#### Configuration variables

The `site` variable can also be used to access the value of any variable defined in your `_config.yaml` file. If you have defined a `test` variable in your `_config.yaml` file, you can access it using `site.test`.  

### Stay DRY: includes et layouts

#### Layouts

Jekyll allows you to work with layout files. By default, these are stored in the `_layout` directory in your project. A child template will use or extend a parent template. All variables available in the child template will automatically be available to the parent template. The special `{{ content }}` variable is replaced in the parent template by the content of the child template.

To use a layout, you just have to specify the layout to be used in the YAML Front Matter of the child template using the `layout` variable. Jekyll will go and fetch the specified template in the `_layout` directory. You can change the directory where layouts are stored via the `layouts_dir:  ./_layouts` in your `_config.yaml` file.

Here is what using a layout looks like:

**child template**: *index.html*
```
---
layout: default
title: My first Jekyll file
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

Nested layouts can come in handy sometimes. If you need a particular layout for a certain type of item to extend your base layout, you could work in the following way:

**child template**: *_posts/2015-12-10-my-first-blogpost.md*
```
---
layout: blogpost
title: The title of my blogpost
---

Content of my blogpost
```

**parent template 1**: *_layouts/blogpost.html*
```
---
layout: default
---
<h1>{{ page.title }}</h1>
{{ content }}
```

**parent template 2**: *_layouts/default.html*
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

As said earlier, any variable defined in the child template will be available in the layout file.

#### Includes

Liquid and Jekyll also let you use includes to store bits of code and use them repeatedly. By default, Jekyll will look for your includes files in the `_includes` directory. That behaviour can be changed using your `_config.yaml` file and the `includes_dir: ./_includes` configuration variable.

```Liquid
{% include sidebar.html %}
```

You can also pass variable to included files:

```Liquid
{% include sidebar.html searchWidget=true %}
{% include sidebar.html searchWidget=page.search %}
```

To use these variables in the context of your include file, you just have to use `include.mavariable`.

```Liquid
{% if include.searchWidget == true %}
  ... code of the search widget ...
{% endif %}
```

### Loops and control structures

Liquid allows you to use `for` loops, which come in handy to loop through your arrays and hashes. For example, to display the title of all the `_posts` in your site, you could use the following `for` loop.

```Liquid
{% for item in site.posts %}
  {{ item.title }}
{% endfor %}
```

If you want to just display the last couple of posts, you have to use the `limit` parameter.

```Liquid
{% for item in site.posts limit:2 %}
 {{ item.title }}
{% endfor %}
```

If you want to omit the first two posts, you can use the `offset` parameter.

```Liquid
{% for item in site.posts offset:2 %}
 {{ item.title }}
{% endfor %}
```

Liquid also allows you to use common control structures like [`if`](https://github.com/shopify/liquid/wiki/Liquid-for-Designers#if--else) or [`case`](https://github.com/shopify/liquid/wiki/Liquid-for-Designers#case-statement) in your templates.

```Liquid
{% if user.age > 18 %}
  <p>Would you like a beer ?</p>
{% else %}
  <p>We have orange juice, limonade, etc.</p>
{% endif %}
```

Coupled to a `for` loop and to [loop variables](https://github.com/shopify/liquid/wiki/Liquid-for-Designers#for-loops), control structures allow you to output clean HTML code in all situations.

```Liquid
{% for item in site.posts %}
  {% if foorloop.first %}<ul>{% endif %}
    <li>{{ item.title }}</li>
  {% if foorloop.last %}</ul>{% endif %}
{% else %}
  <p>No blogposts found</p>
{% endfor %}
```

### `assign` and `capture` tags

Let's start with `assign`, a tag allowing you to simply create a variable and assign a value to it.

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

In this case, combining an alphabetical sorting on the title and the `reversed` parameter is only made possible by dividing the process in two distinct steps using `assign`. Here is another example combining various filters and using the `limit` parameter. Thanks to `assign` the code remains very legible.

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

As the name suggests, `capture` allows you to capture various strings and to store them in a variable.

```Liquid
{% capture fullName %}{{ item.name | capitalize }} {{ item.surname | capitalize }}{% endcapture %}
```

That functionality can be very useful in certain situations, for example when you have to create a yearly archive or your posts:

```Liquid
{% assign allPosts = site.posts | sort:"post.date" %}
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

### Filters: `sort`, `group_by` et `where`

Liquid makes [a series of filters](https://github.com/shopify/liquid/wiki/Liquid-for-Designers#standard-filters) available to you. Filters will help you to modify the output of strings, numbers, objects and arrays in various ways.

Jekyll also has a few filters of its own. Among them, three are really interesting to manipulate arrays or hashes.

- `sort`: allows you to sort and array or hash by one of its properties.
- `group_by`: allows you to group an array of a hash by one of its properties.
- `where`: allows you to filter the elements of an array or a hash by one of its properties.

Here are some examples for the `group_by` and `sort` filters:

`group_by` and nested for loops will for example allow you to easily group your posts by one of their properties. For example, if each of your posts has a `postType` variable in their YAML Front Matter, you can easily create an archive of your posts grouped per type.

```Liquid
<h2>Archive per type</h2>

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

The `where` filter will allow you to filter all elements of an array. An example would be to only display the posts by one specific author. In order for this code to work, you obviously need an author to be defined for your posts, either in the YAML Front Matters of your posts, or via YAML Front Matter Defaults in your `_config.yaml`.

```Liquid
<h2>Posts by author</h2>

{% assign postsByAuthor = site.posts | where:"author","Gengis Khan" %}

{% for item in postsByAuthor %}
  {% if forloop.first %}<ul>{% endif %}
    <li>{{ item.title }}</li>
  {% if forloop.last %}</ul>{% endif %}
{% endfor %}
```

### Data and YAML, JSON and CSV files

As we said earlier, Jekyll allows you to easily manipulate data files written in YAML, JSON or CSV using Liquid. Here is an example with a navigation created on the basis of a YAML file. The `currentNav` variable would be specified either through the YAML Front Matter of each file, or via YAML Front Matter Defaults in your `_config.yaml`.

**YAML**: *_data/nav.yaml*
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

**HTML**: *_includes/mainnav.html*
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

## Jekyll and Github pages

Jekyll is the engine powering [Github Pages](https://pages.github.com/), a tool allowing you to manage and host Jekyll site for free. All Github repositories allow you to use Jekyll. The setup is a little bit [différent for projects than for users or organisations](https://help.github.com/articles/using-jekyll-with-pages/) but it remains quite simple.

By combining Jekyll and Github pages, you will get yourself a collaborative environment, free hosting and a website fully managed with Git.

## Ressources

- [Jekyll](http://jekyllrb.com/): le official website
- [Jekyll Talk](https://talk.jekyllrb.com/): the official forum for questions and discuss enerything Jekyll.
- [Installing Jekyll](http://davidensinger.com/2013/03/installing-jekyll/) - David Ensinger: everything you need to know to install Jekyll on a Mac
- [Liquid for Designers](https://github.com/Shopify/liquid/wiki/Liquid-for-Designers): a good introduction to the main Liquid tags and filters. Don't forget to also have a look at Jekyll's own [filters](http://jekyllrb.com/docs/templates/#filters) and [tags](http://jekyllrb.com/docs/templates/#tags).
- [Jekyll: much more than a static site generator](http://webstoemp.com/blog/jekyll-more-than-a-blog-generator/) - Jérôme Coupé: A blogpost on what's new in Jekyll 2.0
- [Using Jekyll and GitHub Pages for Our Site](https://developmentseed.org/blog/2011/09/09/jekyll-github-pages/) - Young Hahn: one of the best introduction to Jekyll and static sites generators. Also includes some interesting technical tidbits.
- [Jekyll and CMS-less websites with Young Hahn and Dave Cole](http://5by5.tv/webahead/54) (Podcast) - Jen Simmons, Young Hahn, Dave Cole: an episode of "The Web Ahead" dedicated to Jekyll and flat files CMS.
- [Intro to Jekyll](https://www.youtube.com/watch?v=O7NBEFmA7yA) (Video) - Johan Ronsse: a good introduction to Jekyll as prototyping tool
- [Jekyll From Scratch - Getting Started](http://pixelcog.com/blog/2013/jekyll-from-scratch-introduction/), [Jekyll From Scratch - Core Architecture](http://pixelcog.com/blog/2013/jekyll-from-scratch-core-architecture/) and [Jekyll From Scratch - Extending Jekyll](http://pixelcog.com/blog/2013/jekyll-from-scratch-extending-jekyll/) - Mike Greiling: very good intooduction to all aspects of Jelyll.
- [Get Started With GitHub Pages (Plus Bonus Jekyll)](https://24ways.org/2013/get-started-with-github-pages/) - Anna Debenham: an introduction to Github Pages and Jekyll
- [Build A Blog With Jekyll And GitHub Pages](http://www.smashingmagazine.com/2014/08/build-blog-jekyll-github-pages/) - Barry Clark: tutorial on how to build a blogs.
- [Front-end style guides with Jekyll](http://webstoemp.com/blog/front-end-style-guides-jekyll/) - Jérôme Coupé: using Jekyll to create style guides.
- [Static Sites Go All Hollywood](https://vimeo.com/145138875) - Phil Hawksworth: a good introductions to the advantages and inconvenients of using static sites generators. A nice bit on modern workflows and infrastructures, too.

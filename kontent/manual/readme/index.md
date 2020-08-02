---
title: "README"
date: 2020-07-16T20:19:42+02:00
description: All the details you need to know
draft: true
toc: true
---

## Introduction to Multilingual Errorist

Errorist is a website theme designed originally to support a GitHub based workflow for foreign
language students. Specifically, the breaknecks who do the homework on GitHub and wish to include
their grammar exercises with teacher's corrections into their blog – hence the name.

That being said, you can build a 'normal' multilingual, multi-authored responsive website with
Errorist.

Efficient multilingual content management is Errorist's top priority. Images, for instance, are
tightly coupled with their (multilingual) captions and authors using image bundles. Adding the main
image to a page (that is, all translations) is a matter of a single(!) edit.

Keeping code noise in Markdown to the minimum is another priority. This is achieved by using
page bundles combined with certain file naming conventions.

Visually, Errorist as a theme is text-only – not a single graphic file is included. This way,
your site will be defined by your content entirely.

This document assumes that [Hugo](https://gohugo.io/documentation) static website 
generator is already installed on your computer. A soft step-by-step tutorial is supplied with the
bootstrap project and becomes available once you clone it to your computer, run `hugo server -D`,
and open `http://localhost:1313` page in your browser. You can also read it at GitHub website.

## Installation

If you are starting a new project, fork the bootstrapping project on GitHub, and then clone it, recursively.
The Errorist theme will be pulled as a git submodule. This way, you will only need to do a few edits
in configuration to get started. Forking is important, since this will become _your project_, and you 
will likely want to keep it on GitHub as such.

So, fork
[https://github.com/tvendelin/errorist-bootstrap](https://github.com/tvendelin/errorist-bootstrap)
first, and then

```
git clone --recurse-submodules <YOUR FORK OF tvendelin/errorist-bootstrap> 
```

The bootstrapping project comes pre-configured with two languages, English and Russian. Russian has
been chosen for the sake of contrasting example for multilingual features. Adding and removing
languages is trivial and is described in a section of its own further down in this document. 

If you want to use Errorist for an existing project, you would need to merge the example
configuration from `errorist/exampleSite` directory with yours. To clone just the Errorist theme,

```
git clone git@github.com:tvendelin/errorist.git themes/errorist
```

Your content structure might also require some adjustments (see the next section).

## Adding Text Content

Errorist's key features rely on a particular page bundle structure. Here's an example with some
bells and whistles:

```
kontent/test/lesson-learned
├── homework
│   ├── 10_task.txt
│   ├── 20_mysolution.txt
│   ├── 30_solution.txt
│   └── 40_diff.txt
├── index.md
├── index.ru.md
├── main-img
├── summary.md
└── summary.ru.md
```

The English  and Russian pages are in `index*` files – it's just a usual page bundle. 

The sub-directory `homework` contains four text files. These text files hold content for a block of
clickable tabs, identified by directory name (`homework`). Such tab can be included in a page using
a shortcode `{{</*tabs_bq "homework"*/>}}`. Should files contain any Markdown, it will be rendered.
For language-sensitive tabs, `*.md` files should be used instead. See the [Shortcodes]({{< ref
"#shortcodes-for-clickable-tabs" >}}) section for details.

The `summary.*md` files, when present, override the built-in `.Summary` page variable. This gives
you the freedom to write your summaries any way you want. Note that a good summary isn't necessarily a
continuous fragment of a page.

Multiple authorship is supported, as well as localized authors' names. 

The file `main-img` (the name is significant) contains a reference to a page bundle with the main
image for the page with captions in all supported languages and the authors' IDs. Switching the main
image for a multilingual page is thus a matter of editing a single line in a
single file.

All of the above offloads quite a lot of noise from your Markup and makes your content better
manageable. 

Errorist will work with other supported content setups as far as the basic functionality
provided by Hugo is concerned.

Notice, that the content directory is pre-configured as `kontent` instead of the default `content`.
This will save you quite a lot of keystrokes on command line with tab-completion (`k<TAB>` instead
of `cont<TAB>`).

## Image Management with Image Bundles

An 'image bundle' (a term specific to Errorist) is, in effect, a page bundle, stored under a
particular content section, `kontent/images` by default. It includes _a single_ image file and
index.*md files containing associated texts for each supported language:

```
kontent/images/myimage
├── anyname.jpg
├── index.de.md
├── index.md
└── index.ru.md
```

The name of the image
file isn't significant as long as Hugo recognizes its type as 'image'. An image bundle is identified
by its directory name, `myimage` in our example.

The only front matter parameter specific to image bundle is `alt` which holds the same-named
attribute for the `<img>` HTML tag. The caption for the image is the `.Content` of an `index.*md`
file for the current `.Language`. The `alt` value would also fall back to `.Content` if omitted:

```
---
alt: alt text for the image goes here
authors:
- tatkins
---
Image caption goes here
```

An archetype for creating an image bundle is provided. You should adjust it according to the
languages used in your website ([more on this in the following sections]({{< ref "#localization"
>}})). To add an image bundle, run

```
hugo new images/myimage -k img
```

and copy an image that you have in mind into just created image bundle directory
(`kontent/images/myimage/`).

From now on, the image can be included into one or more pages by reference to its image bundle. All
the attributes will follow it like ducklings follow their mother-duck, guarding you against the
embarrassments of mismatching image/caption/author.

### Image Bundle Directory

The directory holding image bundles (`kontent/images` by default) is defined by `.Site.Params.image_bundle_dir`
configuration parameter (see `config/_default/params.yaml`). It must be relative to `kontent`, and its
name should not begin with underscore, if you host your site on GitHub. By default, it is neither
rendered nor listed when the site is built, which is handled by `kontent/images/_index.*md` files'
front matter:

```yaml
---
draft: false
cascade:
    _build:
        list: never
        render: false
---
```

If this is the desired behavior, add a `_index.*md` file like the one above for each language that
you add.  If you want the captioned images to be displayed as content in their own right – for
example, listing them on author's page – remove cascading build configuration and create a normal
`_index.md` file instead.

Nothing prevents you, of course, from copying loose images into `static` directory or a page
bundle directory. The usual Markdown syntax will work.

## Including Images in Pages

### Image Representative of Page

```
echo 'myimage' >> kontent/section/pageBundle/main-img
```

This will make the image under `kontent/images/myimage` the 'main' image for the page, and mark it
as 'representative of page' in terms of meta- and microdata. For instance, it will be the image
people see when you share the page on a social network. 

### Images in Page Content

Use the standard Markdown syntax of `![alt](src "caption")`. If `src` does not contain a dot, an
image bundle would be looked up. Markdown texts override caption and `alt` from image bundle:

- `![](myimage)` without 'extension', the image bundle will be looked up with its caption and `alt`
text.

- `![custom alt](myimage "caption")` same as above, but with caption and `alt` text overridden by
  Markdown.

- `![custom alt](/img/simple.jpg "caption")` with 'extension', the image will be looked up as
`static/img/simple.jpg`

- `![custom alt](simple.jpg "caption")`  the image will be looked up relative to the page bundle.

### The Default Site Image for Social Networks

This is the first element in `images:` list in `config/_default/params.yaml`

## Shortcodes for Clickable Tabs

### `tabs_bq`

Usage:

```
{{</*tabs_bq "homework" "txt" */>}}
```

Creates a block of clickable tabs from contents of `*.txt` files found in `homework` directory
within a page bundle:

```
homework
├── 10_task.txt
├── 20_mysolution.txt
├── 30_solution.txt
└── 40_diff.txt
```

The tabs will be listed alphabetically, the first tab always being active when
the page loads. The tab files can be prepended with digits and an optional underscore to define the
ordering. This prefix as well as `.txt` suffix will be stripped when determining the tab's
identifier, so `10_task.txt` will be identified as `task`. The localized tab name will be looked up
in a `i18n/*.yaml` localization files. If not found, the tab name will default to its identifier.

The Markup in the contents of tab files, if any, will be interpreted. This allows to include
language-agnostic tab content, as well as content in another language, that is supposed to stay the
same regardless of the language of the page. The contents of a tab will be placed within a pair of
HTML `<blockquote>` tags, and special styling will be applied to `<del>` and `<strong>` HTML tags to
highlight corrections. 

The practical use of it is including grammar exercises in a foreign language. Having tab contents in
separate file makes piping from an external program (like git, wdiff, etc.) is particularly
convenient.

For language-sensitive content, use "md" as the second argument. The default is "txt", so it can be
omitted.

### `tabs`

Usage:

```
{{</*tabs "homework" "txt" */>}}
```

Same as `tabs_bq` (see above), but tabs will be wrapped in `<div>` tags, and the appearance of
`<del>` and `<strong>` HTML tags will be the default one.

## Configure Languages that Your Site Supports

Edit the `config/_default/languages.yaml` file. The example configuration includes two languages,
English and Russian, already pre-configured (add or delete as necessary):

```yaml
en:
    languageName: English
    weight: 10
    locale: en_US
ru:
    languageName: Русский
    weight: 20
    locale: ru_RU
```

The keys (`en`, `ru` in the example) should be ISO-639 language codes, as these will be used in metadata.

- `languageName:` the name of a language in that language (see example for Russian)

- `weight:` a decimal number used for ordering languages in a list.
Lower weight = higher position. Use gapped numbering for easier inserts
(i.e. 10, 20, 30, ... ) as opposed to (1, 2, 3, ...)

- `locale:` a combination of ISO-639 language code and ISO-3166 country code
joined with underscore. Currently used only in `og:locale` meta tag.

### Add or Remove the Localization (Translations)

The translations can be found under `i18n` directory in the theme root:

```
themes/errorist/i18n/
├── en.yaml
└── ru.yaml
```

The file names correspond to the YAML keys in `config/_default/languages.yaml` file. 
To add string translations for another language you are going to use, create an `i18n` directory
under your project root, and create additional translations files for the missing languages there
(not in the theme!), naming
the files accordingly. So, if you happen to add Estonian, you should create `i18n/ee.yaml` in your
project.

```
cp themes/errorist/i18n/en.yaml i18n/ee.yaml
# edit i18n/ee.yaml
```

If you are going to add localized strings in your project (author names, clickable tabs, etc.), you
will need to create translation files even for the languages supported by the theme. The
translations in the theme and in your project will be merged, and your translation will win, should
there be any conflict.

You will also need to adjust your archetypes (see the next section).

## Archetypes

Errorist makes a good use of so-called page bundles. To make a good of use of Errorist, a few
archetypes are provided. If your site runs in one single language only, you can safely [skip the next
section]({{< ref "#usage" >}}).

### Localization

Although the supplied set of supported languages will strike a cord with substantial population 
on substantial territory, even more users will need something different. To override the supplied
archetypes, copy them to your project directory first:

```
cp -R themes/errorist/archetypes ./
```

Examine the `archetypes` directory:

```
archetypes/
├── default.md
├── img
│   ├── index.md
│   └── index.ru.md
├── langblog
│   ├── homework
│   │   ├── 10_task.txt
│   │   ├── 20_mysolution.txt
│   │   ├── 30_solution.txt
│   │   └── 40_diff.txt
│   ├── index.md
│   └── index.ru.md
└── page
    ├── index.md
    └── index.ru.md
```

For each additional language your website uses, you will need an `index.<LANG>.md` file, where
`<LANG>` is the language key from `config/_default/languages.yaml` file. Since the supplied
`index.md` files are language-independent, you just need to copy them, adjusting the file name.

Finally, if you are not going to use Russian, you should remove the corresponding files from
archetypes:

```
find archetypes -name 'index.ru.md' -exec rm '{}' \;
```

### Usage

```
hugo new -k page blog/mypost
```

creates a page bundle `kontent/blog/mypost`:

```
kontent/blog/mypost
├── index.de.md
├── index.md
└── index.ru.md
```

```
hugo new -k img images/mypicture
```

creates an image bundle similar to page, but with different front matter parameters. You will need
to copy an image file, of course, to make a use of it.

```
hugo new -k langblog learning-indonesian/pronouns
```

creates a specialized page bundle for a language-learning blog:

```
kontent/learning-indonesian/pronouns
├── homework
│   ├── 10_task.txt
│   ├── 20_mysolution.txt
│   ├── 30_solution.txt
│   └── 40_diff.txt
├── index.de.md
├── index.md
└── index.ru.md
```

This will additionally create a skeleton for clickable tabs representing various stages of solving a
grammar exercise. If you manage your homework using Git/GitHub, you can pipe the different commits
a particular exercise file, and a `wdiff` between your solution and the correct one. And then tell the
world about your accomplishment in three languages you already speak!


## Managing Author(s)

Authors are referred to from the front matter as a list of their IDs (there could be multiple
authors):

```yaml
authors:
- tatkins
- mmustermann
```

Before doing that, you will first need to set up the authors' data as described in the next section.
If an author with a particular ID doesn't exist, an exception will be thrown.

### Add Data Entries for the Authors

Open `data/authors.yaml` in a text editor and add the author(s). The example file can be copied
from `themes/errorist/exampleSite/data/authors.yaml`. If you forked the bootstrapping project, it's
already there.

The top-level key is a unique identifier for an author in ASCII alphanumeric characters
starting with a letter. All keys below are, strictly speaking, optional. Omitting them, however,
won't make your site look prettier.

- `name:` key should contain a fall-back name which will be displayed
should there be no author's page (`kontent/authors/<ID>/index.md`) for a particular `.Language`,
or should the `.Title` of such page be not defined. 

- `image:` can be either local (the path should be relative to `<PROJECT DIR>/static`)
or an URL, as in example below

- `homepage:` is the primary external URL of an author

- `social:` is a list of objects holding external URLs related to an author,
typically social network profiles. The `name:` will be translated or displayed as-is 
depending on whether translation is provided under i18n directory.

Below are a few examples using 'default persons' from various cultures:

```yaml
authors:
  tatkins:
    name: 'Tommy Atkins'
  mmustermann:
    name: 'Max Mustermann'
    image: /img/mmustermann.jpg
    homepage: http://example.com/dipl-ingeneur-informatiker/mmustermann
    social:
    - name: LinkedIn
      url: http://linkedin.com/mmustermann
    - name: Xing
      url: http://xing.com/mmustermann
  vpupkin:
    name: 'Vasya Pupkin'
    image: https://gravatar.com/avatar?d=mp&s=200
    homepage: http://example.com/vpupkin
  anonymous:
```

### Create Authors' Pages in All Supported Languages

```
hugo new -k page authors/<ID>
```

### Why `authors` aren't Included in the Archetypes' Front Matter

If some of your authors (or you) will write a multi-part article or a book that will occupy an
entire (sub)section, it won't make a lot of sense specifying the same author(s) in each single
chapter. Instead, you would put this in the front matter of the `_index.md` of the said opus:

```
cascade:
    authors:
    - mmustermann
```

### Content Listing in the Author's Page 

The author's page display a link only to the top page in content sub-hierarchy. That is, if a
page is written by the same author as the parent page, it will be skipped. 

## Overriding the Defaults

Do not make changes to the Errorist theme directly, because these will likely mess up the upgrading
process. Instead, override them using `layouts` and `static` directories in your project.

### Dummy Partials to Override

These sit under `themes/errorist/layouts/partials` directory. If you have forked the bootstrapping
project as recommended, these files are already included in `layouts/partials`. Otherwise, you will
need to copy or create them yourself.

- `disqus.html` if you plan to include Disqus comments. Just copy the code you've got from them, and
  the comments will be enabled on each page rendered with `single.html`.
- `favicons.html` to include all HTML related to your site's [favicon
  images](https://en.wikipedia.org/wiki/Favicon). To get both the images, and the HTML to
  include them you may choose to use a free online service, like
  [https://realfavicongenerator.net](https://realfavicongenerator.net).
- `gdpr.html` for the user consent widget which lets the user to accept cookies, agree to being
  tracked and what not. I'm not strong in international privacy legalese, so I'm not including such
  code on purpose.
- `head.html`  to customize the `<title>` of your pages and for anything else you might need to
  include between the `<head>` tags. 

### Customizing the Appearance

Copy `themes/errorist/static/css/customized.css` to `static/css/customized.css` in your project and
edit it. This will allow you to change the color of the stripe in the top of the page, the color of
your site name, and whether you'd like to display it in uppercase. 

You can also change how images in articles behave on mouse-over, hinting the user they can be
expanded.

### Choosing the Layout

You can change the layout of the pages rendered with `list.html` template by assigning a custom type
in `_index.*md` file in the section root. The types supported are:

- `list-compact` a more compact list, as the name suggests, still including `.Description` for the
  pages listed.
- `list-titles-only` same as above, but only titles are shown. This is the default for tags.

The plan is to include alternative layouts for the site `index.html` pages.

## Microdata Support

Currently, only `BreadCrumbs` and `Article` with its derivatives (`BlogPosting`, `NewsArticle`, etc.)
are supported. The type of the main content can be set in front matter with `schema:` parameter, the
default being `Article`.

## Reporting Bugs, Requesting Features, Pull Requests and Other Communication

Please use GitHub repository issues for that. I might occasionally respond to posts at Hugo support
forum, but I can easily overlook your posts as well. Please keep in mind that I'm running this
project in my free time.

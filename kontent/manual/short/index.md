---
title: "The Short Story"
date: 2020-08-02T20:36:26+02:00
description: The shortes possible way to set Errorist up if you know what you are doing.
draft: true
---

Recursively clone the bootstrapping project:

```
git clone --recurse-submodules git@github.com:tvendelin/errorist-bootstrap.git
```

This pulls the Errorist theme as a git submodule and provides you with a skeleton
project that needs only a few things to configure. Change into project directory and start the Hugo
server:

```
cd errorist-bootstrap
hugo server -D
```

Open `http://localhost:1313` in your browser. You should see a welcome message, and a link to the
manuals. You will also notice that two languages – English and Russian – are already
pre-configured for the sake of example, but this can be easily changed. We shall keep them
for a short while just to show you the ropes.

Depending on your experience with Hugo, you may want to follow through a step-by-step tutorial that
comes with the package, or just continue with the setup.

## Configure the Authors

Open `data/authors.yaml`, read the comments, and configure one or more authors by example. It's
YAML format, so keep an eye on indentation.

## Configure the Languages

Open `config/_default/languages.yaml`, read the comments, and configure to your needs. You might
want to this step later, though, after you've tried things out.

## Internationalization (I18N for Short)

If you've added new languages, you will also need to add translation files, one per new language.
Create `i18n` directory in your project root, and copy the English translations file to be used as a
template (for German in this example):

```
mkdir i18n
cp themes/errorist/i18n/en.yaml i18n/de.yaml
```

Open `i18n/de.yaml` and change English translations to German.

## Adjust Archetypes

If there were changes in supported languages, you will need to add/remove `index*.md` files
accordingly. That is, for each `index.md` file under `archetypes` directory in your project, you
will need an `index.<LANG>.md` file for each language you support. To remove these files for Russian
(if you are not going to use Russian), run

```
find archetypes -name 'index.ru.md' -exec rm '{}' \;
```

## Clean up Tutorials and Examples

Before you do, though, read a couple of sections in README – these cover the most useful and non-standard
features of Errorist.

```
# Re-create kontent/_index.md
rm kontent/_index.md
hugo new _index.md

# Remove test image bundle
rm -r kontent/images/plain-blue

# Remove the manual
rm -r kontent/manual
```

You are ready to create content and publish your website.

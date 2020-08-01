---
title: "Creating Content with Errorist"
date: 2020-07-27T17:15:38+02:00
description: A soft step-by-step tutorial covering the main features
draft: true
toc: true
---

This page will walk you through the steps necessary to add content to your website.

## Create a Section and Your First Page

From command line, run

```
hugo new -k page blog/test-page
```

This creates a 'Blogs' section (it should now appear in the top-left corner), and a 'page bundle', a directory
where your page lives:

```
kontent/blog/test-page/
├── index.md
└── index.ru.md
```

Two `index.*` files have been created, one for the default language (currently English) and Russian.
In subsequent sections we shall see how to customize this step.

Open `kontent/blog/test-page/index.md` in your editor. You will see some parameters between dashed
lines that describe your page. Together they are called 'front matter'. Let's set some of them, and
also add some content. The result should look like this:

```
---
title: Starting Out with Errorist
date: 2020-07-27T17:23:59+02:00
description: 'My first test page description'
draft: true
---

Once I save this file, my first page is created!
```

If you now click the 'Blogs' link (perhaps opening it in a new browser tab), you will get to the 'Blogs' section
front page, where your test page is listed with its `title` and `description` (you've set these in
the front matter). One more click, and you will see your page.

### Set the Section Name and Description

We probably want our section name to be 'Blog', not 'Blogs', so let's sort that out. Run this
command (mind the underscore):

```
hugo new blog/_index.md
```

Edit `kontent/blog/_index.md` to look like this:

```
---
title: "Blog"
date: 2020-07-27T17:49:45+02:00
description: My Personal blog with Errorist
draft: true
---
A few words as to what my blog is about.
```

Once you save the file, 'Blogs' will become 'Blog', and the section page will now display an
introduction text. This step should be repeated for each language used in your website, inserting
the language code as an additional file name suffix, so for Russian it would be `_index.ru.md`.

## Add the Main Image to a Page

Create a file `kontent/blog/test-page/main-img` and add a single line to it: `plain-blue`, either
using an editor or from command line:

```
echo 'plain-blue' >> kontent/blog/test-page/main-img
```

If you now open [http://localhost:1313/blog/test-page](http://localhost:1313/blog/test-page/), you
will see that an image `plain-blue` has been added to the page. Notice, it's got a caption already,
too. Where did it come from? Have a look into `kontent/images` directory:

```
kontent/images/
├── _index.md
└── plain-blue
    ├── example.png
    ├── index.md
    └── index.ru.md
```

This is where our sample image lives together with its captions and other metadata. The captions in
English and Russian are stored as content in `index.md` and `index.ru.md` files, respectively.
Here's the English one:

```
---
title: "Plain Blue"
date: 2020-07-27T18:37:21+02:00
draft: true
authors:
alt: alt tag of an example image
---
This is a caption for an example image
```

Essentially, we are looking at another page bundle, that just isn't rendered nor listed anywhere –
only included into another page. 

## Adding Content in Another Language

For that, let's edit `kontent/blog/test-page/index.ru.md` file in your page bundle.
If you haven't got a Cyrillic keyboard, as the case might be, here's a sample content you can
copy-and-paste:

```md
---
title: "Welcome to Errorist"
date: 2020-07-27T17:15:38+02:00
description: 
draft: true
toc: true
---
Царь Никита жил когда-то  
Праздно, весело, богато,  
Не творил добра, ни зла,  
И земля его цвела.  
Царь трудился понемногу,  
Кушал, пил, молился богу  
И от разных матерей  
Прижил сорок дочерей ...  
```

Now your test page has got a "Languages" navigation section and a link pointing to the Russian
version of your page. Not surprisingly, once you click it, you will see the Russian page you've just
created. The main image is already there, and notice – it has a Russian caption, too.

That's the benefit of using image bundles: image captions follow the image itself like ducklings
follow their mother-duck, so there never be a mismatch between an image and its caption.

## Creating an Image Bundle

Choose an image and some descriptive name for it. I shall use `myimage` in this example. Run

```
hugo new -k img images/myimage
```

Now copy the image file under `kontent/images/myimage/` (the name of the file doesn't matter).  Edit
`kontent/images/myimage/index.md` file. Then, in `kontent/blog/test-page/main-img` file, change
`plain-blue` to `myimage`, and check the page again:
[http://localhost:1313/blog/test-page](http://localhost:1313/blog/test-page/). The image supplied
with the theme should now change to yours.

## Including Images in the Content of a Page

The general Markdown syntax is

```md
![alt text](src "title text")
```

where `src` is the URL of the image, relative or absolute. With Errorist, if `src` does not contain
a dot (has no 'extention'), the image bundle under `kontent/images` directory will be looked up
instead. The `"title text"` will override the one in the image bundle, and `alt text` will override
the `alt` value. While this might be occasionally useful, most of the time you'd use the texts from
the image bundle, and so reduce it to just

```md
![](src)
```

Create a couple of image bundles and try to expariment with them. What happens if you
refer to nonexistent image bundle? What if image bundle exists, but hasn't got any image?

If you still prefer to use 'loose' images, an absolute path like `/file.jpg` as `src` value would
point to `static/file.jpg`, while relative path `file.jpg` would point to a file in the page bundle 
directory. You will still need to write the captions, so you'll hardly manage to cut any corners
here.

## Clickable Tabs

Errorist theme has been created with foreign language students in mind to let them (b)log their
learning process.

Clickable tabs allow you, among other things, to include your grammar exercises,
showing either the original task or your solution, or the correct solution, or the solution with
corrections, but one at a time. This is useful should you wish to revisit a particular topic in the
future. 

So let's create one. In a page bundle, create a directory `myhomework` (the name is arbitrary):

```
mkdir kontent/blog/test-page/homework
```

In this directory create four files as shown in the following code snippets. Here, the names
_are_ important. If you are experiencing the 'writes's block', here's example contents for the said
files:

`kontent/blog/test-page/homework/10_task.txt`:

```md
Señor Rodriguez ... católico, pero ... muerto. 
Señor Díaz ... vivo, pero hoy no ... católico.
```

`kontent/blog/test-page/homework/20_mysolution.txt`:

```md
Señor Rodriguez es católico, pero es muerto. 
Señor Díaz está vivo, pero hoy no es católico.
```

`kontent/blog/test-page/homework/30_solution.txt`:

```md
Señor Rodriguez era católico, pero está muerto. 
Señor Díaz está vivo, pero hoy no está católico.
```


`kontent/blog/test-page/homework/40_corrections.txt`:

```md
Señor Rodriguez ~~es~~ **era** católico, pero ~~es~~ **está** muerto. 
Señor Díaz está vivo, pero hoy no ~~es~~ **está** católico.
```

The `~~` part will be converted into `<del>` HTML tag, and `**` will become the `<strong>` tag. This
is part of Markdown language, and this is a topic of another tutorial, which is also included with
Errorist. 

Back to our tabs. Your page bundle structure will now look like this:

```
kontent/blog/test-page
├── homework
│   ├── 10_task.txt
│   ├── 20_mysolution.txt
│   ├── 30_solution.txt
│   └── 40_corrections.txt
├── index.md
├── index.ru.md
└── main-img
```

Add the following at the bottom of `kontent/blog/test-page/index.md`
`kontent/blog/test-page/index.ru.md` files:

```
{{</* tabs_bq "homework" */>}}
```

If you look at your test page now, you will see the clickable tabs at the bottom, showing our
fictional homework exercise.

The number at the beginning of a tab file name is for sorting, so that `10_task.txt` appears before
`40_corrections.txt`. The name of a tab is the file name stripped of its `number_` prefix and `.txt`
suffix, so `10_task.txt` becomes `task`. Then, if your localization file (more on that in a moment) contains the tab name as a
key, the translation for that key will be displayed. The entries for 'task', 'mysolution',
'solution' and 'corrections' are already included.

If you were creating a new language-learning blog post from scratch, you could create a page bundle
of this particular kind like this:

``` 
hugo new -k langblog blog/señores-vivos-y-muertos
```

If editing tab files feels a gruelling task, you are not alone. Consider managing your homework
using Git and GitHub, and then you can just pipe different commits and `wdiffs` of your exercises
into tabs files.

## Featuring an Article on the Front Page

It's time to put your best foot forward. In a page bundle directory, edit the `index.md` file, adding
`exposures` parameter to the front matter:

```
---
title: Starting Out with Errorist
date: 2020-07-27T17:23:59+02:00
description: 'My first test page description'
draft: true
exposures:
- major
---
```

If you check the front page of your site at [http://localhost:1313](http://localhost:1313), you
will notice that the main image for your test page now appears at the top together with what is
supposed to be a summary, but is in fact just 70 characters of the page squashed without any respect
to the original formatting. To fix the issue, create a file called `summary.md` (the name is
important), and write the summary for your page into it. Looks better, doesn't it?

To get a summary for the Russian page, too, you should create a `summary.ru.md`.

If you set `exposures` parameter to `minor`, your test page will be listed in the right column of
the front page.

## Adding Links 

The Markdown syntax for links is

```
[Visible Text](http://example.com "title")
```

where 'Visible Text' is the text displayed on the page, and 'title' is
the text that appears when you hover mouse cursor over it, but do not click yet.

To refer to the page of your website, you should use a _shortcode_ in place of URL:

```
[Visible Text]({{< ref "/manual/readme#configure-languages-that-your-site-supports">}} "languages to use")
```

The path is relative to the domain, so if your site is `http://example.com`, the above code will
be rendered as `http://example.com//manual/readme#configure-languages-that-your-site-supports`. The
benefit of usng this shortcode is that Hugo will throw exeption, if this page doesn't exist – so you
are guaranteed against dead links at your own site at least.

Do not use trailing `/` when using the shortcode – it will result in an error.

## Configure Your Project

Now that you've got an idea how thing work with Errorist, you should configure your project.
Specifically, you will need to:

- [Define the languages to use]({{< ref "/manual/readme#configure-languages-that-your-site-supports">}})
- [Adjust the archetypes]({{< ref "/manual/readme#archetypes" >}})
- [Set up the authors]({{< ref "/manual/readme#managing-authors" >}})

All of the links above point to sections in [README]({{< ref "/manual/readme" >}}) file. Once done,
you should clean up the project, add some real content, build your site locally, and then publish it.

## Clean Up

```
# Remove your test blog
rm -r kontent/blog

# Re-create kontent/_index.md
rm kontent/_index.md
hugo new _index.md

# Remove test image bundles
find kontent/images/* -type d -exec rm -r '{}' \; 2>/dev/null

# Remove archetypes for Russian language, if you won't be using it
find archetypes -name 'index.ru.md' -exec rm '{}' \;
```

You might want to keep the Manual section for a while. It won't be published as long as you keep
`draft: true` in the front matter. It will show up only when you run Hugo with `-D` flag.

To remove the manuals as well, run
```
# Remove (optionally) the manual
rm -r kontent/manual
```

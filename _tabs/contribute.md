---
title: Contribute
icon: fas fa-pencil-alt
order: 5
---

## Posts

Create a new file named `YYYY-MM-DD-TITLE.md` and in the `_posts` directory, the top of the file must contain teh following:
```
---
title: Post Title
author:
  name: Author Name
  link: https://github.com/author
date: YYYY-MM-DD HH:MM:SS
categories: [Main Category, Secondary Category]
tags: [tag1, tag2, tag3]
---
```

If you want to add an image to the top of the post contents, specify the attribute src, width, height, and alt for the image:

```
---
image:
  src: /path/to/image/file
  width: 1000   # in pixels
  height: 400   # in pixels
  alt: image alternative text
---
```

## Titles
---

```
# H1 - heading

## H2 - heading

### H3 - heading

#### H4 - heading
```

<h1 data-toc-skip>H1 - heading</h1>

<h2 data-toc-skip>H2 - heading</h2>

<h3 data-toc-skip>H3 - heading</h3>

<h4>H4 - heading</h4>

---
<br>

## Blockquotes

```
<blockquote>
  Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.
  <span class="author"><i>Lorem ipsum</i></span>
</blockquote>
```

<blockquote>
  Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.
  <span class="author"><i>Lorem ipsum</i></span>
</blockquote>

## Comment Boxes

If you need multiple paragraphs, enter `<br/><br/>` tags.

### Important

```
{% include important.html content="This is my note. All the content I type here is treated as a single paragraph." %}
```

{% include important.html content="This is my note. All the content I type here is treated as a single paragraph." %}

### Note

```
{% include note.html content="This is my note. All the content I type here is treated as a single paragraph." %}
```

{% include note.html content="This is my note. All the content I type here is treated as a single paragraph." %}

### Tip
```
{% include tip.html content="This is my note. All the content I type here is treated as a single paragraph." %}
```

{% include tip.html content="This is my note. All the content I type here is treated as a single paragraph." %}

### Warning
```
{% include warning.html content="This is my note. All the content I type here is treated as a single paragraph." %}
```

{% include warning.html content="This is my note. All the content I type here is treated as a single paragraph." %}

### Callouts

The style for the callout. Options are `danger`, `default`, `primary`, `success`, `info`, and `warning`.

```
{% include callout.html content="This is my callout. It has a border on the left whose color you define by passing a type parameter. I typically use this style of callout when I have more information that I want to share, often spanning multiple paragraphs. " type="primary" %}
```

{% include callout.html content="This is my callout. It has a border on the left whose color you define by passing a type parameter. I typically use this style of callout when I have more information that I want to share, often spanning multiple paragraphs. " type="primary" %}

### Code Box Highlight


### Specific Languages

If you want to avoid showing line numbers, add `{: .nolineno}` below your codebox, like so:

````
```bash
if [ $? -ne 0 ]; then
    echo "The command was not successful.";
    #do the needful / exit
fi;
```
{: .nolineno}
````

```bash
if [ $? -ne 0 ]; then
    echo "The command was not successful.";
    #do the needful / exit
fi;
```
{: .nolineno}

#### Console

````
```console
$ env |grep SHELL
SHELL=/usr/local/bin/bash
PYENV_SHELL=bash
```
````

```console
$ env |grep SHELL
SHELL=/usr/local/bin/bash
PYENV_SHELL=bash
```

#### Shell

````
```bash
if [ $? -ne 0 ]; then
    echo "The command was not successful.";
    #do the needful / exit
fi;
```
````

```bash
if [ $? -ne 0 ]; then
    echo "The command was not successful.";
    #do the needful / exit
fi;
```

### Specific filename

````
```sass
@import
  "colors/light-typography",
  "colors/dark-typography"
```
````

```sass
@import
  "colors/light-typography",
  "colors/dark-typography"
```

## Liquid Codes

```
{% raw %}
```liquid
{% if product.title contains 'Pack' %}
  This product's title contains the word Pack.
{% endif %}
```
{% endraw %}
```

{% raw %}
```liquid
{% if product.title contains 'Pack' %}
  This product's title contains the word Pack.
{% endif %}
```
{% endraw %}

## Lists

### Ordered list

```
1. First
2. Second
3. Third
```

1. First
2. Second
3. Third

### Unordered list

```
- Chapter
  - Section
    - Paragraph
```

- Chapter
  - Section
    - Paragraph

### Task list

```
- [ ] TODO
- [x] Completed
- [ ] Build Something
  - [x] Come up with idea
  - [ ] Sketch Workflow
  - [ ] Write Code
```

- [ ] TODO
- [x] Completed
- [ ] Build Something
  - [x] Come up with idea
  - [ ] Sketch Workflow
  - [ ] Write Code

### Description list

```
Something
: Description of what something is

Nothing
: Description of what nothing is
```

Something
: Description of what something is

Nothing
: Description of what nothing is

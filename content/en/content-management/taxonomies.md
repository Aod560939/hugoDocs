---
title: Taxonomies
linkTitle: Taxonomies
description: Hugo includes support for user-defined taxonomies.
keywords: [taxonomies,metadata,front matter,terms]
categories: [content management]
menu:
  docs:
    parent: content-management
    weight: 150
toc: true
weight: 150
aliases: [/taxonomies/overview/,/taxonomies/usage/,/indexes/overview/,/doc/indexes/,/extras/indexes]
---

## What is a Taxonomy?

Hugo includes support for user-defined groupings of content called **taxonomies**. Taxonomies are classifications of logical relationships between content.

### Definitions

Taxonomy
: a categorization that can be used to classify content

Term
: a key within the taxonomy

Value
: a piece of content assigned to a term


## Example Taxonomy: Movie Website

Let's assume you are making a website about movies. You may want to include the following taxonomies:

* Actors
* Directors
* Studios
* Genre
* Year
* Awards

Then, in each of the movies, you would specify terms for each of these taxonomies (i.e., in the [front matter] of each of your movie content files). From these terms, Hugo would automatically create pages for each Actor, Director, Studio, Genre, Year, and Award, with each listing all of the Movies that matched that specific Actor, Director, Studio, Genre, Year, and Award.

### Movie Taxonomy Organization

To continue with the example of a movie site, the following demonstrates content relationships from the perspective of the taxonomy:

```txt
Actor                    <- Taxonomy
    Bruce Willis         <- Term
        The Sixth Sense  <- Value
        Unbreakable      <- Value
        Moonrise Kingdom <- Value
    Samuel L. Jackson    <- Term
        Unbreakable      <- Value
        The Avengers     <- Value
        xXx              <- Value
```

From the perspective of the content, the relationships would appear differently, although the data and labels used are the same:

```txt
Unbreakable                 <- Value
    Actors                  <- Taxonomy
        Bruce Willis        <- Term
        Samuel L. Jackson   <- Term
    Director                <- Taxonomy
        M. Night Shyamalan  <- Term
    ...
Moonrise Kingdom            <- Value
    Actors                  <- Taxonomy
        Bruce Willis        <- Term
        Bill Murray         <- Term
    Director                <- Taxonomy
        Wes Anderson        <- Term
    ...
```

## Hugo Taxonomy Defaults {#default-taxonomies}

Hugo natively supports taxonomies.

Without adding a single line to your [site config][config] file, Hugo will automatically create taxonomies for `tags` and `categories`. That would be the same as manually [configuring your taxonomies](#configure-taxonomies) as below:

{{< code-toggle file="config" copy=false >}}
[taxonomies]
  tag = "tags"
  category = "categories"
{{</ code-toggle >}}

If you do not want Hugo to create any taxonomies, set `disableKinds` in your [site config][config] to the following:

{{< code-toggle file="config" copy=false >}}
disableKinds = ["taxonomy","term"]
{{</ code-toggle >}}

{{% page-kinds %}}

### Default Destinations

When taxonomies are used---and [taxonomy templates] are provided---Hugo will automatically create both a page listing all the taxonomy's terms and individual pages with lists of content associated with each term. For example, a `categories` taxonomy declared in your configuration and used in your content front matter will create the following pages:

* A single page at `example.com/categories/` that lists all the [terms within the taxonomy]
* [Individual taxonomy list pages][taxonomy templates] (e.g., `/categories/development/`) for each of the terms that shows a listing of all pages marked as part of that taxonomy within any content file's [front matter]

## Configure Taxonomies

Custom taxonomies other than the [defaults](#default-taxonomies) must be defined in your [site config][config] before they can be used throughout the site. You need to provide both the plural and singular labels for each taxonomy. For example, `singular key = "plural value"` for TOML and `singular key: "plural value"` for YAML.

### Example: Adding a custom taxonomy named "series"

{{% note %}}
While adding custom taxonomies, you need to put in the default taxonomies too, _if you want to keep them_.
{{% /note %}}

{{< code-toggle file="config" copy=false >}}
[taxonomies]
  tag = "tags"
  category = "categories"
  series = "series"
{{</ code-toggle >}}

### Example: Removing default taxonomies

If you want to have just the default `tags` taxonomy, and remove the `categories` taxonomy for your site, you can do so by modifying the `taxonomies` value in your [site config][config].

{{< code-toggle file="config" copy=false >}}
[taxonomies]
  tag = "tags"
{{</ code-toggle >}}

If you want to disable all taxonomies altogether, see the use of `disableKinds` in [Hugo Taxonomy Defaults](#default-taxonomies).

{{% note %}}
You can add content and front matter to your taxonomy list and taxonomy terms pages. See [Content Organization](/content-management/organization/) for more information on how to add an `_index.md` for this purpose.

Much like regular pages, taxonomy list [permalinks](/content-management/urls/) are configurable, but taxonomy term page permalinks are not.
{{% /note %}}

{{% warning %}}
The configuration option `preserveTaxonomyNames` was removed in Hugo 0.55.

You can now use `.Page.Title` on the relevant taxonomy node to get the original value.
{{% /warning %}}

## Add Taxonomies to Content

Once a taxonomy is defined at the site level, any piece of content can be assigned to it, regardless of [content type] or [content section].

Assigning content to a taxonomy is done in the [front matter]. Simply create a variable with the *plural* name of the taxonomy and assign all terms you want to apply to the instance of the content type.

{{% note %}}
If you would like the ability to quickly generate content files with preconfigured taxonomies or terms, read the docs on [Hugo archetypes](/content-management/archetypes/).
{{% /note %}}

### Example: Front Matter with Taxonomies

{{< code-toggle file="content/example.md" fm=true copy=false >}}
title = "Hugo: A fast and flexible static site generator"
tags = [ "Development", "Go", "fast", "Blogging" ]
categories = [ "Development" ]
series = [ "Go Web Dev" ]
slug = "hugo"
project_url = "https://github.com/gohugoio/hugo"
{{</ code-toggle >}}

## Order Taxonomies

A content file can assign weight for each of its associate taxonomies. Taxonomic weight can be used for sorting or ordering content in [taxonomy list templates] and is declared in a content file's [front matter]. The convention for declaring taxonomic weight is `taxonomyname_weight`.

The following show a piece of content that has a weight of 22, which can be used for ordering purposes when rendering the pages assigned to the "a", "b" and "c" values of the `tags` taxonomy. It has also been assigned the weight of 44 when rendering the "d" category page.

### Example: Taxonomic `weight`

{{< code-toggle copy=false >}}
title = "foo"
tags = [ "a", "b", "c" ]
tags_weight = 22
categories = ["d"]
categories_weight = 44
{{</ code-toggle >}}

By using taxonomic weight, the same piece of content can appear in different positions in different taxonomies.

{{% note "Limits to Ordering Taxonomies" %}}
Currently taxonomies only support the [default `weight => date` ordering of list content](/templates/lists/#default-weight--date--linktitle--filepath). For more information, see the documentation on [taxonomy templates](/templates/taxonomy-templates/).
{{% /note %}}

## Add custom metadata to a Taxonomy or Term

If you need to add custom metadata to your taxonomy terms, you will need to create a page for that term at `/content/<TAXONOMY>/<TERM>/_index.md` and add your metadata in its front matter. Continuing with our 'Actors' example, let's say you want to add a Wikipedia page link to each actor. Your terms pages would be something like this:

{{< code-toggle file="content/actors/bruce-willis/_index.md" fm=true copy=false >}}
title: "Bruce Willis"
wikipedia: "https://en.wikipedia.org/wiki/Bruce_Willis"
{{< /code-toggle >}}

[`urlize` template function]: /functions/urlize/
[content section]: /content-management/sections/
[content type]: /content-management/types/
[documentation on archetypes]: /content-management/archetypes/
[front matter]: /content-management/front-matter/
[taxonomy list templates]: /templates/taxonomy-templates/#taxonomy-list-templates
[taxonomy templates]: /templates/taxonomy-templates/
[terms within the taxonomy]: /templates/taxonomy-templates/#taxonomy-terms-templates "See how to order terms associated with a taxonomy"
[config]: /getting-started/configuration/

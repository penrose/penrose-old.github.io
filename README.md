# Penrose Documentation

![assets/img/penrose-logo.png](assets/img/penrose-logo.png)

This is a documentation site for [penrose](http://penrose.ink). It's not decided
yet where it will be served from, and for it's development is being served 
[here](https://vsoch.github.io/penrose.github.io).

## Setup

 1. Install [Jekyll](https://jekyllrb.com/docs/installation/) locally. For Ruby, I recommend [rbenv](https://github.com/rbenv/rbenv).
 2. Install Jekyll dependencies with `bundle install`
 3. To serve the development server run `bundle exec jekyll serve`

## Folders Included
If you aren't familiar with the structure of a Jekyll site, here is a quick overview:

 - [_config.yml](_config.yml) is the primary configuration file for the site. Variables in this file render as `{{ site.var }}` in the various html includes and templates.
 - [_layouts](_layouts) are base html templates for pages
 - [_includes](_includes) are snippets of html added to layouts
 - [pages](pages) are generic pages (e.g., changelog) that aren't considered docs
 - [_docs](_docs) is a collection of folders that get rendered into the docs sidebar and pages
 - [assets](assets) includes all static assets
 - [_data](_data) has different data files (they can be in `.yml` or `.csv` to render into the site.

## How Do I...

**Add a person, or change them to emeritus?**

The data file [people.yml](_data/people.yml) renders into the listing of people 
involved in the project, so if you edit this file you can add or update a person.

**Edit a Page**

The pages that render the site are all in the [_docs](_docs) folder. It starts
with an underscore because it's a [Jekyll collection](https://jekyllrb.com/docs/collections/).
The first level of folders represents the categories in the sidebar, so you can
look in the appropriately named folder to find your page of interest. 

**Create a new Page**

Create a new markdown file in the category folder of your choice under [_docs](_docs).
Each page has a chunk of text at the top called front end matter, and this is where
you can write the title and category name:

```yml
---
title: This Page is About Pancakes
category: Breakfast
order: 1
---
```

If you are adding a new page, it's often easiest to copy another page in the same
category folder to inherit the category name. You should also be sure that the order
variable (the ordering in the sidebar) renders to what you want. If you don't 
want the page to appear in search, add the tag `excluded_in_search: true`.

**Edit the Changelog**

The Changelog renders as a site post! This means that you can create a new file
under [_posts](_posts) (and use the same date naming convention) and tag it with
minor or major. It will render nicely onto the changelog page.

## Components

### Search

Search is made possible by [lunr](https://lunrjs.com/). It's by far my favorite solution
I've encountered for generating a static search. However, there are likely some pages
that you don't want to add. To disclude them from search, just add `excluded_in_search: true` 
to any documentation page's front matter.

### Navigation

* Change `site.show_full_navigation` to control all or only the current navigation group being open.

#### Previous Art

This template is a modified version of Cloud Canyon's 
[Edition Jekyll Template](https://github.com/CloudCannon/edition-jekyll-template). 
Thank you for this beautiful work!

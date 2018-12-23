---
title: The Penrose Library
category: Domain Development
order: 2
---

The Penrose Library hosted at [https://www.github.com/penrose-lib](https://www.github.com/penrose-lib)
is a Github organization that holds repositories for each of the following 
components that we've discussed:

 - **Domain Specific Language Logic**
 - **Style**
 - **Substance**

If you look at the organization, you will see repositories named with
prefixes to indicate the kind of component above:

 - **domain_** indicates a domain specific language, meaning you will find dsl files within
 - **style_** indicates a style repository
 - **sub_** indicates a substance repository

While each repository is intended to provide a version controlled domain, style,
or substance file(s), it's also likely that you will find repositories with
mixed types. For example, the maintainer of a domain might find it useful
to include a set of example style and substance to help guide an interested
developer. These examples might also be used as defaults.

## Repository Metadata

Each repository is responsible for serving its own metadata, and the penrose
interface can query this metadata to find dependencies.

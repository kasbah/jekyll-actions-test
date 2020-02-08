---
layout: page
title: Configuring the Git-Building output
permalink: /syntax/buildconfsyntax
---


In the base directory of a documentation project you can make a [YAML] file called `buildconf.yaml`.

[YAML]: https://en.wikipedia.org/wiki/YAML


## Recommended information

The following keys are recommended to add information:

* **Authors**: Either a list of authors or the name of the author
* **Affiliation**: A group the project is affiliated with
* **License**: The licence that the project is released under
* **Email**: Contact email address

## Categories and parts

As standard there are two categories: `Part` and `Tool`. The main difference is how the are counted. Parts are added, whereas for tools the maximum number used in any step is recorded. The logic for this is that if you need 1 M3 nut and a 5.5 mm nut driver in one step, and 1 M3 nut and a 5.5 mm nut driver in a later step, then you will need two M3 nuts but only one 5.5 mm nut driver. To set these addition rules `Part` has a `Reuse` property set to `False`, `Tool` has a reuse property set to `True`. 

Categories also have a `DisplayName`. When you set the category in a part link you use its name (`Part` or `Tool`), but when it is displayed in a bill of materials the display name is used ("Parts" and "Tools").

An example custom category might be:

```YAML
CustomCategories:
    PrintedTool:
        Reuse: True
        DisplayName: Printed tools
```

As default anything with a category not explicitly stated is assumed to be a `Part`. The default category can be changed with:

```YAML
DefaultCategory: PrintedTool
```


## Other config options

* **LandingPage**: Specifies which markdown file should become index.html in and HTML build. Not needed if one file is called `index.md`
* **WebsiteRoot**: Sets the root domain for links in the static HTML website that can be built with `gitbuilding build-html` *Default: '/'*
* **PageBOMTitle**: Sets a message to be displayed above the bill of materials on a page when the syntax `{{BOM}}` is used.
* **Title**: Can be used to override the title of the project for the HTML build. Default is the title of index.md or the `LandingPage`
* **Fussy**: Boolean value. If true Git-Building will warn about possible problems with the documentation. *Default: True*


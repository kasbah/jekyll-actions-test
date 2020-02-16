---
layout: page
title: Configuring the GitBuilding output
permalink: /syntax/buildconfsyntax
---

This page explains how you can configure the output of GitBuilding. Most of the customisation is done by putting information in  a [YAML] file called `buildconf.yaml`. You can also customise the HTML output from GitBuilding by editing the `buildconf.yaml` file and adding files to the `assets` directory.

[YAML]: https://en.wikipedia.org/wiki/YAML

<h3>Table of contents</h3>
* Do not remove this line (it will not be displayed)
{:toc}

## Recommended information

It is recommended to add following keys to the `buildconf.yaml` to add important information:

* **Authors**: Either a list of authors or the name of the author
* **Affiliation**: A group the project is affiliated with
* **License**: The licence that the project is released under
* **Email**: Contact email address

## Categories and parts

The `buildconf.yaml` file can be used to customise the categories for parts in your project.

As standard there are two categories: `Part` and `Tool`. The main difference is how the are counted. Parts are added, whereas for tools the maximum number used in any step is recorded. The logic for this is that if you need 1 M3 nut and a 5.5 mm nut driver in one step, and 1 M3 nut and a 5.5 mm nut driver in a later step, then you will need two M3 nuts but only one 5.5 mm nut driver. To set these addition rules `Part` has a `Reuse` property set to `False`, `Tool` has a reuse property set to `True`. 

Categories also have a `DisplayName`. When you set the category in a part link you use its name (`Part` or `Tool`), but when it is displayed in a bill of materials the display name is used ("Parts" and "Tools").

An example custom category might be:

```
CustomCategories:
    PrintedTool:
        Reuse: True
        DisplayName: Printed tools
```

As default anything with a category not explicitly stated is assumed to be a `Part`. The default category can be changed with:

```
DefaultCategory: PrintedTool
```

## Customising HTML output

When you run `gitbuilding serve` or `gitbuilding build-html` GitBuilding will generate an HTML website for your documentation. This output can be customised as follows:

### Custom Header and Footer

The default header and footer for the generated HTML pages can be overridden using the `buildconf.yaml`. For example:

```
HTMLOptions:
    AcknowledgeGitBuilding: True
    CustomFooter: >
        <p class="author">Copyright {Authors} {Year}</p>
        <p class="license">{Title} is released under {License}</p>
        <p><b>Disclaimer: </b>{Disclaimer}</p>
    CustomHeader: >
        <a class="site-title" href="{Root}">{Title}</a>
        <p>This is a custom header</p>
```

The `AcknowledgeGitBuilding` key sets whether to show the "Documentation powered by Git Building" message above the footer.

The `CustomFooter` and `CustomHeader` keys set the HTML for the header. A number of variables from GitBuilding can be used by putting them inside `{}`s. The list of variables are:

* `Title` - Project title
* `Year` - The current year
* `Root` - The website root (see below)
* `Authors` - Authors of the project
* `Email` - Contact email for the project
* `Affiliation` - Project affiliation
* `License` - Project licence, will link to the licence if recognised.

Custom variables can be used by specifying them in the `buildconf.yaml` file as follows:

```
Variables:
    version: v6.2
HTMLOptions:
    CustomFooter: >
        <p class="version">The current version of this project is {version}</p>
```

The footer would be modified to
```
<p class="version">The current version of this project is v6.2</p>
```


### Custom CSS and favicon

GitBuilding scans for a directory called `assets` in the root directory. If this directory exists and css files inside it are included in the `<head>` of the HTML. These style sheets can then be used to override the default CSS of GitBuilding.

The `assets` directory is also scanned for a `favicon.ico` file or any PNG favicon which specifies its size (i.e. `favicon-16x16.png`). If any files are matched they are used as the favicon for the documentation instead of the GitBuilding favicon.

*The PNG favicon will only be found if it matches the following regular expression:*  
`^favicon-[0-9]+x[0-9]+\.png$`

## Other config options

Other options for the `buildconf.yaml` file:

* **LandingPage**: Specifies which markdown file should become index.html in and HTML build. Not needed if one file is called `index.md`
* **WebsiteRoot**: Sets the root domain for links in the static HTML website that can be built with `gitbuilding build-html` *Default: '/'*
* **PageBOMTitle**: Sets a message to be displayed above the bill of materials on a page when the syntax {% raw %}`{{BOM}}`{% endraw %} is used.
* **Title**: Can be used to override the title of the project for the HTML build. Default is the title of index.md or the `LandingPage`
* **Fussy**: Boolean value. If true Git-Building will warn about possible problems with the documentation. *Default: True*


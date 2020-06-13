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
* **License**: The licence that the project is released under. If this is SPDX license identifier that GitBuilding supports, the licence file will automatically be included. If not you should include a copy of the license file in your documentation folder.
* **LicenseFile**: If the license specified is not a recognised SPDX license identifier then this should be a relative link to the license file in your documentation folder.
* **Email**: Contact email address

## Categories and parts

The `buildconf.yaml` file can be used to customise the categories for parts in your project.

As standard there are two categories: `Part` and `Tool`. The main difference is how the are counted. Parts are added, whereas for tools the maximum number used in any step is recorded. The logic for this is that if you need 1 M3 nut and a 5.5 mm nut driver in one step, and 1 M3 nut and a 5.5 mm nut driver in a later step, then you will need two M3 nuts but only one 5.5 mm nut driver. To set these addition rules `Part` has a `Reuse` property set to `False`, `Tool` has a reuse property set to `True`. 

Categories also have a `DisplayName`. When you set the category in a part link you use its name (`Part` or `Tool`), but when it is displayed in a bill of materials the display name is used ("Parts" and "Tools").

An example custom category might be:

<pre class="example-block">
{% raw %}
CustomCategories:
    PrintedTool:
        Reuse: True
        DisplayName: Printed tools
{% endraw %}
</pre>

As default anything with a category not explicitly stated is assumed to be a `Part`. The default category can be changed with:

<pre class="example-block">
{% raw %}
DefaultCategory: PrintedTool
{% endraw %}
</pre>

## Custom navigation

**Note custom navigation will probably update soon**
 
Currently you cannot override the auto-generated navigation, but you can prepend it wil extra links. These links can also be nested. The syntax to do this is:

<pre class="example-block">
{% raw %}
Navigation:
  - Link: relative_url_1
    Title: LinkName1
  - Link: relative_url_2
    Title: LinkName2
    SubNavigation:
      - Link: sublink_url
        Title: SubLinkName
{% endraw %}
</pre>

## Customising HTML output

When you run `gitbuilding serve` or `gitbuilding build-html` GitBuilding will generate an HTML website for your documentation. This output can be customised as follows:

### Custom Header and Footer

The default header and footer for the generated HTML pages can be overridden using the `buildconf.yaml`. For example:

<pre class="example-block" markdown="0">

HTMLOptions:
    AcknowledgeGitBuilding: True
    CustomFooter: >
        &lt;p class="author">Copyright {authors} {Year}&lt;/p>
        &lt;p class="license">{title} is released under {license}</p>
        &lt;p>&lt;b>Disclaimer: &lt;/b>{disclaimer}&lt;/p>
    CustomHeader: >
        &lt;a class="site-title" href="{Root}">{title}&lt;/a>
        &lt;p>This is a custom header&lt;/p>

</pre>

The `AcknowledgeGitBuilding` key sets whether to show the "Documentation powered by Git Building" message above the footer.

The `CustomFooter` and `CustomHeader` keys set the HTML for the header. A number of variables from GitBuilding can be used by putting them inside `{}`s. The list of variables are:

* `title` - Project title
* `year` - The current year
* `root` - The website root (see below)
* `authors` - Authors of the project
* `email` - Contact email for the project
* `affiliation` - Project affiliation
* `license` - Project licence, will link to the licence if recognised.

Custom variables can be used by specifying them in the `buildconf.yaml` file as follows:

<pre class="example-block">

Variables:
    version: v6.2
HTMLOptions:
    CustomFooter: >
        &lt;p class="version">The current version of this project is {version}&lt;/p>

</pre>

The footer would be modified to
<pre class="example-block">

&lt;p class="version">The current version of this project is v6.2&lt;/p>

</pre>


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
* **Exclude**: List of files that are in the documentation directory that you do not want to be processed. (Note: all files beginning with a `.` or a `_` are also excluded.  *Default: [README.md]*
* **ForceOutput**: This is a list of file that are forced to be in the output. As standard non-markdown files are only included in the output if they are linked to in a BuildUp file. *Default is an empty list*
* **Title**: Can be used to override the title of the project for the HTML build. Default is the title of index.md or the `LandingPage`
* **Fussy**: Boolean value. If true GitBuilding will warn about possible problems with the documentation. *Default: True*


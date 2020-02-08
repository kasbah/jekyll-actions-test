---
layout: page
title: BuildUp Syntax
permalink: /syntax/buildup
---


A BuildUp page is a [markdown] page with extra information specified for some links. This information can be used to add parts to you bill of materials, or to identify the linked page as an instruction page or "step". There are also syntax to include calculated information such as the bill of materials.

[markdown]: https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet

Steps allow you to write hierarchical documentation and count all of the parts used in these steps:

![](/assets/Steps.png)

Steps can also be used to generate navigation for the documentation.

## BuildUp Syntax

### Page title

BuildUp may use a page's title in another page. A pages title is the first instance of a `H1` (Heading Level 1) markdown title using the `#` syntax.

### Step links

To link to another file and have it included in as a step you append `{Step: True}` to the link for example

    [Print the components](print.md){Step: True}

or in reference style:

    [Print the components]{Step: True}
    [Print the components]: print.md

If you want to use the title of the page as the link text then you can replace the link text with a `.`, so

    [.](print.md){Step: True}

will also give the same result if the title of `print.md` is "Print the components". This is not implemented for reference style links as there is nothing to reference.

Note that the space after the `:` is optional, and the text is case insensitive.

### Part links

To have a part counted in the bill of materials a part link is used. A part link is a link followed by the syntax `{Qty: quantity}` where quantity can be a number of free text. The following are valid part links:

    [screws]{Qty: 4}
    [bigger screws](bigscrews.md){qty:Many}

Just as with the step links, the space after the `:` is optional, and the text is case insensitive.

Multiple links to the same item will be counted. Only the first one needs to have the link (URI) specified as the final markdown will create a reference style link for all parts. Linking to the same item using different text can be achieved using [markdown] reference link syntax:

    Screw an [M3 nut](m3nut.md){Qty: 1} onto the [M3x25 screw][m3x25screw.md]{Qty: 1}. Then add [a second nut][M3 nut]{Qty: 1} to lock it in place.

This will count two `M3 Nuts` and one `M3x25 screw`.

Numbers are counted as expected. If text entries are added the result is `some`. Git-Building is **not yet** unit aware, but later it should be able to add `10 g` and `45 g` to get `55 g`. 

The part link (URI) can be a link to anything for example another website, a markdown file, or an STL file. To use link to a file in a [part library]({{ /syntax/builduplibrary/ | prepend: site.baseurl }}) you should specify it in the form

    [Part](library.yaml#key){Qty: 1}
    
where `library.yaml` is the library file, and `key` is the key for that part.

#### Extra part information

To add extra information to a part you can use a reference style link, extra information can be added inside `{}`s in the alt-text:

    [Large screwdriver]: screwdriver "{Cat: Tool, Note: 'A Pozidriv works best, but you could try a Philips.'}"

Extra information is specified in key value syntax that is case insensitive, a comma is used before a new key. The following keys are defined:

* `TotalQty` - This sets the total of a part used on a page. It can be use to over-ride the counted value. It can also be used to check all uses are mentioned in the text as Git-Building will produce a warning if they don't match.
* `Cat` - Short for "category". This defines the type of part. Standard BuildUp has two categories `Part` and `Tool`. Parts are counted normally, for tools the maximum quantity used in a single link is used for the bill of materials. Git-Building can define custom categories in its [configuration]({{/syntax/buildconfsyntax | prepend: site.baseurl }}).
* `Note` - This sets a note which is displayed with the part in the bill of materials.

### Displaying Bill of Materials
{% raw %}
To display a pages bill of materials in the markdown the use `{{BOM}}`. `{{BOM}}` will be replaced in the output with the bill of materials.

For pages with a larger bill of materials it make sense to have it on a separate page. To generate the page, `{{BOMlink}}` generates the page and is replaced with, for example to link to the bill of materials:
    
    This bill of materials can be found [here]({{BOMlink}})
{% endraw %}

---
layout: page
title: BuildUp markdown
permalink: /syntax/buildup
---

GitBuilding documentation is written in a format we call BuildUp markdown. A BuildUp markdown is a modified [markdown] format with extra information specified.. This information can be used to add parts to you bill of materials, or to identify the linked page as an instruction page or "step". There are also syntax to include calculated information such as the bill of materials. By defining steps you can write hierarchical documentation and count all of the parts used in these steps:

[markdown]: https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet

![]({{site.baseurl}}/assets/Steps.png)

Steps are also be used to auto-generate navigation for the documentation.

<h3>Table of contents</h3>
* Do not remove this line (it will not be displayed)
{:toc}

## BuildUp Syntax

### Page title

BuildUp may use a page's title in another page. A pages title is the first instance of a `H1` (Heading Level 1) markdown title using the `#` syntax.

### Step links

To link to another file and have it included in as a step you append `{step}` to the link for example
<pre class="example-block">
{% raw %}
[Print the components](print.md){step}
{% endraw %}
</pre>

or in reference style:

<pre class="example-block">
{% raw %}
[Print the components]{step}
[Print the components]: print.md
{% endraw %}
</pre>
    

If you want to use the title of the page as the link text then you can replace the link text with a `.`, so
<pre class="example-block">
{% raw %}
[.](print.md){step}
{% endraw %}
</pre>

will also give the same result if the title of `print.md` is "Print the components". This is not implemented for reference style links as there is nothing to reference.

Note that `{step}` is case insensitive, `{Step}` or `{STEP}` are also valid.

### Part links

To have a part counted in the bill of materials a part link is used. A part link is a link followed by the syntax `{Qty: quantity}` where quantity can be a number of free text. The following are valid part links:

<pre class="example-block">
{% raw %}
[screws]{Qty: 4}
[bigger screws](bigscrews.md){qty:Many}
{% endraw %}
</pre>

Just as with the step links `qty` is case insensitive, also the space after the `:` is optional.

Multiple links to the same item will be counted. Only the first one needs to have the link (URI) specified as the final markdown will create a reference style link for all parts. Linking to the same item using different text can be achieved using [markdown] reference link syntax:
<pre class="example-block">
{% raw %}
Screw an [M3 nut](m3nut.md){Qty: 1} onto the [M3x25 screw][m3x25screw.md]{Qty: 1}. Then add[a second nut][M3 nut]{Qty: 1} to lock it in place.
{% endraw %}
</pre>

This will count two `M3 Nuts` and one `M3x25 screw`.

Numbers are counted as expected. If text entries are added the result is `some`. Git-Building is **not yet** unit aware, but later it should be able to add `10 g` and `45 g` to get `55 g`. 

The part link (URI) can be a link to anything for example another website, a markdown file, or an STL file. To use link to a file in a [part library]({{site.baseurl}}/syntax/builduplibrary/) you should specify it in the form

<pre class="example-block">
{% raw %}
[Part](library.yaml#key){Qty: 1}
{% endraw %}
</pre>
    
where `library.yaml` is the library file, and `key` is the key for that part.

#### Extra part information

To add extra information to a part you can use a reference style link, extra information can be added inside `{}`s in the alt-text:

<pre class="example-block">
{% raw %}
[Large screwdriver]: screwdriver "{Cat: Tool, Note: 'A Pozidriv works best, but you could try a Philips.'}"
{% endraw %}
</pre>

Extra information is specified in key value syntax that is case insensitive, a comma is used before a new key. The following keys are defined:

* `TotalQty` - This sets the total of a part used on a page. It can be use to over-ride the counted value. It can also be used to check all uses are mentioned in the text as Git-Building will produce a warning if they don't match.
* `Cat` - Short for "category". This defines the type of part. Standard BuildUp has two categories `Part` and `Tool`. Parts are counted normally, for tools the maximum quantity used in a single link is used for the bill of materials. Git-Building can define custom categories in its [configuration]({{site.baseurl}}/syntax/buildconfsyntax).
* `Note` - This sets a note which is displayed with the part in the bill of materials.

### Displaying Bill of Materials
{% raw %}
To display a pages bill of materials in the markdown the use `{{BOM}}`. `{{BOM}}` will be replaced in the output with the bill of materials.

For pages with a larger bill of materials it make sense to have it on a separate page. To generate the page, `{{BOMlink}}` generates the page and is replaced with, for example to link to the bill of materials:

<pre class="example-block">
This bill of materials can be found [here]({{BOMlink}})
</pre>
{% endraw %}

### Page steps

Each buildup page can be divided into steps. To do this make an H2 level heading by starting a line with `##` You can then append `{pagestep}` to the line. This serves two purposes, it gives you a sub-heading ID you can refer to in a link, and it also auto numbers the steps. For example

<pre class="example-block">
{% raw %}
## Open the tin {pagestep}
Get the tin and open it with a screw driver.

## Apply paint {pagestep}
Apply paint to the wall
{% endraw %}
</pre>
would render as

<div class="example" markdown="1">
## Step 1: Open the tin

Get the tin and open it with a screw driver.

## Step 2: Apply paint

Apply paint to the wall

</div>

You can then link to this step with `page.md#open-the-tin`. The id for the heading link is contains only the alphanumeric characters in the heading plus `-` and `_`. Spaces are changed to `-`s and all letters int the id are lower case.

You can also redefine the ID, if you replaced the line above with:

<pre class="example-block">
{% raw %}
## Open the tin {pagestep: tin}
{% endraw %}
</pre>

then you could link to the step with `page.md#tin`.

## GitBuilding BuildUp

While GitBuilding is currently the only project using BuildUp we want to make BuildUp into a standard. Long term we would like platforms like [Wikifactory](https://wikifactory.com), [Wevolver](https://www.wevolver.com), and [Instructables](https://www.instructables.com) to support the standard. This way if your documentation is portable.

GitBuilding does a few things which can make documentation more useful, but we don't think they belong directly in the BuildUp standard as other software or platforms will want to work differently. None of the what is detailed bellow changes the BuildUp standard, or adds any extra syntax, instead a certain way of writing BuildUp will cause GitBuilding to tidy up the documentation.

### Image galleries

Sometimes a large group of sequential images looks messy. In GitBuilding if you start a new line and the include multiple images on that same line with no other text it will turn the images into a gallery. For example:

<pre class="example-block">
{% raw %}
Start screwing the gear into the actuator column from the top. Apply a small amount of [light oil]{Qty: "A few drops of"} to the screw thread, before you fully tighten the screw  
![](images/3-5-GearAttach.jpg)![](images/3-6-Oil.jpg)![](images/3-7-GearAttach.jpg)
{% endraw %}
</pre>

will render as:

<div class="example">
<p>Start screwing the gear into the actuator column from the top. Apply a small amount of <a href="#">light oil</a> to the screw thread, before you fully tighten the screw  </p>
<div class="gallery-thumb"><img onmouseover="getElementById('gallery-show3').src=this.src" src="{{site.baseurl}}/images/3-5-GearAttach.jpg" alt="" /><img onmouseover="getElementById('gallery-show3').src=this.src" src="{{site.baseurl}}/images/3-6-Oil.jpg" alt="" /><img onmouseover="getElementById('gallery-show3').src=this.src" src="{{site.baseurl}}/images/3-7-GearAttach.jpg" alt="" /></div>
<div class="gallery-show"><img id="gallery-show3" src="{{site.baseurl}}/images/3-5-GearAttach.jpg" alt=""/></div>
</div>

### STL preview

GitBuilding can do a live 3D preview of STL files. This can be done in one of two ways:

1. A part link to an STL file will a link to a new page with a download link and the STL live preview on it.
1. A link to an STL file that is on its own line will be replaced with a download link and an STL live preview.

An screenshot of the live preview:

<div class="example" markdown="1">
![]({{site.baseurl}}/assets/STL_livepreview.png)
</div>

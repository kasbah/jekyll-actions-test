---
layout: page
title: Syntax
permalink: /syntax/
---

GitBuilding uses a markdown-like syntax called [BuildUp](/syntax/buildup). We plan to formalise BuildUp into a standard once we have figured out what works best.

## Basic example:
{% raw %}

    # Making a widget

    ## You will need
    {{BOM}}

    ## Method
    Take two [M6x10 cap screws](m6capscrew.md){Qty: 2} and screw them into the bottom of the [widget base](base.md){Qty:1} using a [5mm ball driver]{Qty: 1}. 1}.

    [5mm ball driver]: balldriver.md "{cat: Tool}"

    Screw two more [M6x10 cap screws]{Qty: 2} into the top of the [widget base] the [same ball driver][5mm ball driver]{Qty: 1}.

{% endraw %}

Will evaluate to

<div class="example" markdown="1">

# Making a widget

## You will need


### Parts

* 4 x  [M6x10 cap screws]
* 1 x  [widget base]


### Tools

* 1 x  [5mm ball driver]


## Method
Take two [M6x10 cap screws] and screw them into the bottom of the [widget base] using a [5mm ball driver].

Screw two more [M6x10 cap screws] into the top of the [widget base] the [same ball driver][5mm ball driver].

[5mm ball driver]:balldriver
[M6x10 cap screws]:m6capscrew
[widget base]:base

</div >


A full [example is available](https://gitlab.com/bath_open_instrumentation_group/git-building-example).

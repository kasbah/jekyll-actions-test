---
layout: page
title: GitBuilding Software
permalink: /software
---

GitBuilding is a python program which turns pages written in [BuildUp]({{site.baseurl}}/syntax/buildup) into a static website (or into plain markdown). BuildUp a markdown extension that is being developed to add metadata about hardware directly into the instructions. This allows a consistent bill of materials to be generated.

## Installation

To install (for Python 3.6+)

    pip install gitbuilding
    
*Note: If you installed python from the Windows app store Git-Building will not work as intended. If you are on windows you should install [python](https://www.python.org/downloads/) or [Anaconda](https://www.anaconda.com/distribution/). During install make sure you select the box to add python to your PATH*

If you wish to contribute to development of Git-Building you can clone the project from GitLab but you will need to build the javascript editor from source. For more information on contributing see our [contributing guide](https://gitlab.com/gitbuilding/gitbuilding/-/blob/master/CONTRIBUTING.md).

Once the GitBuilding is a bit more stable we will add a simple way to install and run without using the command line.



## Using Git-Building

A Git-Building project can be as simple as a folder with some markdown files in it. To add extra information into these files to allow a bill of materials to be generated we use a syntax we call [BuildUp]({{site.baseurl}}/syntax/buildup). Git-Building will then process the BuildUp and export the result it in either a plain markdown format

When specifying parts in BuildUp they can have their own BuildUp page, or can be part of a [BuildUp part library]({{site.baseurl}}/syntax/builduplibrary) in YAML format. 

It is possible to customise how Git-Building builds the Buildup into plain markdown or HTML by making a [configuration file called buildconf.yaml]({{site.baseurl}}/syntax/buildconfsyntax).

## Running the Git-Building software

All the commands below assume you have successfully installed `gitbuilding` with `pip`.

### Starting a new empty project

Open your terminal in an empty folder you want to use for your documentation and run

    gitbuilding new

empty documentation files will be added to the directory.

### Previewing the documentation and editing in the live editor

GitBuilding has a built in live editor. Open your terminal, navigate to the folder with your documentation, and run

    gitbuilding serve

You can now open a browser and navigate to `http://localhost:6178/`. This will show the documentation in a browsable form, exactly as it is will be output by `build-html` (see below). You can also edit the documentation directly from your browser by selecting `edit` in the top-right corner:

![]({{site.baseurl}}/assets/LiveEditorScreenshot.png)

### Building to standard markdown documentation

To build the documentation in your current folder run

    gitbuilding build

This will create markdown documentation in the directory `_build`. This documentation can then be used anywhere you might usually use markdown.

### Build a static-html site

You can also use GitBuilding to create a static html with

    gitbuilding build-html

This will output your an html website into `_site`.

### Getting help

You can get more detail on the command line options by running

    gitbuilding help

You can get help for a specific command with

    gitbuilding help <command>

If you still have problems please [raise an issue on GitLab](https://gitlab.com/gitbuilding/gitbuilding/issues).

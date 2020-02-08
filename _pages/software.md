---
layout: page
title: GitBuilding Software
permalink: /software
---

GitBuilding is a python program which turns pages written in [BuildUp](/syntax/buildup) into a static website (or into plain markdown). BuildUp a markdown extension that is being developed to add metadata about hardware directly into the instructions. This allows a consistent bill of materials to be generated.

## Installation

To install (for Python 3.6+)

    pip install gitbuilding
    
*Note: If you installed python from the Windows app store Git-Building will not work as intended. If you are on windows you should install [python](https://www.python.org/downloads/) or [Anaconda](https://www.anaconda.com/distribution/). During install make sure you select the box to add python to your PATH*

If you wish to contribute to development of Git-Building you can clone the project from GitLab but you will need to build the javascript editor from source. To do so please see the [dedicated building/contributing page](https://gitlab.com/bath_open_instrumentation_group/git-building/-/blob/master/CONTRIBUTING.md)

Once the GitBuilding is a bit more stable we will add a simple way to install and run without using the command line.



## Using Git-Building

A Git-Building project can be as simple as a folder with some markdown files in it. To add extra information into these files to allow a bill of materials to be generated we use a syntax we call [BuildUp](/syntax/buildup). Git-Building will then process the BuildUp and export the result it in either a plain markdown format

When specifying parts in BuildUp they can have their own BuildUp page, or can be part of a [BuildUp part library](/syntax/builduplibrary) in YAML format. 

It is possible to customise how Git-Building builds the Buildup into plain markdown or HTML by making a [configuration file called buildconf.yaml](/sntax/buildconfsyntax).

## Running the Git-Building software

All the commands below assume you have successfully installed `gitbuilding` with `pip`.

### Starting a new empty project

Open your terminal in an empty folder you want to use for your documentation and run

    gitbuilding new

empty documentation files will be added to the directory.

### Building the documentation

Open your terminal and run

    gitbuilding build

this will build your the documentation in your folder assuming you have a valid `buildconf.yaml` file (see below).

### Previewing the documentation or use the live editor

Open your terminal and run

    gitbuilding serve

and then open a browser and navigate to `http://localhost:6178/`. This will show the documentation in a browseable form. You can also edit the documentation directly from your browser:

![](/assets/LiveEditorScreenshot.png)

### Build a static-html site

Once you have built the documentation with `gitbuilding build` you can convert this into html with

    gitbuilding build-html

### Getting help

You can get more detail on the command line options by running

    gitbuilding help

You can get help for a specific command with

    gitbuilding help <command>

If you still have problems please [raise an issue on GitLab](https://gitlab.com/bath_open_instrumentation_group/git-building/issues).
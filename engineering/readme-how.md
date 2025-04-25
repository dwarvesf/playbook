---
title: null
description: null
date: null
---

# README

A README is a text file that introduces and explains a project. It contains information that is commonly required to understand what the project is about.

## Why we need readme?

It's an easy way to answer questions that your audience will likely have regarding how to install and use your project and also how to collaborate with you.

## Who write readme?

Anyone who is working on a programming project, especially if you want others to use it or contribute. The project lead takes responsiblity to write the readme.

## When write readme?

Definitely before you show a project to other people or make it public. You might want to get into the habit of making it the first file you create in a new project.

## How to write readme?

Every project is different, so content of readme will depend on your targe reader. Here are some sections suggested for a good readme.

### Description

Let people know what your project can do specifically. Provide context and add a link to any reference visitors might be unfamiliar with. A list of Features or a Background subsection can also be added here. If there are alternatives to your project, this is a good place to list differentiating factors.

### Visuals

Depending on what you are making, it can be a good idea to include screenshots or even a video (you'll frequently see GIFs rather than actual videos). Tools like [ttygif](https://github.com/icholy/ttygif) can help, but check out [Asciinema](https://asciinema.org) for a more sophisticated method.

### Installation

Within a particular ecosystem, there may be a common way of installing things, such as using Yarn, NuGet, or Homebrew. However, consider the possibility that whoever is reading your README is a novice and would like more guidance. Listing specific steps helps remove ambiguity and gets people to using your project as quickly as possible. If it only runs in a specific context like a particular programming language version or operating system or has dependencies that have to be installed manually, also add a Requirements subsection.

### Usage

Use examples liberally, and show the expected output if you can. It's helpful to have inline the smallest example of usage that you can demonstrate, while providing links to more sophisticated examples if they are too long to reasonably include in the README.

## Nice to have

### Architecture diagram

If this is a complex application that contains more than one service (micro-services) or involves integrating with several 3rd party services.

### Technical decisions

Why did you chose this library over that 10k-stars-github-repo library? It would be really helpful to new comers as they go through the source code knowing library usages have their own purpose.

### Roadmap

If you have ideas for releases in the future, it is a good idea to list them in the README.

# Example

# Foobar

Foobar is a Python library for dealing with word pluralization.

## Installation

Use the package manager [pip](https://pip.pypa.io/en/stable/) to install foobar.

```bash
pip install foobar
```

## Usage

```python
import foobar

foobar.pluralize('word') # returns 'words'
foobar.pluralize('goose') # returns 'geese'
foobar.singularize('phenomena') # returns 'phenomenon'
```

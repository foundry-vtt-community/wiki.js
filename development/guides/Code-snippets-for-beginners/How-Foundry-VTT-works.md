---
title: EN01A How Foundry VTT works
description: A short explanation concerning the foundry technology
published: true
date: 2022-06-26T09:52:54.623Z
tags: javascript, code, css, handlebars, html, data, technology
editor: markdown
dateCreated: 2022-06-25T22:23:47.608Z
---

# How Foundry VTT works (general)

Well the first step for programming for foundry is to realize what one needs to learn. This on the other hand is dependant on understanding how foundry works and how the different components interact with each other.

The first realization I made, although for an experienced programmer it might seem obvious, is that Foundry VTT is in effect a set of HTML pages. That is the reason you can use it with any (modern) browser you like.

A starting point is:
[Foundry building blocks](https://foundryvtt.com/article/frameworks/)

As a newbie this helped but I neither realized what those framworks are used for nor how the foundry concept really works or interacts. I will thus try to explain how I understand things:

So let's look at the "main" components you probably will encounter first.

1. HTML (DOM)
1. HTML (css)
1. JSON
1. HTML (Handlebars)
1. Javascript (JQUERY)


### HTML (DOM)
The main component is the "web page" or in this case the HTML (DOM) Object. To understand the following you should generally know what object means in a programming context.

A simple web page ist just a structured list of HTML elements.

The HTML DOM object on the other hand includes several different kinds of elements = Objects.

HTML DOM = Document Object Model. 

Each web page (Document) is structured like a tree. HTML elements, data and javascript (Objects) are nested into each other (resulting in a layout = Model) and rendered by the browser.

This **HTML DOM** document can now be divided into 4  components:

1. The HTML elements which are responsible for the general structur or layout of your DISPLAYED document. An analogy would be bones which make up the skeleton of a human.
1. The CSS (cascading style sheets) which defines the look. An analogy would be that it defines hair color, eye color or even muscle proportions for a human. So if I "skin" my HTML bone skeleton differently I can get a male or female looking human. Those humans can look differently also (eycolor, hair etc.).
1. The data defined by the JSON. This (ariable) data is included but not necessarily seen or shown. So to keep the analogy the data would be intelligence or strenght for a human.
1. Javascript which allow us to define what HTML DOM data is shown or how data interacts. Data in this context is a wide term and spans the whole HTML DOM Object. This means it also includes HTML elements.

### HTML (CSS)
CSS is a mighty powerfull tool to "skin" your web page. Over the years it acquired even quite some interactive elements.

In general a HTML page and it's elements are static. Once rendered or once you have defined the structure it does not change anymore. So while rendering the browser has to "find" the correct element and the corresponding CSS description.

To keep the analogy. If you say the color is brown the browser does not know if you mean the skin color or the eye color.

By giving the HTML elements a CLASS Attribute (class="eyecolor") and defining a class "eyecolor" in your css file the browser now knows to make that specific element brown.

Foundry comes with quite some predefined css classes:
https://foundryvtt.wiki/en/development/guides/builtin-css

For more details on css you need to google (you will need it!)
https://www.w3schools.com/css/css_intro.asp

### JSON
JSON is a file format (like XLS, CSV, DOC) that defines the data structure. The Foundry template.json is used for the HTML DOM Object to include the data into the HTML DOM (this does not mean that the data is allready visible!). While it is there what is shown is defined by the HTML structure (more or less ... see Handlebars and Javscript).

### HTML (Handlebars)
Remember that I said HTML is static. Once you defined what paraghraphs, headers and tables or other elements a html document has you cannot change it once it is rendered and displayed.
What you CAN do is change the html definition and render and display it again!
The problem is that html is static. It has no loops, variables or decision making constructs.
That's where Handlebars comes into play. You can define loops and Foundry has helpers included which can make decisions.
So you are able to change the used html elements and thus the html code according to a condition. You can loop trough a skill list and output each skill as an html element without needing to know beforehand how many skills there are.

So handlebars is also one way how you access your data which you definded in your JSON file and which you want to output.

You also are able to write Javascipt code which will extend the list of handlebar helpers which is a function which is called when your html page is put together.

https://handlebarsjs.com/guide/

https://foundryvtt.com/article/frameworks/

### Javascript
So HTML is static but Javascript is the programming language whith which you can access the HTML DOM objects and change them!
With Javascript you can change the layout, "skinning" and the data of your HTML DOM and do some more. You will need Javascript and the foundry object model to implement your game rules, to calculate things and everything else that is to dynamic to do it with HTML elements alone.

## Links
[Foundry JSON Data Structure](https://foundryvtt.com/article/system-development/)

[System-Development-Part-1-I-Made-This](/en/development/guides/System-Development-for-Beginners/System-Development-Part-1-I-Made-This)

[SD01-Getting-started](/en/development/guides/SD-tutorial/SD01-Getting-started)
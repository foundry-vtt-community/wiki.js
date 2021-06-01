---
title: Creating Custom Dialog Windows
description: A short guide on how to create dialog windows with custom content, buttons, and styling.
published: false
date: 2021-06-01T15:03:28.882Z
tags: development, dialog, window
editor: markdown
dateCreated: 2021-06-01T15:03:28.882Z
---

Custom dialog windows are a straightfoward way to interface with users to carry out specific actions or display certain information.
This article aims to walk through the process of creating a custom dialog window from scratch.
This guide is geared towards beginners but can be applied to more complex module development as well.
# The Dialog Class
The `Dialog` class is an extension of the `Application` class. For our purposes, this just means that the foundation of a dialog window is the same as most of the other windows/UI elements you are already familiar with. 

A custom dialog window **requires** the following elements:
* **Title:** What appears in the dialog header.
* **Content:** What appears in the dialog body.
* **Buttons:** Buttons at the bottom of the dialog window.

Additional optional parameters will be covered below. [test](#dialog-options)

To create a custom dialog window (an instance of this class), start with the following:
```js
const myDialog = new Dialog({
	title: "My Custom Dialog Title",
  content: `My custom dialog content`,
  buttons: {}
}).render(true);
```

Note that without including `.render(true)`, the dialog window **will not** appear (render). The above block will not render any buttons since no buttons were defined. Buttons will be covered below. 

Assigning the dialog to a variable (e.g. `myDialog`) is not necessary, but would allow you to render the dialog window later in your code if you desired: `myDialog.render(true)`.

# Content
The body of a dialog window is defined by the `content` property. This is a string of HTML which can be defined during dialog creation as shown above. As it is just HTML, styling can be defined in the content as well:
```js
const myContent = `
	<style>
  	.my-class {
    	color: blue
    }
  </style>
  
  <div class="my-class">
  	This will now appear blue!
  </div>
`
new Dialog({
  tite: "My Custom Dialog Title",
  content: myContent,
  buttons: {}
}).render(true);
```

# Buttons

# Dialog Options

# Factory Methods
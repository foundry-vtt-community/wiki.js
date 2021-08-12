---
title: Creating Custom Dialog Windows
description: A guide on how to create dialog windows with custom content, styling, and buttons.
published: true
date: 2021-08-12T16:04:30.909Z
tags: development, dialog, window
editor: markdown
dateCreated: 2021-06-01T15:03:28.882Z
---

> This is known to be accurate as of Foundry Core 0.8.x
{.is-info}

Custom dialog windows are a straightfoward way to interface with users to carry out specific actions or display certain information.
This article aims to walk through the process of creating a custom dialog window from scratch.
This guide is geared towards beginners but can be applied to more complex module development as well.
# The Dialog Class
The [`Dialog`](https://foundryvtt.com/api/Dialog.html) class is an extension of the [`Application`](https://foundryvtt.com/api/Application.html) class. For our purposes, this just means that the foundation of a dialog window is the same as most of the other windows/UI elements you are already familiar with.

`Dialog` instances are intended for basic interfacing. For more complex data handling, consider using [`FormApplication`](/en/development/guides/understanding-form-applications).

A custom dialog window is comprised of the following elements:
* **Title:** What appears in the dialog header.
* **Content:** The HTML rendered in the dialog body.
* **Buttons:** Buttons at the bottom of the dialog window.

To create a custom dialog window (an instance of this class), start with the following:
```js
const myDialog = new Dialog({
	title: "My Dialog Title",
  content: `My dialog content`,
  buttons: {}
}).render(true);
```

Note that without including `.render(true)`, the dialog window **will not** appear (render). The above block will not render any buttons since no buttons were defined. [Buttons are covered below](#buttons). 

Assigning the dialog to a variable (e.g. `myDialog`) is not necessary but would allow you to render the dialog window later in your code if you desired: `myDialog.render(true)`.

# Content and Styling
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
`;

new Dialog({
  title: "My Dialog Title",
  content: myContent,
  buttons: {}
}).render(true);
```

## Rendering Templates
While content HTML can be defined manually as above, a popular method for creating HTML content is through the use of templates. This requires a separate `.html` or `.hbs` file and for the template rendering to be called in an async function:
```js
const myContent = await renderTemplate("modules/myModule/templates/myTemplate.html", data);

new Dialog({
  title: "My Custom Dialog Title",
  content: myContent,
  buttons: {}
}).render(true);
```

# Buttons
Dialog buttons are comprised of the following elements:
* **Label:** The text to be appear on the button.
* **Callback:** The action to be carried out once the button is clicked.
* **Icon:** The icon to appear before the label (optional).

Each button is defined as an object within the `buttons` property:

```js
new Dialog({
  title: "My Dialog Title",
  content: "My dialog content",
  buttons: {
  	button1: {
      label: "Button #1",
      callback: () => {ui.notifications.info("Button #1 Clicked!")},
      icon: `<i class="fas fa-check"></i>`
    },
    button2: {
      label: "Button #2",
      callback: () => {ui.notifications.info("Button #2 Clicked!")},
      icon: `<i class="fas fa-times"></i>`
    }
  }
}).render(true);
```

## Callbacks
Each button has a callback function which is run when the button is clicked. This function can be defined during dialog creation as above, or can be a set to a pre-defined function:
```js
function myCallback() {
	ui.notifications.info("Button #1 Clicked!");
}

new Dialog({
  title: "My Dialog Title",
  content: "My dialog content",
  buttons: {
  	button1: {
      label: "Button #1",
      callback: () => myCallback(),
      icon: `<i class="fas fa-check"></i>`
    }
  }
}).render(true);
```

Note that the callback **must** be provided in the form: `() => {}` or `myCallback.bind(this)`.

As with any function, callbacks can be provided arguments to be used during their execution. A helpful argument is `html`, which provides the callback with the HTML content of the dialog at time of execution (when the button is clicked). This content is provided in a `jQuery` object (as of core version `0.8.6`):

```js
const myContent = `
	Value:
  <input id="myInputID" type="number" value="0" />
`;

new Dialog({
  title: "My Dialog Title",
  content: myContent,
  buttons: {
  	button1: {
      label: "Display Value",
      callback: (html) => myCallback(html),
      icon: `<i class="fas fa-check"></i>`
    }
  }
}).render(true);

function myCallback(html) {
	const value = html.find("input#myInputID").val();
  ui.notifications.info(`Value: ${value}`);
}
```

A special type of callback that is not associated with a button is the `close` callback. This function is called when the dialog window closes, no matter how this closure was initiated (via a button click, escape key press, or close header button click):
```js
new Dialog({
  title: "My Dialog Title",
  content: "My dialog content",
  buttons: {},
  close: () => {ui.notifications.info("My dialog was closed!")}
}).render(true);
```

A similar operation can be done when the dialog is rendered using `render`:
```js
new Dialog({
  title: "My Dialog Title",
  content: "My dialog content",
  buttons: {},
  render: () => {ui.notifications.info("My dialog was rendered!")}
}).render(true);
```

## Icons
Icons can be selected from fontawesome.com. Simply copy the HTML provided at the top of each icon page.

## Default Button
A button can be assigned as "default" meaning it will be "clicked" when the enter key is pressed:
```js
new Dialog({
  title: "My Dialog Title",
  content: "My dialog content",
  buttons: {
  	button1: {
      label: "Button #1",
      callback: () => {ui.notifications.info("Button #1 Clicked!")},
      icon: `<i class="fas fa-check"></i>`
    },
    button2: {
      label: "Button #2",
      callback: () => {ui.notifications.info("Button #2 Clicked!")},
      icon: `<i class="fas fa-times"></i>`
    }
  },
  default: "button1"
}).render(true);
```

Note that the value for the `default` key is a string.
# Dialog Options
As with all `Appliaction` instances, additional options can be provided during dialog creation to adjust parameters such as window size and position:
```js
const myDialogOptions = {
  width: 200,
  height: 400,
  top: 500,
  left: 500
};

new Dialog({
  title: "My Dialog Title",
  content: "My dialog content",
  buttons: {}
}, myDialogOptions).render(true);
```

For a full list of options, see the `Application` class in `foundry.js`.

# Factory Methods
The `Dialog` class provides two built-in helper methods to quickly create dialogs.
Note that these are static methods of the `Dialog` class, and so they are called on `Dialog` itself, not a new instance of the class (i.e. do not use `new`).

## Confirm
```js
Dialog.confirm({
  title: "My Confirm Dialog Title",
  content: "My confirm dialog content",
  yes: () => {ui.notifications.info("Yes was pressed!")},
  no: () => {ui.notifications.info("No was pressed!")},
  defaultYes: false
});
```

## Prompt
```js
Dialog.prompt({
	title: "My Prompt Dialog Title",
  content: "My prompt dialog content",
  label: "My Prompt Button Label",
  callback: () => {ui.notifications.info("Prompt button pressed!")}
});
```
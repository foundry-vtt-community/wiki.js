---
title: Importing a .JS File in Foundry VTT
description: 
published: true
date: 2021-01-15T19:32:45.424Z
tags: 
editor: undefined
dateCreated: 2020-09-23T00:34:13.915Z
---

Given the vast amount of information out there about ES6 modules it's easy for someone new to JS to get lost trying to figure out how to carry out the simple task of importing a `.js` library into Foundry VTT. Because the process is simple it is something often overlooked in installation explanations.

### How do I add a third party `.js` plugin or library so that I can call it inside my character sheet?
Foundry VTT supports ES6 Modules, so the simplest way to import your `.js` file is to establish an export for it.

Using Example (`example.min.js`) constructor class,

1. Prepare your .js as an export class.
	1. Open your example.min.js (note: if your .js already begins with `export class ...` you can skip the next step)
	2. Wrap the entire `.js` file in `export class <classname> {AllTheFunctionsGoHere}`

2. Define your import
	1. Open your primary modules file (the one defined as "Modules": in your system.json),
	2. at the top you will see something that looks like:

	```js
	// Import Modules
	import { classname } from "./module/yourclasshere.js"
	```

	3.follow this formatting to import your class. Save.

3. Your JS file is now imported! 

### But how do I use it in the HTML for my character sheet? 

Any script you would normally add as `<script>` in your HTML file you can add in the extended `ActorSheet.js` by using the function 

```js
activateListeners(html) {
	// your script here
}
```

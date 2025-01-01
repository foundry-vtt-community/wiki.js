---
title: CSS Cascade Layers
description: A guide to the CSS Cascade Layer system introduced in Foundry VTT v13
published: true
date: 2025-01-01T13:29:15.586Z
tags: 
editor: markdown
dateCreated: 2024-12-31T20:13:49.920Z
---

> Page is known to be up to date with Core Version 13.334
{.is-info}

## TL;DR

CSS Layers are a new part of the CSS standard which allows CSS authors to take more active control of the order in which CSS rules are applied. This makes it easier to isolate certain groups of rules to certain parts of the page.

This is very useful for Foundry VTT module and system authors, who often need to isolate heavily styled app windows from Foundry's built-in styles.

## Is this a breaking change in FVTT?

Short answer: no.

The core team introduced layers a while ago and then backed them out because the initial implementation broke some existing packages because of styles which were previously not overrides core suddenly being elevated in precedence.

They have been reintroduced in Foundry VTT V13, with the addition of a `compatibility` layer which means that anything using V1 styles should stay the same. 


## Reading & References

* [GitHub ticket for adding layers to Foundry's CSS][gh-layers-ticket]
* [MDN Reference on CSS Cascade Layers][mdn-cascade]
* [MDN Reference on the @layer CSS rule][mdn-at-layer]
* [MDN Reference on the `revert-layer` value][mdn-revert-layer]

## Background

CSS was originally designed to write styles for a document where a single author would be in control of the whole thing. Increasingly though, web apps may contain multiple parts managed by different teams, and in the past it's been a challenge to prevent cascade conflicts.

A cascade conflict occurs when multiple CSS rules target the same element with different style declarations. CSS uses specificity rules to determine which styles "win" - but when multiple stylesheets are involved (like Foundry's core styles and a module's styles), these conflicts can lead to unexpected results or require increasingly specific selectors to override existing styles.

This would manifest as module authors having to use "specificity hacks" to make their styles "win" against the Foundry built-ins.

Starting in Foundry v13, Foundry's built-in styles are now organised into CSS layers.


## Predefined layers

Currently the best *original* documentation for Foundry's CSS layers is [this Github Ticket.][gh-layers-ticket] It lists ten layers:

```css
@layer reset,           /* 1. Browser reset styles */
       variables,       /* 2. Declare framework variables */
       elements,        /* 3. HTML elements and typography */
       blocks,          /* 4. Block styles which are used across applications */
       applications,    /* 5. Application-specific rules */
       compatibility,   /* 6. V1 compatibility layer */
       layouts,         /* 7. Page layouts */
       system,          /* 8. Default game system styles */
       modules,         /* 9. Default module styles */
       exceptions;      /* 10. Special exceptions */
```


In practice though, there are multiple nested layers in use, plus some others that are not mentioned in that ticket. The full list layers that Foundry v13.334 defines is as follows. Note that this list is in reverse declaration order, so the first layer is the highest priority. Personally I find this this order makes more sense.

```
base                          △  highest priority / declared last
specific                      |
general                       |
exceptions                    |
  exceptions.prosemirror      |
modules                       |
system                        |
layouts                       |
  layouts.responsive          | nested layers have lower priority than parents
  layouts.views               | even if declared later
  layouts.full                |
compatibility                 |
applications                  |
blocks                        |
  blocks.ui                   |
  blocks.base                 |
elements                      |
  elements.custom             |
  elements.forms              |
  elements.media              |
  elements.typography         |
variables                     |
  variables.themes            |
  variables.base              |
reset                         ▽  lowest priority / declared first
```

`general`, `specific`, and `base` are not mentioned in the ticket, but are used in the core Foundry CSS. They are probably not intended for use by developers.


## How do I use CSS layers in my module or system

First of all: **you don't have to do anything manually unless you have an unusual use-case**.

If you include your CSS the normal way, by listing it in your manifest (`module.json` or `system.json`), Foundry v13 and later will automatically import it into either the `module` or the `system` layer, as appropriate. If you look at the list of layers above, you can see that `system` has priority over all of Foundry's built-in styles, and `module` has priority over `system` (and therefore also over Foundry's built-ins.)

For example, putting this in your `module.json` will add your CSS to the `module` layer without any more intervention from you:

```json
"styles": ["mymodule.css"],
```

This one change means that your system or module styles will always beat conflicting styles that are built-in, with no extra work from you.

## But I don't want to use the `module` or `system` layer

Congratulations, you can use use object syntax to import your CSS into any layer. For example, in `module.json`, this would import `mymodule.css` into the `module` layer (as above) but add `myvariables` into the `variables` layer:

```json
"styles": [
  "mymodule.css",
  {
    "src": "myvariables.css",
    "layer": "variables"
  }
],
```

## Opting out of Foundry's automatic layers entirely

If you want to take full control of the layers used in your CSS, you can use the `null` as the value for `layer`. The stylesheet will not be added to any layer, and will always take precedence over all layered rules, or you can use the `@layer` rule to add it to a specific layer.

```json
"styles": [
  {
    "src": "myverycleverstylesheet.css",
    "layer": null
  }
],
```

### The (not very) dreadful consequences of not using layers at all

If you do use `"layer": null`, and don't manually wrap your CSS in `@layer {...}` rules, your CSS will all end up in the default/unlayered group, which takes precedence over all layered rules. So it will "work", but it will not play nicely with code that expects layers. Specifically, your CSS will have precedence over the `exceptions` layer, so nobody will be able to use that layer to override your CSS. Also, if your package is a system, its CSS will have precedence over modules (the intent is that modules can override systems.)

See the [MDN documentation on `@layer` rules][mdn-at-layer].


## Reverting a layer

One of the benefits of using CSS layers is that authors can now specifically revert a particular layer within a particular context.

For example, in the core foundry CSS, `h1`, `h2` etc. are defined as having a color based on a CSS variable:

```css
/* foundry2.css (abridged) */
@layer elements.typography {
  h1 {
    color: var(--color-text-emphatic);
  }
}
```

However in my system, I want `h1`, `h2` etc to simply inherit their color from their parent.

I can solve this in a few ways.

I *could* simply put this rule in my CSS:

```css
.application.my-system .window-content h1 {
  color: inherit;
}
```

This would go into the `system` layer and override the rule in `foundry2.css`, above.

However - having looked at the other definitions in `@layer elements.typography`, I've decided that I don't want any of it. I am handling my own typography, and having anything from core show up in my window is going to break stuff. I can use the `revert-layer` value in conjunction wit the `all` shorthand property, so just completely opt out of `@layer elements.typography` in my application windows. There's a few ways you could handle this, but personally I would make a stylesheet called `revert.css`, and load it using `"layer": null` as shown above. Then you can have full control of layers and use it to revert `@layer elements.typography` like this:

```css
@layer elements.typography {
  .application.my-system .window-content {
    &, & * {
      all: revert-layer;
    }
  }
}
```

(You could skip the `@layer elements.typography {` declaration, and then import it using `"layer": "elements.typography"` in the manifest - but then you'll only be able to revert that single layer. Doing it my way means you can go on an absolute layer killing spree in one file.)

To break down that CSS:

```css
@layer elements.typography {
```

We're injecting rules into the elements.typogrpahy layer. Named layers are always open for adding, so even though the layer doesn't belong to us, we can still add to it.

```css
  .application.my-system .window-content {
```

We're selecting for the window content of applications that belong to our system. We don't want to clobber the styles elsewhere, just in our windows.

```css
    &, & * {
```

A compound rule using the [CSS nesting selector][mdn-nesting-selector]. `&` just means "whatever the outer selector is, so that's `.application.my-system .window-content`.

`& *` is the same but with a descendant splat - so this rule means anything inside `.application.my-system .window-content`.

So the overall meaning of `&, & *` here is `.application.my-system .window-content` *and* anything inside it.

```css
      all: revert-layer;
```

The [`all`][mdn-all] shorthand property means "every single css property".

[`revert-layer`][mdn-revert-layer] means that the values of those properties goes back to whatever they were in the layer below the current one (which is `elements.typography`) &mdash; so that's `variables`. In this case that effectively reverts to the browser default style, which for the `color` of `h1` is `inherit`.


## Reverting nested layers

Just a side note: you *may* be tempted to think that if you revert the `elements` layer it will automatically revert all the child layers of `elements`. That is not, sadly, how it works. If you revert `elements` it only reverts the rules that are directly in `elements`, so you're effectively  reverting to `elements.custom`. You need to manually revert each nested layer, as shown in the example above.


## If you're importing your CSS in some non-standard way

If you're loading CSS in a weird way, like bundling it with a Vite or similar, you will need to take charge of putting it into layers yourself.

As noted above, if you don't put your CSS into a layer, its automatically put into an "unlayed group" that always takes precedence over layered rules, which may not be what you want.


## Further reading

[MDN Reference on Cascade Layers][mdn-cascade]

[gh-layers-ticket]: https://github.com/foundryvtt/foundryvtt/issues/6842
[mdn-cascade]: https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Styling_basics/Cascade_layers
[mdn-at-layer]: https://developer.mozilla.org/en-US/docs/Web/CSS/@layer
[mdn-all]: https://developer.mozilla.org/en-US/docs/Web/CSS/all
[mdn-revert-layer]: https://developer.mozilla.org/en-US/docs/Web/CSS/revert-layer
[mdn-nesting-selector]: https://developer.mozilla.org/en-US/docs/Web/CSS/Nesting_selector
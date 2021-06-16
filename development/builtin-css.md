---
title: Foundry's Built-in CSS Framework
description: An overview of the built-in CSS styles used and provided by Foundry
published: false
date: 2021-06-16T23:48:33.866Z
tags: css
editor: markdown
dateCreated: 2021-06-16T23:48:33.866Z
---

# Foundry's style.css

Out of the box, Foundry provides a sizeable CSS framework. This article is intended to provide an overview of this framework so that system and module developers can leverage it - or avoid colliding with it - in their customizations.

## Viewing the CSS

To obtain or view the main `style.css` file:

1. In your Foundry installation, the file can be found at `$installDirectory/resources/app/public/css/style.css`
1. Using the Style editor in your browser's developer console, locate the file named `style.css`:
![foundrycss.png](/development/foundrycss.png)

## Flexbox Classes

Foundry provides a few general classes for employing CSS flexbox in your layouts.

> If you're not familiar with flexbox, CSS Tricks has a solid guide [here](https://css-tricks.com/snippets/css/a-guide-to-flexbox/).
{.is-info}


### `flexrow`

The `flexrow` class can be used on an HTML component so that it lays out its children horizontally.

### `flexcol`

The `flexcol` class can be used on an HTML component so that it lays out its children vertically.

### `flex0`

### `flex1`, `flex2`, `flex3`

# Font Awesome

Foundry also includes the free Font Awesome icon font set.

`TODO: Example`

# Helper Utilities

## Class and ID extraction

Discord user danielrab#7070 in the League server wrote a python script to extract all IDs and classes used by the Foundry CSS.

The original message can be found [here](https://discord.com/channels/732325252788387980/734755256524865557/854858038639591464).

The python script:

```python
import re

with open('style.css', 'r') as file:
    data = file.read()
print(data)
data = re.sub(r'\{[^{}]*\}', '', data)
data = re.sub(r'/[^\n]*\n', '', data)
data = re.sub(r'@[^\n]*\n', '', data)
data = re.sub(r',\n', '\n', data)

ids = sorted(list(set(re.findall(r'#([^\s#.]+)', data))))
classes = sorted(list(set(re.findall(r'\.([^\s#.]+)', data))))

with open('used ids.txt', 'w') as file:
    for i in ids:
        file.write(i+'\n')
with open('used classes.txt', 'w') as file:
    for c in classes:
        file.write(c + '\n')
```

and its output as of v0.8.7 on 16 June 2021:

* [used_ids.txt](/development/used_ids.txt)
* [used_classes.txt](/development/used_classes.txt)

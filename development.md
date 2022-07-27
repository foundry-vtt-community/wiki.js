---
title: Development
description: The development section
published: true
date: 2022-05-19T13:21:03.159Z
tags: 
editor: markdown
dateCreated: 2020-10-19T14:47:03.038Z
---

This page acts as a directory for the Development section of the wiki.

## Basics and Best Practices

### [Getting Started with Package Development](/en/development/getting-started-with-development)
Our big page of FAQs and starting out Gotchyas.

#### Topics
- Is there hook documentation?
- How do I extend Foundry Core behavior without a hook?
- How do I make an API available to other modules?
- What is a Flag/Setting?
- My development package doesn't show up in Foundry!
- Common JS terminology


### [Package Development Best Practices Checklist](/en/development/guides/package-best-practices)
A checklist of best practices in the Foundry VTT Package Development space.

#### Topics
- Release and Update conventions
- Localization
- Working with Files and Imports
- Module APIs and Dependencies
- Common Code Practices
- Overwriting Foundry Core Behavior


### [Package Releases and Version History](/en/development/guides/releases-and-history)
A deep dive about how Foundry VTT handles installing and updating packages with recommendations and best practices.

#### Topics
- How does Foundry know which version of my package to install?
- Are users able to update to incompatible package versions?
- How do I ensure that users can install a previous version of my package?

---

## Foundry Development Core Concept Guides

### [Handling Data: Flags, Settings, and Files](/en/development/guides/handling-data)
A guide dedicated to three methods of storing data from a module's perspective.

#### Topics
- Data Storage decision flowchart
- CRUD for Flags
- CRUD for Settings
- Settings Menus
- JSON and FileUpload


### [Understanding FormApplication](/en/development/guides/understanding-form-applications)
The powerhouse of Foundry's UI framework, FormApplication has a lot of nuances discussed in this guide.

#### Topics:
- Dialogs vs FormApplications
- What can a FormApplication do?
- Defining and using a FormApplication subclass
- FormApplication options


### [Creating Custom Dialog Windows](/en/development/guides/creating-custom-dialog-windows)
An in-depth guide to using the Dialog UI class.

#### Topics:
- Setting the Content
- Styling a Dialog
- Defining Buttons for input



### [Active Effects Primer](/en/development/guides/active-effects)
A page dedicated to explaining the concepts behind the Active Effect API.

---

## I need help with 0.8 specifically

### [Migration Summary for 0.8.x](/en/migrations/foundry-core-0_8_x)
We've compiled a lot of the most asked questions with some answers in this guide to working with the 0.8.x Document API.

#### Topics
- What is a Document?
- How do I CRUD Documents?
- What are the main diffs between 0.7.x and 0.8.x?



## Contribution Guide

### Goals of /Development

This section does not aim to replace the official API documentation.

#### On Topic

Package development related guides and resources, and specific API examples.

#### Off Topic

Lists of packages, recommended modules not related to developing modules.

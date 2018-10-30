---
title: Globals
overview: >
  Globals are groups of variables made available everywhere in your templates, all the time, and are never tied to any one specific page, entry, or URL.
related_reading:
  - 4da38f4b-0154-42ec-873e-35a5bfbafc79
  - 767fe16a-406f-450b-840b-a7c34ebba465
id: e8092f11-c3b4-477a-a3a4-9c5e65f45cc4
---
## Overview

Globals are designed for reusable content, such as:

- Company info
- Footer and sidebar content
- Customer quotes or testimonials
- Site settings (e.g. on/off switches for various features)
- Success and/or error message text

You can create multiple Globals sets in `site/content/globals`, or `Configure » Content » Globals` in the control panel.

Variables set inside your default set (`global.yaml`) will simply be available in all your templates by their variable name. Each set/file becomes its own scope, allowing you to group your Globals together logically. For example, all of the variables inside a `footer.yaml` file will be accessed through `{{ footer:var_name }}`.

## Managing Globals

Each Global set will need its own [Fieldset](/fieldsets) to define what fieldtypes you want to use to edit the data.

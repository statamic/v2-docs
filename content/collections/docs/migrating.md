---
title: Migrating From Statamic v1
id: 94dd66f9-d0cd-40c8-b723-d2e2c90c9af5
overview: >
  We made a number of breaking changes, every one of them on purpose. We will now justify those decisions, get you to believe in them, and then show you how to bring your v1 site up to v2.
---

## Improvement Requires Change {#change}

We spent over 3 years growing v1, learning what our users needed, and maintaining backwards compatibility at every turn. We never made a [semantic][semver] 2.0 update (even though maybe we should have for marketing purposes) that would break anything.

Not changing how anything works on the end-user level meant living with naming mechanisms that we grew out of, organization structures that became brittle, and technical limitations and debt that became looming harbingers of doom. Feature Requests began to feel like [TPS Report](https://www.youtube.com/watch?v=PPnMt5UbAD4) assignments. When you lose the ability to change quickly and with confidence you lose the ability to scale and grow alongside your users.

For that reason, we rewrote everything for v2 and took the opportunity to analyze every feature, every tag name, every fieldtype, every file and folder -- to make sure they fit together consistently, naturally, and intuitively. Our codebase is modern, flexible, testable (and tested), and is ready for the long haul.

From here out we will explain what has changed, why, and what you'll need to do to upgrade your v1 site. Most of these things can be done with a project-wide find & replace (everything is _still_ files after all), or some dragging, dropping, and renaming of files.

## Recommended Upgrade Approach {#approach}

We recommend starting with a fresh Statamic 2 install and moving your content, theme, fieldsets, and users over group by section. It'll help to focus on which areas need changes and isolate any unexpected behaviors.

The best way to move your content is through is probably to use the [Statamic v1 Exporter][exporter]. It will generate a JSON representation of all of your content (pages, entries, globals, and taxonomies) and let you use v2's Importer to generate all the right files in the right places.

The v2 Importer can be found in the Control Panel, in the `Tools` section.

## Breaking Changes {#breaking-changes}

Each of these changes may require a little bit of work. You'll want to check out the [folder tour](/guides/getting-started#folder-tour) to get acclimated, and then dive into the following changes.

### File and Folder Naming Conventions {#file-and-folder}

- Filename order keys are **always** delimited with a period before the slug. For example: `2015-12-25.christmas.md` and `2.about/index.md`. You can use a tool like [Name Mangler][name-mangler] to batch all these at once _and_ confirm that the output is right before you do it.
- Rename `page.md` files to `index.md`.
- Convert any pages that aren't in the `folder/index.md` format (e.g. `/about/team.md` becomes `/about/team/index.md`).

### Entries Move into Collections {#entries-to-collections}

- Move entries into their own [Collection][collection] and configure their [route][routes].
- Mount them to their corresponding pages by adding `mount: collection_name` to the appropriate `index.md`.
- Move `fields.yaml` from the page's to the collection's folder, and name it `folder.yaml`

### Renamed Variables {#renamed-variables}

- `{{ layout_content }}` is now `{{ template_content }}` because it's the actual contents of the template. Feels so much more natural.
- ``{{ current_time }}``

### Removed Variables {#removed-variables}

- `{{ environment }}`
- `{{ _is_draft }}`
- `{{ _is_hidden }}`
- `{{ taxonomy_name }}`
- `{{ taxonomy_slug }}`

### Setting and Variable Naming Conventions {#settings-and-variables}

You know all those things that start with an underscore? `_site_root`, `_layout`, `_template`. We grew to hate those blasted underscores. They're all gone. All of them, everywhere. They're replaced with non-underscored versions of themselves. We now have some system protected words. Don't name your variables any of these words:

- `template`
- `layout`
- `fieldset`
- `slug`
- `attributes`
- `parse_content`
- `first`
- `last`

### Removed and Replaced Fieldtypes {#removed-fieldtypes}

The former `templates` fieldtype is now named `template`, singular.

The Location fieldtype, tag, and `{{ entries:meld }}` tag have been removed. We feel it's important to eliminate reliance on third-party APIs for all core features. The maps HTML from v1 came from [Leaflet.js][leaflet] and was really just a wrapper for their default implementation. Check it out, it's pretty straightforward.

The File fieldtype is replaced by [Assets](/reference/fieldtypes/assets). As long as you don't move your files, your links won't break, but you will lose your Control Panel connection to them. Check out the [Assets guide][assets] to learn about the new and improved ways.

Status is gone. We now have the notion of "published" and "unpublished". Unpublished content is represented by files beginning with an _underscore.


### Removed Modifiers {#modifiers}

The following modifiers were removed because they were rarely, if ever used and only served to make scanning the docs tedious. Feel free to copy and paste them out of v1 and release them as free addons. You have our 100% permission.

- abs
- acos
- asin
- atan
- censor
- cos
- decbin
- dechex
- decoct
- deg2rad
- distance_in_km_from
- distance_in_mi_from
- double
- hexdec
- log
- log10
- octdec
- ordinal
- rad2deg
- rot13
- sin
- spaced_list
- sqrt
- tan
- triple

### Addons {#addons}

Addons are structured pretty differently in v2. Chances are [porting one][addons] won't prove to be very difficult, and may even be an enjoyable experience, but you can't just drop a v1 addon into v2. At best nothing will happen. At worst, a [wampa](http://starwars.wikia.com/wiki/Wampa) will try to eat you.

### The Control Panel {#control-panel}

The [Control Panel](control-panel) is now accessible at `/cp` instead of `admin.php`. You can rename that in your `index.php` file.


## New Defaults {#new-defaults}

### Required Fields {#fields}

- The `title` field is no longer required when working with files only.
- The `content` field is no longer required. If you choose to ignore the `content` field, the `---` YAML Front Matter delimiters are removed automatically for your (and our) convenience. It will be a straight YAML file.

### Settings {#settings}

Settings are now broken out by area of responsibility, and can be found in `site/settings`. You can explore them all in the Control Panel with descriptions, help text, and a nice interface. Of course, you can do it all in files still too, just like v1.

## Better Ways of Doing Things {#better-ways}

There are ways of doing things in v2 that are streets ahead of v1. You aren't by any means required to change your logic, but if you choose to do so, you'll be glad you did. Don't let yourself get streets behind. Here's a summary of those things.

### Content Types {#content-types}

- Taxonomies, globals, and assets are first class citizens now; they are full-on [Content Types][content-types-guide] with their own customizable fields.
- Just about everything is multilingual now, and it's pretty darn neat.

### Relationships {#relationships}

- All content gets a unique id: `id`. If you manually create a file, one will get added for you automatically. These ids let you create unbreakable relationships between various bits of data. It's neat. Also, don't remove them from your files.
- The new [Relate Fieldtypes][relate-fieldtypes] replaces Suggest and provides an all-in-one relationship management interface, pairing nicely with the [Relate Tag][relate-tag] and `| get` Modifiers.

### Users {#users}

- Users can log in with email address in addition to username.
- Users can be organized into Groups.
- User groups can have Roles.
- User roles can have [Permissions][permissions].
- Permissions let you decide which users get to do which thing, like delete pages, manage settings, or run the Updater.

### Templates {#templates}

- Tags now have optional scopes you can use to eliminate any conflict with The Cascade.
- Entries Listing and Collection tags can use a child variable alias (e.g. `{{ entries }}`) to let you more easily manage your markup.
- There are a lot more modifiers and an easier to use syntax. There's a good chance you can simplify your templates and make them more readable.
- You know what? Just read through the whole [Template Language guide][template-language-guide]. You'll be glad you did.

### Search {#search}

[Search][search-guide] is Bloodhound's cooler cousin. Search automatically indexes for performance. Search is more easily configured. Search connects to [Algolia](https://algolia.com) in a matter of seconds. Search is included in core. What more can you ask for? (Besides keyword weighting. Search doesn't do keyword weighting.)


[addons]: /guides/addons
[assets]: /guides/assets
[collection]: /guides/content-types#collections
[content-types-guide]: /guides/content-types
[email]: mailto:gentlemen@statamic.com
[leaflet]: http://leafletjs.com/
[name-mangler]: https://manytricks.com/namemangler/
[permissions]: /guides/permissions
[relate-fieldtype]: /reference/fieldtypes/relate
[relate-tag]: /reference/tags/relate
[routes]: /guides/urls-and-routing#routes
[search-guide]: /guides/search
[semver]: http://semver.org
[template-language-guide]: /guides/template-language
[trading-post]: https://trading-post.statamic.com
[users]: /
[exporter]: http://github.com/statamic/exporter

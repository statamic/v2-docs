---
title: "You Can Get Here From There"
nav_title: Upgrading From v1
id: 87bff107-cf81-472d-81a7-fd9eb90f0487
cover: /assets/img/trail-guides/upgrading.jpg
description: >
  Learn how to upgrade your v1 site to v2 and see what's new while you're at it.
overview: >
  **Confession:** We made bunch of breaking changes on purpose. We will now justify those decisions, get you to believe in them, and then show you how to bring your v1 site up to v2.
---
## Improvement requires change

We spent over 3 years growing v1, learning what our users needed, and maintaining backwards compatibility at every turn. We never made a [semantic][semver] 2.0 update (even though maybe we should have for marketing purposes) that would break anything.

Not changing how anything works on the end-user level meant living with naming mechanisms that we grew out of, organization structures that became brittle, and technical limitations and debt that became looming harbingers of doom. Feature Requests began to feel like [TPS Report](https://www.youtube.com/watch?v=PPnMt5UbAD4) assignments. When you lose the ability to change quickly and with confidence you lose the ability to scale and grow alongside your users.

For that reason, we rewrote everything for v2 and took the opportunity to analyze every feature, every tag name, every fieldtype, every file and folder -- to make sure they fit together consistently, naturally, and intuitively. Our codebase is modern, flexible, testable (and tested), and is ready for the long haul.

From here out we will explain what has changed, why, and what you'll need to do to upgrade your v1 site. Most of these things can be done with a project-wide find & replace (everything is _still_ files after all), or some dragging, dropping, and renaming of files.

## Recommended upgrade approach {#approach}

We recommend starting with a fresh Statamic 2 install and moving your content, theme, fieldsets, and users over group by section. It'll help to focus on which areas need changes and isolate any unexpected behaviors.

## Breaking changes {#breaking-good}

Each of these things may require a little bit of work. We've estimated that the average site takes about 30 minutes to upgrade. You'll want to check out the [folder tour](/guides/getting-started#folder-tour) to get acclimated, and then dive into the following changes.

### File and folder naming conventions

- Filename order keys are **always** delimited with a period before the slug. For example: `2015-12-25.christmas.md` and `2.about/index.md`. You can use a tool like [Name Mangler][name-mangler] to batch all these at once _and_ confirm that the output is right before you do it.
- Rename `page.md` files to `index.md`.
- Convert any pages that aren't in the `folder/index.md` format (e.g. `/about/team.md` becomes `/about/team/index.md`).

### Entries move into Collections

- Move entries into their own [Collection][collection], mount them to their corresponding pages, and configure their [route][routes].

### Renamed/removed variables

- `{{ layout_content }}` is now `{{ template_content }}` because it's the actual contents of the template. Doesn't that feel better?
- `{{ _xml_header }}` is no longer necessary to render XML headers. Instead, use the `.xml` extension on your URL.

### Setting and variable naming conventions

You know all those things that start with an underscore? `_site_root`, `_layout`, `_template`. We grew to hate those blasted underscores. They're all gone. All of them, everywhere. We now have some system protected words. Don't name your variables any of these words:

- `template`
- `layout`
- `fieldset`
- `slug`
- `attributes`
- `parse_content`
- `first`
- `last`

### Bloodhound is now integrated Search

[Search][search-guide] is Bloodhound's cooler cousin. Search automatically indexes for performance. Search is more easily configured. Search is included in core. What more can you ask for? (Besides keyword weighting. Search doesn't do keyword weighting.)

### Removed Location fieldtype and tags

The Location fieldtype, tag, and `{{ entries:meld }}` tag have been removed. We feel it's important to eliminate reliance on third-party APIs for all core features. The maps HTML from v1 came from [Leaflet.js][leaflet] and was really just a wrapper for their default implementation. Check it out, it's pretty straightforward.

### Removed modifiers

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

### Addons

Addons are structured pretty differently in v2. Chances are [porting one][addons] won't prove to be very difficult, and may even be an enjoyable experience, but you can't just drop a v1 addon into v2. At best nothing will happen. At worst, a [wampa](http://starwars.wikia.com/wiki/Wampa) will try to eat you.

## New Defaults {#new-defaults}

### Required fields

- The `title` field is no longer required or included by default in your fieldsets.
- The `content` field is no longer required or included by default.
- If you choose to skip the `content` field, the `---` YAML Front Matter delimiters are removed automatically for your (and our) convenience.
- The Control Panel is now accessible at `/cp` instead of `admin.php`. You can rename the route your `index.php` file.

### Files and Assets

- The File fieldtype is replaced by [Assets](/reference/fieldtypes/assets). As long as you don't move your files, your links won't break but you'll probably want to use because frankly, it's a lot better. Check out the [Assets guide][assets].

## Better Ways of Doing Things {#better-ways}

There are ways of doing things in v2 that are streets ahead of v1. You aren't by any means required to change your logic, but if you choose to do so, you'll be glad you did. Don't let yourself get streets behind. Here's a summary of those things.

### Content Types

- Taxonomies, globals, and assets are first class citizens now; they are full-on [Content Types][content-types-guide] with their own customizable fields.
- Just about everything is multilingual now, and it's pretty darn neat.

### Relationships

- All content gets a unique id: `id`. If you manually create a file, one will get added for you automatically. These ids let you create unbreakable relationships between various bits of data. It's neat. Also, don't remove them from your files.
- The new [Relate Fieldtype][relate-fieldtype] replaces Suggest and provides an all-in-one relationship management interface, pairing nicely with the [Relate Tag][relate-tag].

### Users

- Users must log in with email address instead of username.
- Users can be organized into Groups.
- User groups can have Roles.
- User roles can have [Permissions][permissions].
- Permissions let you decide which users get to do which thing, like delete pages, manage settings, or run the Updater.

### Templates
- Tags now have optional scopes you can use to eliminate any conflict with The Cascade.
- Entries Listing and Collection tags can use a child loop `{{ entries }}` to let you more easily manage your markup.
- There are a lot more modifiers and an easier to use syntax. There's a good chance you can simplify your templates and make them more readable.
- You know what? Just read through the whole [Template Language guide][template-language-guide]. You'll be glad you did.

## Conclusion {#conclusion}

If you need help upgrading your v1 site, let us know! We're offering Upgrades as a Service (I guess that's a UaaS?). Just [send us an email][email] and we'll chat.

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

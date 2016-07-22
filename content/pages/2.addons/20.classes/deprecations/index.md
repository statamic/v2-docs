---
title: Deprecations
id: dab8a4fd-7e1d-4ea4-8e95-c925faec797a
---
## Statamic 2.1

The API was cleaned up considerably with the release of 2.1.

It was made to be more Laravel/Eloquent-ish. The majority of the changes come from the `Statamic\API\Content` class handling almost everything. Now each content type gets its own class.


| Deprecation | Suggestion |
|--|--|
| `Content::uuidRaw()` | `Content::find()` |
| `Content::uuid()` | `Content::find()->toArray()` |
| `Content::getRaw()` | `Content::whereUri()` |
| `Content::get()` | `Content::whereUri()->toArray()` |
| `Content::pageRaw()` | `Page::whereUri()` |
| `Content::page()` | `Page::whereUri()->toArray()` |
| `Content::entryRaw()` | `Entry::whereSlug()` |
| `Content::entry()` | `Entry::whereSlug()->toArray()` |
| `Content::entryByUrlRaw()` | `Entry::whereUri()` |
| `Content::entryByUrl()` | `Entry::whereUri()->toArray()` |
| `Content::taxonomyTermRaw()` | `Term::whereSlug()` |
| `Content::taxonomyTerm()` | `Term::whereSlug()->toArray()` |
| `Content::entryByUrlRaw()` | `Entry::whereUri()` |
| `Content::taxonomyTermByUrl()` | `Term::whereUri()->toArray()` |
| `Content::entries()` | `Entry::all()` or `Entry::whereCollection()` |
| `Content::pages()` | `Page::all()` or `Page::whereUriIn()` |
| `Content::taxonomyTerms()` | `Term::all()` or `Term::whereTaxonomy()` |
| `Content::globals()` | `Globals::all()` or `Globals::whereHandle()` |
| `Content::globalSet()` | `Globals::whereHandle()` |
| `Content::collections()` | `Collection::all()` |
| `Content::collection()` | `Collection::whereHandle()` |
| `Content::collectionNames()` | `Collection::handles()` |
| `Content::collectionExists()` | `Collection::handleExists()` |
| `Content::taxonomies()` | `Taxonomy::all()` |
| `Content::taxonomy()` | `Taxonomy::whereHandle()` |
| `Content::taxonomyNames()` | `Taxonomy::handles()` |
| `Content::taxonomyExists()` | `Taxonomy::handleExists()` |
| `Content::entryExists()` | `Entry::slugExists()` |
| `Content::taxonomyTermExists()` | `Term::slugExists()` |
| `Content::pageExists()` | `Page::uriExists()` |
| `Content::exists($url)` | `Content::uriExists($uri)` for checking URIs. `Content::exists()` is now for checking by IDs. |
| `Content::pageFolders()` | `PageFolder::all()` |
| `Content::pageFolder()` | `PageFolder::whereHandle()` |
| `Page::getByUuid($uuid)` | `Page::find($id)` |
| `Page::getByUrl($url)` | `Page::whereUri($uri)` |
| `Entries::getFromCollection()` | `Entry::whereCollection()` |
| `Entries::createCollection()` | `Collection::create()` |
| `TaxonomyTerm::getFromTaxonomy()` | `Term::whereSlug()` |
| `TaxonomyTerms::getFromTaxonomy()` | `Term::whereTaxonomy()` |

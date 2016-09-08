---
title: Variables
overview: A plethora of variables are automatically available at your fingertips based on context. Let's have a look!
id: 7408e005-23e7-4ba2-bc7b-9f00ee457995
template: variables
types:
  -
    type: global
    description: |
      Globals are available at any time, regardless of whether the URL you are on is for content (a Page, Entry, Term)
      or if it has been created using a route.
  -
    type: content
    description: |
      "Content" encompasses data types that can be mapped to a URL. These include Pages, Entries, and Taxonomy Terms.
      Any time you encounter one of these, the following variables will be available, as well as any fields you've
      defined yourself.
  -
    type: page
    description: |
      Pages have access to all the Content variables, and these:
  -
    type: entry
    description: |
      Entries have access to all the Content variables, and these:
  -
    type: term
    description: |
      Taxonomy terms have access to all the Content variables, and these:
  -
    type: asset
    description: |
      Assets have access to all the Content variables, and these:
---

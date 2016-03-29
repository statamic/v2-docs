overview: >
  Breadcrumbs are a common form of site navigation, usually used in situations where you have a lot of hierarchal content the user must navigate and you want to make sure they know where they are. They consist of a list of links, separated by a divider, leading you from the current page back up to the home page.
description: Display breadcrumb-style navigation links to your current page.
parameters:
  -
    name: 'url '
    type: string
    description: >
      Defaults to your current page, but can be set to anywhere if necessary.
  -
    name: include_home
    type: 'boolean *TRUE*'
    description: >
      You can choose to turn off the home page
      in the breadcrumbs, opting to start the
      crumbs from the first level nav item.
  -
    name: reverse
    type: 'boolean *TRUE*'
    description: Reverse the output of the breadcrumbs.
  -
    name: trim
    type: 'boolean *TRUE*'
    description: >
      Trim the whitespace from each iteration
      of the loop.
variables:
  -
    name: is_current
    type: boolean
    description: >
      Whether the current page is the URL
      being viewed. Useful for outputting
      active states.
  -
    name: page data
    type: mixed
    description: >
      Each page being iterated has access to
      all the variables inside that page. This
      includes things like `title`, `content`,
      etc.
title: Breadcrumbs
id: 485f1703-fc6f-4d0f-94f2-e84ae625e1b7

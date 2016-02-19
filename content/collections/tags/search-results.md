title: Results
overview: Iterate over the results of a search.
parameters:
  - 
    name: fields
    type: array
    description: >
      Specify the fields the search should
      look through. You can pipe-separate
      multiple fields, eg. `title|content`
  - 
    name: param
    type: 'string *q*'
    description: >
      The query string parameter used for the
      search term.
variables:
  - 
    name: no_results
    type: boolean
    description: If there are no results.
  - 
    name: first
    type: boolean
    description: If this is the first item in the loop.
  - 
    name: last
    type: boolean
    description: If this is the last item in the loop.
  - 
    name: count
    type: integer
    description: >
      The number of current iteration in the
      loop.
  - 
    name: index
    type: integer
    description: >
      The zero-based count of the current
      iteration in the loop.
  - 
    name: total_results
    type: integer
    description: The number of results in the loop.
  - 
    name: search_score
    type: float
    description: >
      The internal relevance score that
      Statamic given to this result. Helpful
      for debugging, but useless to the
      public.
  - 
    name: content data
    type: array
    description: >
      Each page being iterated has access to
      all the variables inside that page. This
      includes things like `title`, `content`,
      etc.
id: e74144ff-72f3-4b2a-84da-af6ae5a79746

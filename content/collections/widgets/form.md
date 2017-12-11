---
title: Form
overview: Display a listing of form submissions.
description: Display a listing of form submissions.
parameters:
  -
    name: form
    type: string
    description: The form from which to list submissions.
  -
    name: fields
    type: array
    description: An array of fields to be displayed.
  -
    name: limit
    type: integer
    description: Limit the total results.
  -
    name: date_format
    type: 'string *Y/n/d*'
    description: >
      How the submission dates should be formatted. Defaults to the format defined in your formset, then
      the format defined for the Control Panel.
  -
    name: title
    type: string
    description: The title of the widget. Defaults to the name of the form.
id: 6edfd5dd-ed24-4055-a548-34a2926776a4
---
## Overview

You can use the Form Widget to show the most recent form submissions in your dashboard. You can customize how many submissions it displays as well as which columns you want shown.

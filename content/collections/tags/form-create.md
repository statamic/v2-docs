---
title: Create
id: aa96fcf1-510c-404b-9b63-cea8942e1bf8
overview: >
  Generate necessary `<form>` markup to accept form submissions.
parameters:
  -
    name: formset|in
    type: string
    description: >
      The name of the formset this form should use. This is only required if you do _not_ use the `form:set` tag, or
      if you don't have a `formset` defined in the current context.
  -
    name: redirect
    type: string
    description: >
      The location your user will be taken after a successful form submission. If left blank, the user will stay
      on the same page.
  -
    name: attr
    type: string
    description: >
      Allows you to set any number of HTML attributes on the `<form>` tag. You can specify multiple
      attributes by pipe delimiting them. eg. `attr="class:pretty-form|id:contact"`
variables:
  -
    name: fields
    type: array
    description: >
      An array of the fields in your formset. Each field contains an `old` value that holds previous input
      in the case of failed submission.
  -
    name: errors
    type: array
    description: An array of any validation errors upon submission.
  -
    name: success
    type: boolean
    description: This will be `true` if the form was submitted successfully.
---
## Example {#example}

Here we'll be creating a form to submit an entry in the `contact` form.

```
{{ form:create in="contact" }}

    {{ if errors }}
        <div class="alert alert-danger">
            {{ errors }}
                {{ value }}<br>
            {{ /errors }}
        </div>
    {{ /if }}

    {{ if success }}
        <div class="alert alert-success">
            Form was submitted successfully.
        </div>
    {{ /if }}

    {{ fields }}
        <div class="form-group">
            <label>{{ display }}</label>
            <input type="text" name="{{ name }}" value="{{ old }}" />
        </div>
    {{ /fields }}

    <button>Submit</button>

{{ /form:create }}
```

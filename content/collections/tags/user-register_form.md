---
title: Register Form
overview: 'Generate a `<form>` tag to register a user.'
parameters:
  -
    name: redirect
    type: string
    description: >
      Where the user should be taken after
      successfully registering.
variables:
  -
    name: errors
    type: array
    description: An array of validation errors.
id: 9323ebd8-c36c-4f0f-b73a-cb5ac4544e72
---
The `username`, `email`, `password`, and `password_confirmation` fields are all required.

## Example {#example}

A basic registration form, with validation errors.

```
{{ user:register_form }}

    {{ if errors }}
        <div class="alert alert-danger">
            {{ errors }}
                {{ value }}<br>
            {{ /errors }}
        </div>
    {{ /if }}

    <label>Username</label>
    <input type="text" name="username" value="{{ old:username }}" />

    <label>Email</label>
    <input type="email" name="email" value="{{ old:email }}" />

    <label>Password</label>
    <input type="password" name="password" />

    <label>Password Confirmation</label>
    <input type="password" name="password_confirmation" />

    <button>Register</button>

{{ /user:register_form }}
```

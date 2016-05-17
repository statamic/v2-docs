---
title: How to configure and send email
id: cb453729-0d2f-4924-ba88-b7b1f95747af
overview: >
  Emails are used throughout Statamic, from within the Control Panel to inside tags and addons.
kb_categories:
  - 41f8eb46-342c-4a52-91a3-809d971a5165
---
## Settings {#settings}
In the Control panel, you can find a section for configuring your email settings. It's split up into sections
depending on the email driver you decide to use.

### Drivers {#drivers}
Out of the box, Statamic supports a number of email drivers.

- The `log` driver that simply writes emails to your log file instead of sending. Very useful for testing.
- Server based solutions PHP Mail (`mail`) and `sendmail`. These usually "just work" on most servers. Note we said _usually_, not always.
- Third party services [`mandrill`](https://mandrillapp.com) and [`mailgun`](https://www.mailgun.com/). You'll need accounts on those sites and get API keys.
- The ol' faithful `smtp` option. This will require you to enter your SMTP server credentials.

### Default sender {#sender}
You will also find the "Sender Email Address" and "Sender Name" settings. These will be used in all emails
where a sender isn't explicitly provided. For example, password reset emails.


## Templates {#templates}
Where appropriate, Statamic comes bundled with default email templates.

For example, when triggering a password reset, Statamic has a simple email ready for you.

However, you are able to override any email with your own templates. This will allow you to
customize the design and language.

Throughout the documentation, wherever emails are sent, you will be told the name of the email template
that'll be used. To override the default, you should create a template of the same name in an `email`
subfolder.

### HTML and Text versions {#html-and-text-versions}

Email templates act the same as any other Statamic template, with one extra feature: you can specify
both the text and html versions in the same file. The two versions are separated by `---` with the text
version before, and the html version after.

Since these are Statamic templates, you are able to use any tags, modifiers, and addons that you normally would anywhere else. _Note: you currently cannot use template front-matter in email templates._

For example, if we wanted to override the password reset email, we could create a template file in `site/themes/theme-name/templates/email/user-reset.html` that looks something like this:

```
Reset your password
===================
You may reset your password by visiting {{ reset_url }} in your browser.

---

<h1>Reset your password</h1>
<p>You may reset your password by visiting the <a href="{{ reset_url }}">password reset page</a>.</p>
```

If you'd like to use 3 dashes (`---`) in your template, it will conflict with the default text/html
separator. You can adjust the separator by changing the `email_separator` string in Theming settings.

``` .language-yaml
email_separator: '<!-- HTML -->'
```


## Sending your own emails {#sending-your-own}
If you are creating an addon that needs to send an email, you can use the handy `Email` class.

``` .language-php
$email = \Statamic\API\Email::create();

$email->to('b.lumbergh@initech.com')->subject('TPS Report')->template('tps_report');

$email->send();
```

Read more about the `Email` class over at the [Email class reference](/addons/classes/api/email).

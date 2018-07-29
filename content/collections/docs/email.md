---
title: Email
id: a382a891-987c-4edf-b1cf-497353ee9937
overview: >
  Email notifications and triggers are a critical part of modern websites. Statamic has a number of options for configuring, testing, and delivering email.
---
## Your Email Settings {#settings}

There is a whole settings section in for configuring email. It is split into sections, depending on which email driver you decide to use.

Setting File: `/site/settings/email.yaml`  
Control Panel: `Configure » Email`

### Email Drivers {#drivers}

Out of the box Statamic supports the following email drivers.

**Log** -- The log driver writes email output to your log file instead of actually sending it. This is very useful for testing without cluttering your inbox.

**PHP Mail and Sendmail** -- These native methods of sending email often "just work" on many servers by default, but can sometimes be disabled, especially in shared hosting environments.

**[Mandrill](https://mandrillapp.com) and [Mailgun](https://www.mailgun.com/)** -- These third party services may require accounts with API keys, but are quick to set up and are as close to "bulletproof" as it gets with email delivery.

**SMTP** -- The good, old faithful SMTP. By configuring SMTP server credentials, you can connect to nearly any email delivery service or provider known to humankind. These settings are bit more complicated, so here's an example to get you started:

```
driver: smtp
host: mailtrap.io
port: 2525
encryption: tls
username: 123
password: abc
```

### Default Sender {#sender}

The "Sender Email Address" and "Sender Name" settings are configurable, and used in all emails where a sender isn't explicitly set. For example, password reset or user activation emails.

```
from_email: hello@kitty.website
from_name: Hello Kitty
```

## Email Templates {#templates}

Statamic has a few email templates built in, like password reset. You can override any of these built in email with your own, allowing you to
customize the design, content, and so on.

Throughout this documentation you will find the names of email templates used for various situations. To override them, create a template of the _same filename_ in `site/themes/theme-name/templates/email`.

```
<!-- password_reset.html -->
/site/themes/redwood/templates/email/password-reset.html
```

### HTML and Text versions {#html-and-text-versions}

Email templates work and behave like any other Statamic template, with one added feature: you can specify both the text _and_ HTML versions in the same file. The two versions are separated by three hyphens `---`. The text comes before the seperator, the HTML after.

You are able to use any tags, modifiers, and addons in your email templates, but front-matter is not supported

For example, if we wanted to override the password reset email, we could create a template file in `/site/themes/theme-name/templates/email/user-reset.html` that looks something like this:

```
Reset your password
===================
You may reset your password by visiting {{ reset_url }} in your browser.

---

<h1>Reset your password</h1>
<p>You may reset your password by visiting the <a href="{{ reset_url }}">password reset page</a>.</p>
```

If you'd like to use 3 dashes (`---`) in your template, it will conflict with the default text/html separator. You can adjust the separator by changing the `email_separator` string in Theming settings.

``` .language-yaml
email_separator: '<!-- HTML -->'
```


## Sending Your Own Emails {#sending-your-own}

If you are creating an addon that needs to send an email, you can use the handy `Email` class.

``` .language-php
$email = \Statamic\API\Email::create();

$email->to('b.lumbergh@initech.com')->subject('TPS Report')->template('tps_report');

$email->send();
```

If you have variables to parse in the template, make sure you use the `with` method.

```.language-php
$vars = [
  'message' => 'Yeah... I'm gonna need this stapler',
];

$email->to('m.waddams@initech.com')->subject("You're fired.")->with($vars)->template('fired');

$email->send();
```

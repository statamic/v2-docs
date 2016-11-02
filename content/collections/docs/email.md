---
title: Email
id: a382a891-987c-4edf-b1cf-497353ee9937
---
## Configuring Email Settings {#settings}
There's a whole section for configuring email settings. It's split up into sections depending on the email driver you decide to use.

Setting file: `/site/settings/email.yaml`
Control Panel: `Configure » Email`

### Drivers {#drivers}
Out of the box, Statamic supports a number of email drivers.

- The `log` driver that simply writes emails to your log file instead of sending. Very useful for testing.
- Server based solutions PHP Mail (`mail`) and `sendmail`. These usually "just work" on most servers. Note we said _usually_, not always.
- Third party services [`mandrill`](https://mandrillapp.com) and [`mailgun`](https://www.mailgun.com/). You'll need accounts on those sites and get API keys.
- The ol' faithful `smtp` option. This will require you to enter your SMTP server credentials.

Example SMTP settings:

```
driver: smtp
host: mailtrap.io
port: 2525
encryption: tls
username: 123
password: abc
```

### Default sender {#sender}

You will also find the "Sender Email Address" and "Sender Name" settings. These will be used in all emails where a sender isn't explicitly provided. Like password reset emails, for example.

```
from_email: hello@kitty.website
from_name: Hello Kitty
```

## Using Templates {#templates}
Statamic has a few default email templates, like password reset. You can override any built in email with your own templates, allowing you to
customize the design, content, and do whatever you want.

Throughout the documentation, wherever emails are sent, you will be told the name of the email template that'll be used. To override the default, you can create a template of the same name in an `email`
subfolder of your `site/themes/theme-name/templates/` folder.

### HTML and Text versions {#html-and-text-versions}

Email templates act the same as any other Statamic template, with one extra feature: you can specify both the text and html versions in the same file. The two versions are separated by `---` with the text version before, and the html version after.

Since these are Statamic templates, you are able to use any tags, modifiers, and addons that you normally would anywhere else. _Note: you cannot use template front-matter in email templates._

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
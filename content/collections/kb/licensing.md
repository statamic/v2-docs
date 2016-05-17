---
title: How does Statamic's license work?
alias:
  - Why is my site showing a giant statamic logo?
  - Why isn't my production site loading?
id: cbe048ab-93c3-44aa-9aa8-ee13a04d78eb
kb_categories:
  - 41f8eb46-342c-4a52-91a3-809d971a5165
---
This does not cover any legalities of the license, just the technical features.

## The Statamic Outpost

Statamic will 'radio in' to our web service, which is affectionately named _The Outpost_. We'll send some details across such as your license key and the domain you're using so we can perform appropriate license validation.

This happens once an hour to prevent your site being slowed down. Changing your license key setting will force Statamic to radio in without waiting the full hour. Tampering with it will cause Statamic to consider your license invalid. We'll also send our flying police monkeys to your office to throw poop at you. Oh, and arrest you.

## Single Website

A Statamic license entitles you to use it on _one single domain_. You need to specify the domain you plan to use from the [licenses page in your account](https://account.statamic.com/licenses). The domain will be treated as a wildcard, however, so that you may use subdomains for locales, etc.

This also means that you can only use the control panel from the URL of your _default_ locale.

If you use attempt to use the site from another domain, you'll get an angry message from within your Control Panel. You may change the domain through your account/licenses page at any time.

## Trial

The trial gives you access to all features of Statamic as long as you are on a non-public domain.


## What is a public domain?

When Statamic calls home, we'll determine if the domain you're using is considered public.

If any of the following checks pass, the domain is considered _not_ public:

- Is it a single segment? eg. `localhost`
- Is it an IP address?
- Does it use a port which is something besides `80` or `443`?
- Does it have a dev-esque subdomain? eg. `test.`, `testing.`, `sandbox.`, `local.`, `stage.`, or `staging.`
- Does it use a dev-esque TLD? eg. `.dev`, `.local`, or `.app`

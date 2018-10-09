---
title: Obfuscate
id: 161f6255-465d-41bd-8028-d6aba01cebbf
parse_content: false
overview: |
  Obfuscation is a method of encoding content so that the source code is hard or impossible to understand. This is generally used on email addresses to prevent spambots from recognizing it as an email address and keeping you safe from unwanted emails.
description: Obfuscate content (usually email addresses) to prevent screenscraping.
---
## Usage {#usage}

```
{{ obfuscate }}
	heisenberg@example.com
{{ /obfuscate }}
```

```.language-output
# output appears as heisenberg@example.com
he&#x69;se&#x6e;&#x62;&#x65;&#114;&#103;@&#101;&#120;a&#109;&#x70;&#x6c;&#x65;&#x2e;c&#x6f;&#109;
```

### Tip:

If you have chosen to use EMail addresses for logging in users (which means `{{ username }}` is a member's  email address), and you want to create a `mailto:` link, you need to use obfuscate's modifier form, as in:  `<a href="mailto:%22{{ first_name }}%20{{ last_name }}%22%20%3C{{ username | obfuscate }}%3E" >`. 

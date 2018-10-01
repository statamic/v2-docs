---
title: How URLs Work
overview: "Let's not get too technical, there are plenty of other sites dedicated to HTTP requests. Here we're going to explain the <span class="highlight">two primary methods</span> Statamic uses to establish URLs: **Pages and Routes.**"
id: 439b2338-4dba-498f-bbd9-7cb6435807fe
---
## Pages {#pages}

Pages are literally™ a literal representation of your site's URL structures. Each Page lives in hierarchy among other Pages. Together they form a logical tree structure made of parents, children, and siblings.

If you want to be thorough and overly detailed with the analogy, we could also find some grandparents, cousins, aunts, uncles, nephews, orphans, and probably that weird next-door neighbor who invites himself to all of your parties and calls himself **<FEED.XML>** in all caps.

![Hark! Pages!](/assets/img/screenshots/cp-page-tree.jpg) {.rounded}

Each page has its own `slug`, or segment of a URL (such as `oh-hi-mark`) and those slugs team up based on hierarchy to create URLs. Let's walk through an example and then we'll have a visual.

### Slugs and Restaurants {#slugs}

Let's assume you own a restaurant that _isn't_ failing, doesn't need Gordon Ramsey or Robert Irvine to save it, and is booked solid 7 nights a week with people desperate to eat your delicious yum yums.

Naturally, we decide to create a **Menus** page with two sub-pages called **Specials** and **Wine List**. A good start. Because of that parent/child relationship, the following URLs are now available on the site, each with its own unique content.

- `http://example.com/menu`
- `http://example.com/menu/specials`
- `http://example.com/menu/wine-list`

A slug is the bit of the URL unique to a particular piece of content. You've just seen how the `specials` and `wine-list` slugs combined with their parent page's slug, `menu`, to create full and unique URLs.

> Slugs are primarily used to make URLs more user-friendly, easy to remember, and help your users know what to expect before clicking a link. Search engines rank pages higher if the search word is in the URL. **It's a scientific fact.**

### Best Practices {#best-practices}
Slugs should always be lowercase, never contain spaces, and generally always use hyphens to separate words. Follow those rules blindly and you'll be just fine. Break them and you're on your own.

Pages, Entries, and Taxonomy Terms all have slugs. That sentence sounds gross out of context. Feel free to learn more about all of [Statamic's Content Types](/content-types) - including those with and without slugs.

## Routes {#routes}

Routes are rules that map URL patterns to your content, templates, or other parts of the application. They might be explicitly defined -- like an exact URL pointing to an exact template -- or they might contain variables that let Statamic change content out and load different templates dynamically.

Go learn some more about [routes](/routing). You'll be glad you did.

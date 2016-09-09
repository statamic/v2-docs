---
title: How URLs Work
overview: "A website is nothing if not a collection of URLs that each display some sort of content. Luckily, Statamic is nothing if not a tool to create and manage URLs. With that small victory established, there are two primary methods of creating and managing your site's URLs."
id: 439b2338-4dba-498f-bbd9-7cb6435807fe
---
## Pages {#pages}

Pages are a wonderfully literal representation of your site's URL structures. Each Page lives in hierarchy among other Pages. Together they form a logical tree structure made of parents, children, and siblings. If you wanted to be semantic and overly detailed with the analogy you would be forced to acknowledge the grandparents, cousins, aunts, uncles, nephews, and probably that weird next-door neighbor who invites himself to all of your parties and calls himself **<FEED.XML>**. Really? All caps?

![Hark! Pages!](/assets/img/screenshots/cp-page-tree.png)

Each page has its own `slug`, and those slugs team up based on hierarchy to create URLs. Let's walk through an example and then we'll have a visual. Let's assume we have a restaurant that isn't failing, doesn't need Gordon Ramsey or Robert Irvine to save it, and is booked solid 7 nights a week with people desperate to eat your delicious yum yums.

Naturally, we decide to create a **Menu** page on the website and give it the slug: `menu`. Next we create two new sub-pages called **Specials** with the slug `specials` and **Wine List** with the slug `wine-list`. Mission accomplished.

Because of that parent/child relationship, the following URLs are now available on the site, each with its own unique content:

- `http://example.com/menu`
- `http://example.com/menu/specials`
- `http://example.com/menu/wine-list`

### Slugs {#slugs}

As you may have guessed from context, a slug is the bit of the URL unique to a particular piece of content. You've just seen how the `specials` and `wine-list` slugs combined with their parent page's slug, `menu`, to create full and unique URLs.

> Slugs are primarily used to make URLs more user-friendly and help your users know what to expect before clicking a link. Search engines rank pages higher if the search word is in the URL. It's a scientific fact.

### Best Practices {#best-practices}
Slugs should always be lowercase, never contain spaces, and almost always use hyphens to separate words. Follow those rules blindly and you'll be just fine.

Pages, Entries, and Taxonomy Terms all have slugs. That sentence sounds gross out of context. Feel free to learn more about all of [Statamic's Content Types](/guides/content-types) - including those with and those without slugs.

## Routes {#routes}

Routes are a common pattern in the world of web applications, but not one found often in content management.

Routes are, on the most basic level, a list of rules that map URL patterns to your content, templates, or other parts of the application. They might be explicit, like an exact URL pointing at an exact template, or they might contain variables that let URL patterns do things more dynamically.

Learn all about writing [routes in Statamic](/routing)!

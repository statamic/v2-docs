---
title: Version Control Strategies
kb_categories:
  - 41f8eb46-342c-4a52-91a3-809d971a5165
id: 7c9fd9f6-69d8-402a-ac95-ae676200c293
---
Statamic is literally designed from the ground up to be 100% version controllable. The Flat File approach enables workflows that would be impossible with a database-driven platform. You can branch, fork, pull request, merge, and rollback any changes to your content, templates, assets, you name it.

If you're used to using a Git workflow for your projects, you probably already have a good idea how to deploy your site updates. If not, check out [Deploying Sites with Laravel Forge](/knowledge-base/forge).

## Dealing with changes on production

The area most people need a few creative ideas around is how to keep changes in production in sync with the repo. Here are a few solutons we've used in the past, present, and future.

### The old "SSH and commit" trick

If you have full access to your server and you're deploying with Git, it's pretty trivial to SSH into production and run a few quick commands. You could even script/schedule this.

```.language-console
git add --all
git commit -m "changes from production"
git push
```

### Spock

We built an addon that watches for changes to your content and then run whatever console commands you tell it. In this workflow, each change becomes a commit. You can even customize the commit messages with the name of the author, the url of the file changed, and so on. You get the picture.

Check out [Spock and its docs](https://github.com/statamic/spock).

### Github as a control panel

As much as we love the control panel (we put a ton of work into it), there's a really good argument for using Github as your control panel. [Asana](https://asana.com) uses Statamic in this fashion and they have an [awesome blog post about their workflow](https://blog.asana.com/2014/02/scaling-asana-com/).

See how creative you can be when you remove the DB?

### Don't makes changes in production

Sure it seems obvious, but we don't mind being thorough. You don't always _have_ to force your clients or content managers make changes in production. You could stand up a staging server (our [license agreement](https://statamic.com/license) allows for this, no second license needed!) so you could preview changes before they go live with the help of a developer, or someone who knows the basics of git.

### Don't version control your content

We don't like this one so much, but at least a handful of our users don't actually version control their content. They mount it on Dropbox or AWS, or `rsync` the "one source of truth" to production. It's another way to live.

But honestly, try to find a workflow that lets you just version control everything. Someday you will thank us. And when you do, send us a postcard.
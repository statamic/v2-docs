---
title: Updating
id: e7560816-e650-4fc4-886e-43d9b14ee558
overview: Statamic is updated regularly! We're always fixing bugs, adding features, and playing hopscotch. You can keep an eye on the [Changelog](https://statamic.com/changelog) or your Control Panel sidebar to see when new updates are available.
---

## Command line {#command-line}

Statamic has a command line updater. Just run the following command:

``` .lang-bash
php please update
```

That's it! It'll make a backup, download the latest release, swap out the files, and clean up after itself. 

## Control Panel {#control-panel}

Statamic also has an updater in the Control Panel. When an update is available, users with Update permissions will see a badge in the sidebar next to the logo. Clicking on that badge will take you to a magical, bountiful land flowing with updates and releases.

<div class="screenshot">
		<img src="/assets/img/screenshots/update-available.png">
</div>

Once here, all you need to do is click the **Update** button and watch as the work is done for you.

You can get to this section at any time by going to `Tools » Updater`. It's possible that you could beat the system and find an update before it makes it to the badge. Just keep refreshing. That's one way to live.

## Manual updates {#manual-updates}

Manually updating is just as simple:

- Download the latest version from [statamic.com](http://statamic.com).
- Replace your `statamic/` folder with the newest one.
- Run `php please update:housekeeping` to perform any additional tasks that the updater would have taken care of automatically.
- Run `php please update:addons` if you have any addons installed.

---

If you're looking for how to upgrade from Statamic v1, check out the [Migrating From v1 Guide](/migrating).

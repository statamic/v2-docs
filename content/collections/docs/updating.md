---
title: Updating Statamic
id: e7560816-e650-4fc4-886e-43d9b14ee558
overview: Statamic is updated regularly, with an average release every other week. We never stop fixing bugs, adding features, and playing hopscotch with our adult neighbors. You can keep an eye on the [Changelog](https://statamic.com/changelog) or your Control Panel sidebar to see when new updates are available.
---

## Via Command Line {#cli}

Statamic has a command line updater. Just run the following command...

``` .lang-bash
php please update
```

...and that's it. It'll make a backup, download the latest release, swap out the files, and clean up after itself, unlike guests at your annual Christmas party.

## Via Control Panel {#control-panel}

Statamic also has an updater in the Control Panel. When updates are available, users with Update permissions will see a badge in the sidebar nav with the number available. Clicking on that badge will take you to a magical, bountiful land flowing with updates and releases.

<div class="bg-blue-grey p-2 mb-2 rounded">
    <img src="/assets/img/screenshots/cp-update-available.png" width="208" height="126">
</div>

## DIY Manual Updates {#manual-updates}

Manually updating involves the follow steps:

- Download the latest version from [statamic.com](http://statamic.com).
- Replace your `statamic/` folder with the newest one.
- Run `php please update:housekeeping` to perform any additional tasks that the updater would have taken care of automatically.
- Run `php please update:addons` if you have any addons installed.

---

> If you're looking for how to upgrade from Statamic v1, check out the [Migrating From v1 Guide](/migrating).

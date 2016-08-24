---
title: Updating
id: e7560816-e650-4fc4-886e-43d9b14ee558
---

## One-click Updates {#one-click-updates}

Statamic has a one-click updater built into the Control Panel. When an update is available, users with Update permissions will see a badge in the sidebar next to the logo. Clicking on that badge will take you to a magical, bountiful land flowing with updates and releases.

Once here, all you need to do is click the **Update** button and watch as the work is done for you.

You can get to this section at any time by going to `Tools » Updater`. It's possible that you could beat the system and find an update before it makes it to the badge. Just keep refreshing. That's one way to live.

## Manual Updates {#manual-updates}

If you need to update your install by hand, download the latest version from [statamic.com](http://statamic.com) and replace your `statamic/` folder with the newest one.

If you have any addons, you should run `php please addons:refresh` to make sure any addons know an update has been run. If you don't have command line access, you can go to `Configure » Addons » Refresh` and run it there.

Lastly, run `php please clear:cache` to – you guessed it – clear your cache.

_If you're looking for how to upgrade from v1, check out the [Migrating From v1 guide](/migrating)._

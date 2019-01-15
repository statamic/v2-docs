---
title: Reordering items in a collection saves very slowly (with Spock installed)
kb_categories:
  - Troubleshooting Common Scenarios
---
If you've got a numerically-ordered collection, and you use the *'Reorder'* functionality in the Control Panel, **and** you've got Spock installed, and set to push its commits automagically, you may have also noticed that the wait between clicking *'Save Order'* and being returned to the collection's entry listing can take a pretty long time. 

Statamic collections are organized by filename — by date (in the case of date-based entries), or by number (in the case of numerically-ordered entries). Statamic's entry-reordering operates by altering/updating the filenames of the files included in the view being reordered. 

Presently, Statamic's *'Reorder'* functionality fires off an event for **each** file that gets touched by a *'Reorder'* — and even if the relative order of a given entry isn't changing, if it's in the view within which the *'Reorder'* is taking place, it's getting touched by the process.

Spock, in-turn, attempts to commit and — here's where things can get slowed down — `git push` the touched entries, one-at-a-time, up to your repository. Since each push takes some time to complete (whether or not the push is successful — more on that shortly), the total amount of time the Control Panel hangs up between saving the re-order, and returning you to the listing, is the total amount of time that ALL of those Spock-initiated `git push` commands take to execute. At sometimes upwards of a second or two per affected entry, this can add up.

An additional piece of the puzzle is that *some* of the commits/pushes will probably fail, since some of the files have been touched, *but not actually changed at all* — and GIT sends back a response that in-turn winds up getting logged to Statamic's log file(s). Which *may* add to the experienced delays.

## What can you do?

- **Disable Spock's Automatic `git push`**  
  If Spock isn't attempting to `git push` each commit it makes, in sequence, this problem seems to be eliminated — or at least the execution time required for all of the commits is far less than if each of them is also being pushed, in sequence, one at a time. With this solution/workaround, you'll need to remember to `git push` manually at the end of your content editing/managing session in order to propagate your automagically-Spock-committed changes back to your repo.

- **Install and enable Redis queuing**  
  In Spock v2.2.0, the ability to have Spock queue its commands was added. You can take advantage of this to allow Spock to apply its Vulcan logic to your files in the background, while your Control Panel stays snappy for you (and the site's other users). You'll need to have Redis installed and configured on the server in question, and your Statamic `.env` file will need to be modified to identify Redis as the queue driver. Please see the [Spock docs](https://statamic.com/marketplace/addons/spock/docs#queueing-commands) for configuration information.
  
  Installing and configuring Redis on your server is going to differ depending on the server configuration. Here are some links to a few of the more common server/OS scenarios:
  
  [Installing Redis on OS X](https://medium.com/@petehouston/install-and-config-redis-on-mac-os-x-via-homebrew-eb8df9a4f298)
  [Installing Redis on Ubuntu 14.04](https://hostpresto.com/community/tutorials/how-to-install-and-configure-redis-on-ubuntu-14-04/)
  [Installing Redis on Ubuntu 16.04](https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-redis-on-ubuntu-16-04)
 
 - **Wait for newer versions of Statamic that have *Advanced Alien Technology*™**
   It seems that in the future (near or far currently unknown), the event(s) fired by Statamic when a collection is re-ordered/saved may be improved, in order to consolidate all of the changes made into a single event (thus causing Spock to only execute a single commit and/or push). Which should, in-turn, reduce the time spent by the system executing those processes, and make the re-order/save experience a lot snappier in this situation. 

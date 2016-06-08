---
title: I see an infinitely spinning loading graphic.
kb_categories:
  - f470efec-cdc6-4e6a-ac40-bf30f3515cc4
id: 3b9561bf-cf36-42b5-aeb9-34dd2cacc430
---
Statamic uses AJAX all over the place, and uses a loading graphic while you wait for the request to complete.
If something goes wrong during the request on the server side, it may never be marked as complete on the Javascript side.

## What can you do?

- **Check your logs**  
  Most of the time if there's a server error, a log will be generated containing the error details. You can find yours
  in `local/storage/logs`.

- **View the actual response**  
  If you didn't have your developer console open when it happened or you have `debug` mode disabled, you wont be able
  to use this method. You could open your console and try whatever you did again, if you like. Make sure debug mode
  is on, too.

  However if you _did_ have it open, you might see a Javascript error
  saying something like `Uncaught (in promise)`. This isn't really helpful apart from saying 'something went wrong'.

  To see the actual error you will need to open the _Network_ tab and click
  the failed request (it would be red). If you had debug mode enabled, you would see a _Whoops, looks like something went
  wrong_ screen with the error message. That's the one that you need.

- **Tell us all about it**  
  Explain what happened, what you were doing, and give us the log and/or error message. We'll be able to add some better
  error handling at that point so that you see something more useful than a spinning circle.

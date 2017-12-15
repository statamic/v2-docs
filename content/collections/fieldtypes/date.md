title: Date
description: A datepicker that includes an optional timepicker.
overview: >
  Select a date and optional time. Data is stored as a timestamp and can be formatted with any of the date [modifiers](/modifiers) or used in Tag conditions to filter results.
image: /assets/fieldtypes/date.png
options:
  -
    name: allow_time
    type: boolean *true*
    description: Enable/disable the timepicker
  -
    name: allow_blank
    type: boolean *false*
    description: Allow the field to be left blank on save
  -
    name: format
    type: string *Y-m-d* (*H:i*)
    description: How the date should be saved. Any [PHP date formatting variables](http://php.net/manual/en/function.date.php) may be used.
id: 7dfba904-8a74-40e1-b507-51cd2b5f6123

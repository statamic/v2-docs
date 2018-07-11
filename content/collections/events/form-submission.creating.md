---
title: Form.submission.creating
class: Form.submission.creating
id: 4d01ae66-fb74-44e9-98b0-bb7f8a114a87
---
Fired when a form is submitted, but before it is saved. Allows you to stop or modify the submission.

The `Submission` object will get passed through.

To modify the submission, return an array with a `submission` key containing the modified `Submission` object.

To stop the submission and display errors, return an array with an `errors` key containing an array of error messages.

```
use Statamic\Contracts\Forms\Submission;

public $events = ['Form.submission.creating' => 'handle'];

public function handle(Submission $submission)
{
    return [
        'submission' => $submission,
        'errors' => []
    ];
}
```

To stop the submission, but make it appear as though it was successful (for example, a Honeypot spam technique) you may throw a `Statamic\Exceptions\SilentFormFailureException`.

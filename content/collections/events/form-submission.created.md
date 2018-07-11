---
title: Form.submission.created
class: Form.submission.created
id: 1f723274-fdab-47e2-ac63-d175f2b5be74
---
Fired when the submission is successfully saved. The `Submission` object is passed through.

```
use Statamic\Contracts\Forms\Submission;

public $events = ['Form.submission.created' => 'handle'];

public function handle(Submission $submission);
```

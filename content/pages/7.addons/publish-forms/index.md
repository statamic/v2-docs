---
title: Custom Publish Forms
id: f76ec58e-d0ea-436c-a099-1f5c5ebc2c19
overview: >
  When creating or editing content (entries, pages, etc), you are presented with a form view. This is what we call the "Publish" form.
  As of Statamic 2.8, it's possible for you to use this component in your addons.
---

## Overview

The Publish component takes some data, pre-processes it through a fieldset, renders it within the appropriate fieldtypes, and post-processes it upon submission.

Typically, the data comes from within Statamic - for example a page may be stored in `site/content/pages`.
However, in your addon, you may want to edit data coming directly from an external source. That can anything you want: a database, an API, etc.

The flow is essentially this:

- You retrieve data from somewhere, however you want.
- Pass that data along with a fieldset into the `populateWithBlanks` method. This will prepare the data for consumption by the Vue component.
- Render the Vue component by passing in some props.
- Pass the submitted form fields along with the fieldset into the `processFields` method. This will prepare the data for saving to your source.
- Save the data somewhere, however you want.

## Example

Below you will find an example controller that handles creating, storing, editing, and updating items.

It uses a fictional `ContentStore` class with `get` and `save` methods which would get or save the data to and from some source like an API.
Of course, however you choose to implement this part is up to you.

``` .language-php
<?php

namespace Statamic\Addons\ContentStore;

use Statamic\API\Helper;
use Statamic\API\Fieldset;
use Illuminate\Http\Request;
use Statamic\Extend\Controller;
use Statamic\CP\Publish\ProcessesFields;
use Statamic\CP\Publish\PopulatesWithBlanks;

class ContentStoreController extends Controller
{
    use ProcessesFields;
    use PopulatesWithBlanks;

    private $store;

    public function __construct(ContentStore $store)
    {
        $this->store = $store;
    }

    public function create()
    {
        return $this->view('index', [
            'id' => null,
            'data' => $this->prepareData([]),
            'submitUrl' => route('contentstore.store')
        ]);
    }

    public function edit(Request $request)
    {
        $id = $request->id;

        $data = $this->prepareData($this->store->get($id));

        return $this->view('index', [
            'id' => $id,
            'data' => $data,
            'submitUrl' => route('contentstore.update')
        ]);
    }

    public function update(Request $request)
    {
        $id = $request->uuid;

        $this->save($id, $request);

        return $this->successResponse($id);
    }

    public function store(Request $request)
    {
        $id = Helper::makeUuid();

        $this->save($id, $request);

        return $this->successResponse($id);
    }

    /**
     * Prepare the data for the view.
     *
     * Vue needs to have at least null values available from the start in order
     * to properly set up the reactivity. The data in the contentstore is not
     * guaranteed to have every single field. This method will add the
     * appropriate null values based on the provided fieldset.
     *
     * @return array
     */
    private function prepareData($data)
    {
        return $this->populateWithBlanks(Fieldset::get('post'), $data);
    }

    private function save($id, $request)
    {
        // Vue submits data slightly differently to how it should be saved.
        // Each field's fieldtype will process the data appropriately.
        $data = $this->processFields(Fieldset::get($request->fieldset), $request->fields);

        $this->store->save($id, $data);
    }

    private function successResponse($id)
    {
        $message = 'Entry saved';

        if (! request()->continue || request()->new) {
            $this->success($message);
        }

        return [
            'success'  => true,
            'redirect' => route('contentstore.edit') . '?id=' . $id,
            'message' => $message
        ];
    }
}
```

``` .language-yaml
routes:
  edit:
    uses: edit
    as: contentstore.edit
  post@update:
    uses: update
    as: contentstore.update
  create:
    uses: create
    as: contentstore.create
  post@store:
    uses: store
    as: contentstore.store
```

The Blade view should place the JSON encoded data in the `Statamic.Publish.contentData` object, and pass some props into the `publish` Vue component.

``` .language-blade
@extends('layout')

@section('content')

    <script>
        Statamic.Publish = {
            contentData: {!! json_encode($data) !!},
        };
    </script>

    <publish title="{{ $id ? $data['title'] : 'New entry' }}"
             :is-new="{{ bool_str($id === null) }}"
             fieldset-name="post"
             content-type="entry"
             submit-url="{{ $submitUrl }}"
             id="{{ $id }}"
             :extra='{"collection": "blog"}'
             :remove-title="true"
    ></publish>

@endsection
```

| Prop | Description |
|------|-------------|
| `title` | The heading of the form. This is typically the entry's title, or "New Blog Post". |
| `is-new` | A boolean that controls a couple of feature such as whether slugs should be auto-generated. |
| `fieldset-name` | The name of the fieldset to be rendered. |
| `content-type` | Generally an `entry` or `page`. If you choose `entry`, you can pass `extra.collection` to make it editable based whether the user can edit that collection.  If you choose `page` it'll be editable if they can edit pages. You may omit this prop. |
| `submit-url` | Where the form will be submitted. You should point it at one of your controller routes. |
| `id` | The ID of the item. It'll get posted at submission. |
| `remove-title` | A boolean that hides the `title` field if one exists in the fieldset. A special title field is added to the component that'll be _before_ the sidebar meta on narrow viewports. |
| `extra` | An object containing some extra data specific to certain cases. |
| `extra.collection` | When using `content-type: entry` this will be the corresponding collection. |
| `extra.order_type` | When using `content-type: entry` this will be the the way the collection is ordered. eg, `date`. |
| `extra.datetime` | When using `content-type: entry` this is the date (and time) of the entry. eg, `2017-07-31 10:42` |
| `extra.datetime` | When using `content-type: taxonomy` for editing taxonomy terms, this is name of the taxonomy. |
| `extra.parent_url` | When using `content-type: page` this will be the parent page's URL. |
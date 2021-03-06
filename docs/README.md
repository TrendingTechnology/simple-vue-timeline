# simple-vue-timeline
![CI](https://img.shields.io/travis/scottie34/simple-vue-timeline/master.svg?style=flat-square)
![download](https://img.shields.io/npm/dm/simple-vue-timeline.svg?style=flat-square)
![version](https://img.shields.io/npm/v/simple-vue-timeline.svg?style=flat-square)
![license](https://img.shields.io/badge/license-MIT-green.svg?style=flat-square)

A timeline vue component which leverages the use of common libraries:
 * [bootstrap vue components](https://bootstrap-vue.js.org/),
 * [vue-fontawesome](https://github.com/FortAwesome/vue-fontawesome) 
 * and [fecha](https://github.com/taylorhakes/fecha) date formatting.

If you find it useful, give it a [star](https://github.com/scottie34/simple-vue-timeline) and please consider [buying me a coffee](https://www.buymeacoffee.com/scottie34).

Use [github](https://github.com/scottie34/simple-vue-timeline) for any issue you encountered or to give me some feedback of your usage.

![sample](https://raw.githubusercontent.com/scottie34/simple-vue-timeline/master/docs/simple-vue-timeline.png)

* TOC
{:toc}

## Getting Started

### Installation
```
npm install --save simple-vue-timeline
```

### Style
```ts
@import '~simple-vue-timeline/dist/simple-vue-timeline';
```

As bootstrap is used, you must add the bootstrap style:
```ts
@import '~bootstrap/scss/bootstrap';
```

### Font Awesome
Refer to [vue-fontawesome](https://github.com/FortAwesome/vue-fontawesome#usage) documentation.  


### Template Element
Add the element as follow:

`template`
```vue
<timeline :items="items" dateFormat="YY/MM/DD" @timeline-edit="edit" v-on="$listeners"></timeline>
```

`script`
```ts
import { SimpleTimeline, Item, Control, Status } from 'simple-vue-timeline';

@Component({
  components: {
    timeline: SimpleTimeline
  }
})
export default class ...
```
Refer to the `Vue Class Component Sample` section below for a complete sample.

## Props

| Name | Type | Description |
| --- | --- | --- |
| `items` | `Item[]` | An item array containing your timeline items |
| `dateformat` | `string` | The [fecha](https://github.com/taylorhakes/fecha) pattern to use to format date |
| `v-on` | `Listener[]` | This one must be set to `$listeners` to be able to react on event emitted by Control (see Controls below) |
| `@timeline-<xxx>` | `string` | The method to be called to react on `<xxx>` events (see Controls below) |

## items
Component expects an array of Items

| Variable | Type | Description |
| --- | --- | --- |
| `id` | `number` | An unique id for your Item |
| `icon` | `string` | The id of the `fontawesome` icon to use |
| `status` | `Status` | A field of the Status enum related to [bootstrap color variant](https://bootstrap-vue.js.org/docs/reference/color-variants/#base-variants) (used for icon and card). |
| `title` | `string` | The Item title |
| `controls` | `Control[]` | An array of control for this Item (see Controls below) |
| `createdDate` | `Date` | Date of your Item (`dateFormat` used to format it) |
| `body` | `string` | The Item content |

## Controls
It allows to add buttons on your Item.

| Variable | Type | Description |
| --- | --- | --- |
| `method` | `string` | A method name used when emitting events |
| `icon` | `string` | The id of the `fontawesome` icon to use |

### Event
On button click, an event is emitting using the identifier `timeline-<method>`.

Events are emitted with the following object as parameter 
```vue
{ eventId: this.eventId }
```
 (`this.eventId` matches the id of the Item).

#### Example
For instance adding the following control 
```vue
new Control("edit", "pencil-alt")
```
will generate `timeline-edit` event.

To react on such event, one should provide:
 * the following prop to the timeline component: 
 ```vue
 @timeline-edit="edit"`
```

 * the associated `edit` method which will be called 
```vue 
public edit(e: any) {
    console.log("edit " + e["eventId"]);
}
```

## Vue Class Component Sample

```vue
<template>
  <div id="app">
    <timeline
      :items="items"
      @timeline-edit="edit"
      @timeline-copy="copy"
      @timeline-trash="trash"
      v-on="$listeners"
    ></timeline>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import {
  SimpleTimeline,
  Item,
  Control,
  Status
} from "simple-vue-timeline";

@Component({
  components: {
    timeline: SimpleTimeline
  }
})
export default class App extends Vue {
  public items: Item[] = [
    new Item(
      0,
      "calendar-alt",
      Status.WARNING,
      "title",
      [new Control("edit", "pencil-alt"), new Control("copy", "plus")],
      new Date(),
      "Here is the body message of item 0"
    ),
    new Item(
      1,
      "tasks",
      Status.WARNING,
      "title",
      [new Control("edit", "pencil-alt"), new Control("trash", "trash")],
      new Date(2019, 10, 20),
      "Here is the body message of item 1"
    )
  ];

  public edit(e: any) {
    console.log("edit " + e["eventId"]);
  }

  public copy(e: any) {
    console.log("copy " + e["eventId"]);
  }

  public trash(e: any) {
    console.log("trash " + e["eventId"]);
  }
}
</script>
```


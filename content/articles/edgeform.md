---
title: Edge Form
description: "A Dynamic form build by a JSON Schema"
position: 2
category: "mbrvault"
author: Dustin Stewart

corecomponents:
  - DateEdit
  - ModelAutocomplete
  - Money
  - MoneyRaw
  - Phone
  - SelectEdit
  - States
  - SwitchEdit
  - TextareaEdit
  - TextareaReadOnly
  - TextEdit
  - Time
  - Wysiwyg (tiptap)

peers:
  - vue 2.6
  - vuetify 2.3.4
  - vuetify-loader 1.6
  - vue-the-mask 0.11.1
  - tiptap-vuetify 2.24.0
  - v-currency-field 3.1.1
  - v-money 0.8.1
  - lodash 4.17.20


---

## Overview

EdgeForm.vue is a dynamic form that is produced from a JSON schema.  It validates inputs and updates or creates resources.

`<edge-form :headers="schema" :active="active" path="contacts" meta v-on:act="alert" />` produces a form that looks like this: 
<form-demo></form-demo>

## Installation

`npm install edge-form`

### Uninstalled Peer Dependencies

<list :items="peers"></list>


## Introduction

The edge-form generates a form for creating a new record or editing a record based on a schema.



The edge-form loops through the active schema and creates a row containing a single input. The input is selected based upon the components "is" prop: `:is="mapType(field.type)"` We have a mapType function that will render an actual component in place of `<component>`.

#### Props

<table>
<thead>
<tr>
<th>Name</th>
<th>Type</th>
<th>Default/Required</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>headers</td>
<td>Array</td>
<td>Required</td>
<td>The schema used to build the form</td>
</tr>
<tr>
<td>active</td>
<td>Object</td>
<td>Required</td>
<td>If creating a new resource, active is the "bound" structure, else if editing it is the active resource</td>
</tr>
<tr>
<td>path</td>
<td>String</td>
<td>null</td>
<td>The name of the Vuex module from the store folder</td>
</tr>
<tr>
<td>meta</td>
<td>Boolean</td>
<td>false</td>
<td>Used by the form to differentiate between create and update (candidate for removal)  </td>
</tr>
<tr>
<td>subUpdate</td>
<td>Boolean</td>
<td>false</td>
<td>The form can be used to "subUpdate" -> otherwise known as a put request on a nested resource: `members/1/notes/3`</td>
</tr>
<tr>
<td>parentPath</td>
<td>String</td>
<td>""</td>
<td>Used only with the SubUpdate request to determine what the parent resource is.  This is a candidate for cleanup. </td>
</tr>
<tr>
<td>noIcon</td>
<td>Boolean</td>
<td>false</td>
<td>Changes the layout to remove the icon. This also changes the base layout b/c the icon uses 1 col in a 12 column grid.  If this is true, the input becomes a full 12 cols </td>
</tr>
<tr>
<td>dense</td>
<td>Boolean</td>
<td>true</td>
<td>Controls the vuetify dense prop. If set to false the form will not be `dense` </td>
</tr>
</tbody>

</table>
<div>

#### Data

<table>
<thead>
<tr>
<th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>editField</td>
<td>Communicates back with the component on which field is active. May be removed in the future</td>
</tr>
<tr>
<td>selectField</td>
<td>Confirms the editField. May be removed in the future.</td>
</tr>
<tr>
<td>loading</td>
<td>Boolean value that displays the loading component. </td>
</tr>
</tbody>
</table>

#### Computed Properties

<table>
<thead>
<tr>
<th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>OrderedHeaders</td>
<td>clones and sorts the headers prop by the order property.</td>
</tr>
<tr>
<td>alt</td>
<td>A computed getter with a get/set function. It provides access to the active prop without mutating it.  The setter allows us to change the value of alt through the use of a function; `saveChangedField`</td>
</tr>
</tbody>
</table>

#### Example Schema

```{
  "title": {
    "field": "title",
    "icon": "mdi-check-outline",
    "display": true,
    "createDisplay": true,
    "label": "Note Title",
    "type": "text",
    "required": true,
    "value": "title",
    "used": "true",
    "permission": "",
    "order": 1,
    "column": 1
  },
  "body": {
    "field": "body",
    "icon": "mdi-sign-text",
    "display": true,
    "createDisplay": true,
    "label": "Note Details",
    "type": "textarea",
    "required": false,
    "value": "body",
    "used": "true",
    "permission": "",
    "order": 2,
    "column": 2
  }
}
```


#### Interacting with Inputs

The core vue components that appForm interacts with are:
<list :items="corecomponents"></list>

Additionally, there are special circumstances or edge cases that require the use of a custom component. An example of this is the memberAction button. This is a read only, formatted link that opens up the member in an active dialog.

#### Special components

You can use your own components that are not listed directly in this project by calling them in the schema.  The schema first tries to find the component in our core component file and if it can't match based on the name, it will attempt to render a component with the exact field name from the schema. 

Example of a component structure: 

```
<template>
  <div>
    <transition name="fade" mode="out-in">
      <v-text-field
        data-text-field
        key="textActive"
        :ref="opts.field"
        :outlined="editField == opts.field"
        dense
        v-on:keydown.enter="emitChange"
        v-on:keydown.tab="emitChange"
        v-model="computedValue"
        :label="opts.label + required"
        class="ma-3"
        :readonly="opts.readOnly"
        :rules="rules"
        validate-on-blur
        @focus="emitFocus"
        @change="emitChange"
      ></v-text-field>
    </transition>
  </div>
</template>

<script>
export default {
  props: {
    active: {
      type: Object,
      required: true
    },
    opts: {
      type: Object,
      required: true
    },
    editField: {
      type: String,
      required: true
    },
    value: {
      type: [String, Number],
      required: false
    }
  },
  data: () => ({
    emit: null,
    edit: false
  }),

  computed: {
    computedValue: {
      get() {
        return this.value;
      },
      set(value) {
        this.emit = value;
      }
    },
    rules() {
      let value = this.value;
      let required = true;
      if (this.opts.required) {
        required = value => !!value || "Required.";
      }
      return [required];
    },
    required() {
      if (this.opts.required) {
        return "*";
      } else {
        return "";
      }
    }
  },
  methods: {
    emitFocus() {
      this.$emit("fieldFocused", this.opts.field);
    },
    emitChange() {
      //this.$emit("act", this.emit, this.opts.field);
      if (this.emit != null) {
        this.$emit("act", this.emit, this.opts.field);
      }
    }
  },
  watch: {
    emit() {
      this.$emit("input", this.emit);
    },
    value() {
      this.emit = this.value;
    },
    editField() {
      if (this.editField == this.opts.field) {
        this.edit = true;
      } else {
        this.edit = false;
      }
    }
  }
};
</script>

<style lang="scss" scoped>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.1s;
}
.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
  opacity: 0;
}
</style>
```

## Creating new resources

The form can be used to create new resources. It does this by using the vue [sync modifier](https://vuejs.org/v2/guide/components-custom-events.html#sync-Modifier) The sync modifier emits the input data out of appForm with this syntax:

```
this.$emit(`update:${this.selectedField}`, newValue);
```

The actual resource creation is handled by the parent component that houses the appForm component. In other words, AppForm is accepting the value from the input component and then passing it on the parent to be created.

## Updating resources

Appform communicates directly with the vuex store for the purpose of updating. This is done using the `v-on:act="update"` event on the dynamic component.

Every time an input component is changed, it will emit back to edge-form an "act" event. edge-form will accept this event and call the update function.

The update must have two params: e: updated value; field: the input fielding being updated.

```
async update(e, field) {
      if (this.loading) return;
      if (!this.validate(e, field)) {
        return;
      }
      this.loading = true;

      let resource = {};
      resource[field] = e;
      let obj = {};

      if (this.meta) {
        this.$emit("act", e, field);
      } else {
        obj = {
          id: this.active.id,
          payload: resource
        };

        let success = await this.$store.dispatch(`${this.path}/update`, obj);
        if (success) {
          this.$emit("success", e);
        } else {
        }
      }
      this.loading = false;
      if (this.subUpdate) {
        this.$store.dispatch(`${this.parentPath}/list`);
      }
    }
```

The update function will validate the input and then package it with the `active.id` and dispatch it to the proper vuex store using `path`

Notes: I need to investigate the use of meta in the update field. I don't believe it is actively in use, but I want to double check before I remove it. If it is in use, it is a prime candidate for removal.

Validation has limited functionality here, though it is needed. It is only checking for required fields.

## File Upload

We often have the need to upload documents through the form - email attachments, page images, etc. They are a special circumstance and needed to be handled directly by appForm.

## Tests

No tests have been completed.

#### unit tests to complete

- test appearance of components
- validation
- test the output of .sync modifier
- test the output of the update functions





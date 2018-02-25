---
layout: default
id: index
---

## Introduction

This library allows you to present hierarchically organized data in a nice and logical manner based on [VueJS](http://vuejs.org) framework.
There are lots of libraries but always something was missing (in my humble opinion). It is an attempt to make a __perfect__ tree.

`Just try it. The tree you were waiting for!`


### Features
* mobile friendly
* events for every action
* flexible configuration
* any number of instances per page
* multi selection
* keyboard navigation




## Getting Started

### Installation

- npm: `$ npm install --save liquor-tree`
- Yarn: `$ yarn add liquor-tree`

It has to be installed to VueJS instance. Please take a look at the [official documentation](https://vuejs.org/v2/guide/components.html) to understand how to use VueJS components (if it needs of course).

You no need to care about styles, they are automatically appended to the document.


**When used with a module system there are 3 ways to registrate the component (maybe more... I don't know).
Okay. It's our ways:**

``` javascript
import Vue from 'Vue'
import LiquorTree from 'liquor-tree'

// global registration
Vue.use(LiquorTree)
```

``` javascript
import Vue from 'Vue'
import LiquorTree from 'liquor-tree'

// global registration
Vue.component(LiquorTree.name, LiquorTree)
```

``` javascript
import LiquorTree from 'liquor-tree'

export default {
  name: 'your-awesome-component',
  components: {
    // you can name tree as you wish
    [LiquorTree.name]: LiquorTree
  },
  ...
}
```

To registrate the library you can choose between 3 ways I mentioned before.

**When used directly in browser you are able to include `liquor-tree` via CND:**

```html
<script src="https://cdn.jsdelivr.net/npm/liquor-tree/dist/liquor-tree.umd.js"></script>
```

### Component Options

| Name                   | Type         |  Default  | Description |
|------------------------|:------------:|:---------:|-------------|
| **multiple**           | Boolean   | true    | Allows to select more that one node. Ignored in `checkbox` mode. In `checkbox` mode it always possible to select multiple nodes   |
| **checkbox**           | Boolean   | false   | `checkbox` mode. It shows checkboxes for every nodes            |
| **checkOnSelect**      | Boolean   | false   | For `checkbox` mode only. Node will have `checked` state when user clicking either text or checkbox |
| **autoCheckChildren**  | Boolean   | true    | For `checkbox` mode only. Children will have the same 'checked' state as their parent. |
| **parentSelect**       | Boolean   | false   | By clicking node which has children it expands node. i.e we have two ways to expand/collapse node: by clicking on arrow and on text |
| **keyboardNavigation** | Boolean   | true    | Allows user to navigate tree using keyboard |
| **propertyNames**      | Object    | -       | This options allows to redefine default tree's structure. [See example above](#Redefine-Structure) |
| **deletion**           | Boolean	&#124; Function | false | If **keyboardNavigation** is false - this property is ignoring. This property defined deletion behaviour. [See example](#Keyboard-Navigation) |




### Structure

The component has only two props: **data** and **options**. More about props read [VueJS documentation](https://vuejs.org/v2/guide/components.html#Passing-Data-with-Props)

- property **options** - This property defines tree behavior. See [Component Options](#Component-Options)
- property **data** - Array-like object that defines tree nodes

Property **data** has its own structure for every node:

``` javascript
{
  "id": Int,
  "text": String,
  "data": Object,
  "children": Array,
  "state": Object
}
```


* `id`: By default if node didn't have an id it will be generated randomly
* `text`: Label for Node
* `data`: Intermediate data for each node. It can be everything you want. This objects creates for every node and VueJS makes this property reactive.
* `children`: List of children nodes.
* `state`: Allows user to set Node's state.

  By default Node has states: 
    ```javascript
    {
      selected: false,
      selectable: true,
      checked: false,
      expanded: false,
      disabled: false,
      visible: true,
      indeterminate: false
    }
    ```
  It is not necessary to pass all the states for every Node. It will automatically merged with default states object

Initial **data** example:

``` javascript
  const treeData = [
    { text: 'Item 1', state: { visible: false } },
    { text: 'Item 2' },
    { text: 'Item 3', state: { selected: true } },
    { text: 'Item 4' },
    { text: 'Item 5', children: [
      { text: 'Item 5.1', state: { disabled: true } },
      { text: 'Item 5.2', state: { selectable: false } }
    ]}
    // and so on ...
  ]
```



### Basic Usage

#### ES6 (using vue-loader)

``` javascript
<template>
  <div id="app">
    <tree
      :data="treeData"
    />
  </div>
</template>

<script>
  import Vue from 'Vue'
  import LiquorTree from 'liquor-tree'

  Vue.use(LiquorTree)

  export default {
    name: 'awesome-component',
    data: () => ({
      treeData: [
        { text: 'Item 1' },
        { text: 'Item 2' },
        { text: 'Item 3', state: { selected: true } },
        { text: 'Item 4' }
      ]
    })
  }

</script>
```

#### If you are using CDN that's all you need to build your first app (I mean app that using `VueTree` library)

In this way you no need to registrate the library as a component.

``` html
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
</head>
<body>
  <div id="app">
    <tree
      :data="treeData"
      :options="treeOptions"
      @node:selected="onNodeSelected"
    />
  </div>
</body>
  <!-- first import Vue -->
  <script src="https://unpkg.com/vue/dist/vue.js"></script>
  <!-- import JavaScript -->
  <script src="https://cdn.jsdelivr.net/npm/liquor-tree/dist/liquor-tree.umd.js"></script>
  <script>
    new Vue({
      el: '#app',
      data: function() {
        return {
          treeData: [
            // see above the format of data
          ],
          treeOptions: {
            miltiple: false
          }
        }
      }
    })
  </script>
</html>
```

## Guides

### Basic Features

This example demonstrates default behaviour of tree without any configurations. Each node from received data has its **own states properties** ([view full list](#node-atata))

<iframe width="100%" height="500" src="//jsfiddle.net/amsik/25bv7nh0/embedded/html,result/dark/" allowpaymentrequest allowfullscreen="allowfullscreen" frameborder="0"></iframe>

You able to select multiple nodes with Ctrl key. The same behavior as we are used to ;)

### Checkboxes

It was a default mode. You can switch it to `checkbox` mode. To do it you have to add tree's option:

``` html
    <tree
      :data="treeData"
      :options="{ checkbox: true }"
    />
```

<iframe width="100%" height="500" src="//jsfiddle.net/amsik/ewcy3jee/embedded/html,result/dark/" allowpaymentrequest allowfullscreen="allowfullscreen" frameborder="0"></iframe>

> States of node like **checked** and **selected** are not interchangeable. They can be uses together.


### Redefine Structure
  This component has `strict` structure. But! You can easily **redefine** this format. Yeah... Sometimes you don't want to change your server-side code and you have very different format for tree. To do this you just need to send the object:

For instance you have data:

``` javascript
[
 { 'SOME-AWESOME-PROPERTY-FOR-TEXT': 'Item 1' },
 { 'SOME-AWESOME-PROPERTY-FOR-TEXT': 'Item 2' },
 { 'SOME-AWESOME-PROPERTY-FOR-TEXT': 'Item 3', 'kids': [
  { 'SOME-AWESOME-PROPERTY-FOR-TEXT': 'Item 3.1' },
  { 'SOME-AWESOME-PROPERTY-FOR-TEXT': 'Item 3.2' }
 ]}
]

```

You just need to add `propertyNames` options to **Component Options**:


``` javascript
 {
  'text': 'SOME-AWESOME-PROPERTY-FOR-TEXT',
  'children': 'kids'
 }
```

Then your data will be transformed to readable tree format. Awesome! See [example](#Redefine-Structure-Example) below


### Keyboard Navigation

By default **keyboardNavigation** options is true. It allows user to navigate tree using keyboard. Navigation is implemented in the usual way (i.e Windows Navigation pane). Disabled Nodes are ignoring.

Also you have abillity to define condition to remove Node. To do this, determine the **deletion** component option.
It receives Boolean object (default is false) or Function.

  - If this property received a `true` it will remove selected Node
  - If this property revieved a `function` it will remove Node **IF** the `function` return `true`

Ohh, too hard. See example:

- In this case Node will be removed

```javascript
    <tree
      :data="treeData"
      :options="{ deletion: true }"
    />
```

- In this case if Node doesn't has a children it will be removed

```javascript
    <tree
      :data="treeData"
      :options="{ deletion: node => !node.hasChildren() }"
    />
```
Try [Checkboxes](#Checkboxes) to remove ONLY nodes that has `checked` state 


## Examples

### JSON Viewer

I did this in 40 min ... do not judge me strictly :)
In plans:
- online editor
- to reveal all the possibilities of slots
- process in real time

<iframe width="100%" height="500" src="//jsfiddle.net/amsik/mknwyqjc/embedded/html,css,result/dark/" allowpaymentrequest allowfullscreen="allowfullscreen" frameborder="0"></iframe>

### Radio

This example shows how to replace default content. It's allows you to control a content in any way. It possible thanks to VueJS scoped slots.

<iframe width="100%" height="500" src="//jsfiddle.net/amsik/6jm2b1dq/embedded/html,result/dark/" allowpaymentrequest allowfullscreen="allowfullscreen" frameborder="0"></iframe>

### Redefine Structure Example

For instance: we had a super-tree-component with its own structure (never mind, just for test). And we have a lot of dependencies to that component. This example shows how to apply data from server without a headache.

In this example we have structure:
```javascript
[
  MY_TEXT: 'some text', KIDS: []
]
```

A library doesn't know this format. But we can add `propertyNames` options and redefine structure. See example


<iframe width="100%" height="500" src="//jsfiddle.net/amsik/y7190jkd/embedded/html,result/dark/" allowpaymentrequest allowfullscreen="allowfullscreen" frameborder="0"></iframe>


## API



### Tree API

To have directly access to the Tree API you have to use `ref` property of component. See more [Child-Component-Refs](https://vuejs.org/v2/guide/components.html#Child-Component-Refs).

#### [Tree.find(criteria, [multiple = false])](#Tree-find-criteria-multiple-false)

- **Arguments:**
  - `{ Object | String } criteria`
  - `{ Boolean } multiple`

- **Returns:**
  - [Selection](#Selection-API)

- **Usage:**

  This method uses in every methods where you can find node by criteria. If `criteria` is passed as string it will be transformed like `{ text: criteria }`.
  Examples:
```javascript
  this.$refs.tree.find('Node Text') // It will find Node that has text 'Node Text'
  this.$refs.tree.find(/Node Text/) // Using RegExp. It will find: Node Text ATAT, ATATA Node Text and so on...
  this.$refs.tree.find({
    text: /^Item 2/,
    state: { checked: true, selected: true }
  })
```
By default this method finding the first found node. You can add the second parameter and this function will return all found nodes. 


#### [Tree.findAll(criteria)](#Tree-findAll-criteria)

- **Arguments:**
  - `{ Object | String } criteria`

- **Returns:**
  - [Selection](#Selection-API)

- **Usage:**

This method "syntactic sugar" of `Tree.find(criteria, true)`



#### [Tree.selected()](#Tree-selected)

- **Returns:**
  - [Selection](#Selection-API)

- **Usage:**

  You can get access to `selected` nodes and do everything you want with NodeAPI


#### [Tree.checked()](#Tree-checked)

- **Returns:**
  - [Selection](#Selection-API)

- **Usage:**

  You can get access to `checked` nodes and do everything you want with NodeAPI


#### [Tree.append(criteria, node)](#Tree-append-criteria-node)

- **Arguments:**
  - `{ Object | Node } criteria`  (see [find method](#Tree-find-criteria-multiple-false))
  - `{ Object | String } node`

- **Returns:**
  - Appended Node
  - null

- **Usage:**

  This method allows you to append (add to the end of the list) new Node to the Tree.
  There are **2 types of insertion:**
    - Set criteria and new Node. It will try to find Node (using criteria) and append as children of this Node. Example:
```javascript
  this.$refs.tree.append(
    { text: 'My super Text' },              // search criteria
    'New CHILD Node for "My super Text"'    // this string will be converted to Node object with default state parameters
  )
```
    - Set only one argument (Node). In this way it will add new Node as `root` element. (Yeah, we are able to have more than one root element)
```javascript
  this.$refs.tree.append({
    text: 'My NEW Node',
    state: { selected: true }
  })
```

#### [Tree.prepend(criteria, node)](#Tree-prepend-criteria-node)

- **Usage:**

  This method has behaviour the same as `Tree.append`. But insertion place will be different (**in the start** of the list)


#### [Tree.before(criteria, node)](#Tree-before-criteria-node)

- **Usage:**

  This method has behaviour the same as `Tree.append`. But insertion place will be different (**before** found node or the start of the list (as root))


#### [Tree.after(criteria, node)](#Tree-after-criteria-node)

- **Usage:**

  This method has behaviour the same as `Tree.append`. But insertion place will be different (**after** found node or the end of the list (as root))


#### [Tree.remove(criteria, multiple = false)](#Tree-remove-criteria-multiple)

- **Arguments:**
  - `{ Object | String } criteria` (see [find method](#Tree-find-criteria-multiple-false))
  - `{ Boolean } multiple`

- **Returns:**
  - [Selection](#Selection-API)

- **Usage:**

  Remove Node by criteria.


### Selection API

In progress...

### Node API

In progress...

### Events

In progress...
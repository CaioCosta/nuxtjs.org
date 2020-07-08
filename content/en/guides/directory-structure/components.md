---
title: components
description: The components directory contains your Vue.js Components. Components are what makes up the different parts of your page and can be reused and imported into your pages, layouts and even other components.
position: 3
category: directory-structure
csb_link: https://codesandbox.io/embed/github/nuxt-academy/guides-examples/tree/master/04_directory_structure/03_components?fontsize=14&hidenavigation=1&theme=dark
questions:
  - question: 
    answers:
      - 
      - 
      - 
    correctAnswer: 
  - question: 
    answers:
      - 
      - 
      - 
    correctAnswer: 
  - question: 
    answers:
      - 
      - 
      - 
    correctAnswer: 
  - question: 
    answers:
      - 
      - 
      - 
    correctAnswer: 
---

The components directory contains your Vue.js Components. Components are what makes up the different parts of your page and can be reused and imported into your pages, layouts and even other components.

### Fetching Data

When working with data inside components we cannot use asyncData as this only works in page components. To access data from an API in your components you can use the Nuxt fetch(). Using `$fetchState.pending` we can show a message when the data is waiting to be loaded and using `$fetchState.error` we can show an error message if there is an error fetching the data. When using fetch we must declare the data in the data property. This then gets filled with the data that comes from the fetch.

`components/MountainsList.vue`

```html
<template>
<div>
    <p v-if="$fetchState.pending">Loading....</p>
    <p v-else-if="$fetchState.error">Error while fetching mountains</p>
    <ul v-else>
        <li v-for="(mountain, index) in mountains" :key="index.id">
           {{ mountain.title }}
        </li>
    </ul>
</div>
</template>
<script>
export default {
  data() {
    return {
      mountains: []
    }
  },
  async fetch() {
      this.mountains = await fetch('https://api.nuxtjs.dev/mountains')
      .then(res => res.json())
  },
}
</script>
```

➡️ See the chapter on [fetch()](/guides/features/data-fetching#the-fetch-method) for more details on how fetch works

## Components Module

With Nuxt.js you can now create your components and auto import them into your .vue files meaning there is no need to manually import them in the script section. Nuxt.js will scan and auto import these components for you and can be used in pages, layouts or even other components. 

If you are using Nuxt 2.13+ then all you need to do is set components to true in your nuxt.config file. The [components module](https://github.com/nuxt/components) is included by default but it is not activated by default. 

`nuxt.config.js`

```js
export default {
  components: true
}
```

If you are suing Nuxt 2.10+ you need to first import the module

<code-group>
  <code-block label="Yarn" active>

  ```bash
  yarn add -D @nuxt/components
  ```

  </code-block>
  <code-block label="NPM" >

  ```bash
  npm install --save-dev @nuxt/components
  ```

  </code-block>
</code-group>

And then add the module to the build modules section of your nuxt.config.js

`nuxt.config.js`

```bash
export default {
  buildModules: [
    '@nuxt/components'
  ]
}
```

Once you create your components in the components directory they will then be available to be auto imported.

`components directory`

```html
components/
  TheHeader.vue
  TheFooter.vue
```

`layouts.default.vue`

```html
<template>
  <TheHeader />
	<Nuxt />
  <TheFooter />
</template>
<script>
</script>
```

### Dynamic Imports

To dynamically import a component also known, as lazy loading a component, all you need to do is add the lazy prefix in your templates. 

`layouts.default.vue`

```html
<template>
  <TheHeader />
	<Nuxt />
  <LazyTheFooter />
</template>
<script>
</script>
```

Using the lazy prefix you can also dynamically import a component when an event is triggered.

`pages/index.vue`

```html
<template>
    <div>
        <h1>Mountains</h1>
        <LazyMountainsList v-if="show" />
        <button v-if="!show" @click="showList">Show List</button>
    </div>
</template>

<script>
export default {
  data() {
    return {
      show: false
    }
  },
  methods: {
    showList() {
      this.show = true
    },
  }
}
</script>
```

### Nested Directories

If you have components in nested directories such as:

```bash
components/
  base/
    Button.vue
```

The component name will be based on its own filename. Therefore the component will be:

```html
<Button />
```

We recommend you use the directory name in the filename for clarity

```bash
components/
  base/
    BaseButton.vue
```

However if you want to keep the filename as Button.vue then you can use the prefix option in the nuxt config to add a prefix to all components in a specific folder.

```bash
components/
  base/
   Button.vue
```

`nuxt.config.js`

```bash
components: {
    dirs: [
      '~/components',
      {
        path: '~/components/base/',
        prefix: 'Base'
      }
    ]
  }
```

And now in your template you can use the BaseButton instead of Button without having to make changes to the name of your `Button.vue` file.

```html
<BaseButton />
```

<app-modal>
  <code-sandbox  :src="csb_link"></code-sandbox>
</app-modal>

➡️ To learn more about the [components module](/blog/improve-your-developer-experience-with-nuxt-components)
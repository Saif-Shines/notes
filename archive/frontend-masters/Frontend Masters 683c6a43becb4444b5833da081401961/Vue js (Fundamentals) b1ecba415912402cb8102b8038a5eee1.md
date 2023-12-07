# Vue.js (Fundamentals)

Course Repository

[GitHub - bencodezen/complete-intro-to-vue-3-workshop](https://github.com/bencodezen/complete-intro-to-vue-3-workshop)

# Overview

A javascript frameworks for building web interfaces.

Why?

1. Approchable - Builds on top of standard HTML, CSS and JavsScript with intuitive API and world class documentation.
2. Performant
3. Versatile - A rich, incremetnally adoptable ecosystem that scales from a library and full featured framworks.
4. Community first - Google Careers,

**Prerequisites** 

[Introduction | Vue.js](https://vuejs.org/guide/introduction.html)

******CDN******

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8" />
  <meta http-equiv="X-UA-Compatible" content="IE=edge" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Hello Frontend Masters!</title>
</head>

<body>
  <h1>Hello Frontend Masters!</h1>
  <div id="app">
    {{ message }}
  </div>
  <script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
  <script>
    const { createApp } = Vue;
    const app = createApp({
      data() {
        return {
          message: "Hello it works!"
        }
      }
    });
    app.mount("#app")
  </script>
</body>

</html>
```

## Event Handling & View Model

There is some important concept to learn on this case.

Consider following is under the `<script>`

```jsx
Vue.createApp({
    data: () => {
      return {
        count: 10,
        incrementAmount: 8,
        message: "Hello it works!",
        listOfItems: [1, 2, 3, 4, 5]
      }
    },
    methods: {
      incrementCount() {
        this.count += this.incrementAmount
      },
      changeIncrementAmount(event) {
        this.incrementAmount = Number(event.target.value);
      }
    }
  }).mount('#app')
```

Now, the `HTML`

```jsx
<div id="app">
  <h1>Counter</h1>
  <p>{{count}}</p>
  <button v-on:click="incrementCount">Increment Count</button>
  <div>
    <label for="incrementAmount"> Increment By: </label>
    <input type="number" v-bind:value="incrementAmount"
      v-on:input="changeIncrementAmount" />
  </div>
</div>
```

1. It appears like the approach of `createApp()` and passing options like `data` and `methods` is called Vue’s Options API.
2. The `data` is first and foremost is not just a property that holds any primitive value, but instead it’s a function that returns. It’s in the return statement, we tend to define scope of variable for our current rendering context - under `#app`.
3. The `methods` are bits of functionality that can be invoked when user events occur. For example, `v-on:click="incrementCount"`  on the  button HTML element, can track andupdate the `count` value using `this.count` to access scope variable under `#app`.

v-model

```html
<div>
    <label for="incrementAmount"> Increment By: </label>
    <input type="text" v-model="incrementAmount" aria-label="a" />
 </div>
<script>
Vue.createApp({
    data: () => {
      return {
        count: 10,
        incrementAmount: 8, // this value is tracked to the equivalent v-model
        message: "Hello it works!",
        listOfItems: [1, 2, 3, 4, 5]
      }
    },
    methods: {
      incrementCount(newAmount, event) {
        console.log(newAmount, event)
        console.log("incrementAmount", this.incrementAmount)
        this.count += Number(this.incrementAmount)
      }
    }
  }).mount('#app')
</script>
```

**Shortcuts** — `@` and `:`

```html
<button @click="incrementCount">Increment Count</button>

<!-- is equivalent to: -->

<button v-on:click="incrementCount">Increment Count</button>
```

and

```html
<input type="number" v-bind:value="incrementAmount" v-on:input="changeIncrementAmount" />

<!-- is equivalent to: = shortcuts v-bind -->

<input type="number" :value="incrementAmount" @input="changeIncrementAmount" />
```

******************Passing Events******************

```html
<button @click="incrementCount(10, $event)">Increment Count</button>
<script>
Vue.createApp({
 data: () => {
		
	},
	methods:{
		incrementCount(newValue, event){
			newValue; // 10 
			event; // PointerEvent with Event properties
			// $ symbol only ensure you pass the argument in parameter you would like to catch in the fn.
		}
	}
})
</script>
```

********Keyups********

```html
<div v-else-if="characters.includes(answer.toLowerCase())">{{right}}
      <label for="addCharacter">Did we miss an character? Add it!</label>
      <input type="text" id="addCharacter" v-model="newCharacter">
      <button @click="addToCharacterList(newCharacter)" 
class="favorite-btn">Add</button>
</div>

<!-- Notice that you have to add a button in order to save a new character -->
```

Instead you can do this 

```html
<div v-else-if="characters.includes(answer.toLowerCase())">{{right}}
      <label for="addCharacter">Did we miss an character? Add it!</label>
      <input type="text" 
						 id="addCharacter" 
						v-model="newCharacter"
						@keyup.enter = "addToCharacterList"
>
</div>

<!-- Keyup observes teh keys. `.enter` invokes the addToCharacterList() method only when you want to save the character name (this.newCharacter) in the addtoChracterList's execution context-->
```

# Watchers & Computed Properties

**********************************watch vs computed**********************************

These are two different parts of Options API that Vue offers. 

- Computed is a lot more helpful when you are managing reative data - the data where there are data and methods dependencies and actively change with user inputs.
- Watch is good for triggering based on user interaction. It’s basically not constantly checking, and destroying too many notes.

```jsx
Vue.createApp({

computed: {
      displayTitle() {
        if (this.count > 30)
          return 'Premium Title'
        else
          return 'Just Standard'
      },
      optimizedIncrementAmount() {
        return this.displayTitle.length * this.incrementAmount;
      }
    },
    watch: {
      count(newValue) {
        newValue
        if (newValue > 20)
          this.counterTitle = 'Premium Title'
        else
          this.counterTitle = 'Just Standard'
      }
    }
})

```

### Replacing JQuery with Vue.js

[Replacing jQuery With Vue.js: No Build Step Necessary — Smashing Magazine](https://www.smashingmagazine.com/2018/02/jquery-vue-javascript/)

# Vue Tools

Although companies like GitLab have used CDN way to slowly do their migrations, a full fledged enterprise application doesn’t really build Vue Apps this way.

VS Code Plugin for Syntax Highlighting - Volar

Debugging tool on chrome - Vue Developer Tools

## Custom Components

Components are a way to modularise your app. For example, you might have a statistics section that depends on particular `data`, `computed` or `methods`. It’s only logical to break it down into scoped components with their won `<template>` and  `<script>`.

```bash
# The `src` directory
.
├── App.vue
├── assets
│   ├── base.css
│   ├── logo.svg
│   ├── main.css
│   └── old_main.css # I moved the original css rules to this file, because at this time I don't know how to use componenet level <style>
├── components
│   └── FriendsStats.vue
└── main.ts
```

Create a new file named `FriendsStats.vue`

```jsx
<script>
export default {
  props: {
    characters: {
      type: Array,
      required: true
    }
  },
  computed: {
    totalAgeOfGroup() {
     ...
    },
    getRelationshipCount() {
      ...
			// depends on characterList
			...
    }
  }
}
</script>

<template>
  <h2>Statistics</h2>
  <ul>
    <li v-for="[index, value] in Object.entries(getRelationshipCount)" :key="`${index}`">
      {{ index }}:
      {{ value }}
    </li>
  </ul>
  <div class="stats">
    The total age of the F.R.I.E.N.D.S Group: <mark>{{ totalAgeOfGroup }}</mark>
  </div>
</template>
```

1. Notice we simply moved the section of HTML into `<template>` and eveything that’s dependant into `script`.
2. The only difference is is `script` will `export default {}` which is equivalent to `{..}` passed to `Vue.createApp({..})`

Allow `App.vue` to render `FriendsStats.vue`

```jsx
<script>
import FriendsStats from '@/components/FriendsStats.vue'

export default {
	components:{
		FriendsStats
	},
	data():{ 
		return {
		foo: "",
		bar: "",
		characterList: []
	}
},
	computed: {
		foo(){..},
		bar(){..}
	},
	methods:{
		foo(){..},
		bar(){..}
	}

}
</script>

<template>
<h1> Title </title>
<FriendsStats :characters="characterList"/>
</template>
```

This is not enough to successfully render the component, because the statistics are really dependant upon the `characterList` to which `computed` methods are invoked. About the right time to introduce Props.

## Props

Let’s assume:

```markdown
App.vue (root) -> FriendsStats.vue (child)

- App.vue's - data = characterList is used by FriendsStats.vue
```

Observe

```jsx
// App.vue

<FriendsStats :characters="characterList"/>
```

```jsx
// FriendsStats.vue
  props: {
    characters: {
      type: Array,
      required: true
    }
```

## Emitting Events

Consider you want an to invoke a method in the parent component when click event actually occurs on the child? One of basic things you might do is to pass a method to the child props, and invoke that prop-passed-down-method to orignally invoke method on the parent component.

This is not how web was built. 

The web has this concept of bubbling up events. We will use the same approach.

```jsx
src
├── App.vue // parent
├── components
│   ├── base-counter.vue
│   └── user-card.vue // child
├── assets
│   ├── base.css
│   ├── logo.svg
│   └── main.css
└── main.ts
```

1. Usually to pass data down to the components from parents to child, you use Props
2. To bubble up events from child to parents, use `emits`

Here’s an example

```html
// child
<script>
export default {
  data() {
    return {
      name: 'Anjana'
    }
  },
  props: {
    counterTitle: {
      type: String,
      required: true
    }
  },
  emits: ['change-name']
}
</script>

<template>
  <h1>User: {{ counterTitle }}</h1>
  <button @click="$emit('change-name')">change name</button>
</template>
```

```html
// parent
<script>
import BaseCounter from '@/components/base-counter.vue'
import UserCard from '@/components/user-card.vue'

export default {
  components: {
    BaseCounter,
    UserCard
  },
	data(){
	  counterTitle: 'Counter Standard',
		count: 389
	},
	methods:{
		changeName(){
			this.counterTitle = "New CounterTitle";
			// this.count = 0 doesn't work because count is not in props of child. Only :counterTitle is.
		}
	}
}
</script>

<template>
<UserCard @change-name="changeName" :counterTitle="counterTitle" />

</template>
```

- The emitted `change-name` event will change only `counterTitle` - notedly used by both *************parent & child************* components.
- If let’s say `change-name` tries to update `count` value from `389` to `0`.

## Emitting Custom events

```html
// CharacterCard

<script>
export default {
  props: {
    capitalize: {
      type: Function,
      required: true
    },
    character: {
      type: Object,
      required: true
    }
  },
  emits: ['favorite'],
  methods: {
    favoriteCharacter(actor) {
      this.$emit('favorite', actor)
    }
  }
}
</script>

<template>
  <div>
    <h2>Select a Favorite Character:</h2>
    <ul>
      <li v-for="(actor, index) in character" :key="`${index}`">
        {{ capitalize(actor.name) }}
        <button class="favorite-btn" @click="favoriteCharacter(actor)">Mark Favorite</button>
      </li>
    </ul>
  </div>
</template>
```

```html
// App.vue
    
<script>

export default {
	methods: {
    addtoFavList(payload) {
      console.log('value', payload)
      this.favoriteCharacters.push(payload)
      console.log(this.favoriteCharacters)
  }
}

</script>

<template>
<CharacterCard
      :capitalize="capitalizeFirstWord"
      :character="characterList"
      @favorite="addtoFavList"
    /> 

 </template>
```

## Slots

More often than not, you might come across cases where you will want to pass everything to the child components. Until know, we used to pass the data to the child components through only Props. 

```html
//Parent

<script>
import Title from '@/components/Title.vue'
export default {
	components:{
		Title
	}
}
</script>

<template>
  <TitleName>
    <template v-slot:niche> // one way of declaring
      <FriendsStats :characters="characterList" />
    </template>
    <template #upar></template> // another way of declaring
  </TitleName>
</template>
```

```html
// Child

<script>
export default {
  data() {
    return {
      title: 'Who are the characters in the show?'
    }
  }
}
</script>

<template>
  <h1>{{ title }}</h1>
  <slot name="upar"></slot>
  <slot name="niche"></slot>
</template>
```

# Fetching Data

## Lifecycle Events

Let’s say you want to list a bunch of pokemons from a 3rd party endpoint. So far from what we learned we can only make this API call on a button click, or wait for any data to change so that reactively our fetchPokemons method is invoked by Vue. 

There’s a better way to do this through something called Vue Lifecycle Components. 

[Lifecycle Hooks | Vue.js](https://vuejs.org/guide/essentials/lifecycle.html#lifecycle-diagram)

If you look at it’s life cycle [diagram](https://vuejs.org/assets/lifecycle.16e4c08e.png), you can use every toll-gate such as `beforeMount`, `created` as one of the properties through Vue.js options API. Here’s how it works:

```html
<script>
export default {
  data() {
    return {
      pokedex: [1, 2, 3, 4, 5]
    }
  },
  methods: {
    async fetchPokeAPI() {
      const url = 'https://pokeapi.co/api/v2/pokemon?limit=151'
      let response = await fetch(url)
      response = await response.json()
      this.pokedex = response
    }
  },
  created() { // Lifecycle Hook renders the list of API response when
    console.log('created', this.pokedex)
    this.fetchPokeAPI()
  }
}
</script>

<template>
  <h1>New App</h1>
  <pre> {{ pokedex }}</pre>
  <button @click="fetchPokeAPI">Fetch Pokemon</button>
</template>
```

## Generic Components

Let’s say I am building a page (single page application) withough any URL routing capabilties. The page has two options “Login” and “Home”. 

```html
<template>
  <header class="header">
    <span class="logo">
      <img src="@/assets/vue-heart.png" width="30" />C'est La Vue
    </span>
    <nav class="nav">
      <a href="#" @click.prevent="showHomePage">Home</a>
      <a href="#" @click.prevent="showLoginPage">Login</a>
    </nav>
  </header>
  <HomePage v-if="currentPage === 'Home'" />
  <LoginPage v-else />
</template>
```

```html
<script>
import HomePage from "./components/HomePage.vue";
import LoginPage from "./components/LoginPage.vue";

export default {
  components: {
    HomePage,
    LoginPage,
  },
  data: () => ({
    currentPage: "Home",
  }),
  computed: {
    renderPage() {
      return `${this.currentPage}Page`;
    },
  },
  methods: {
    showHomePage() {
      this.currentPage = "Home";
    },
    showLoginPage() {
      this.currentPage = "Login";
    },
  },
};
</script>

<template>
  <header class="header">
    <span class="logo">
      <img src="@/assets/vue-heart.png" width="30" alt="tagline" />C'est La Vue
    </span>
    <nav class="nav">
      <a href="#" @click.prevent="showHomePage">Home</a>
      <a href="#" @click.prevent="showLoginPage">Login</a>
    </nav>
  </header>
  <component :is="renderPage" />
</template>
```

# Composition API

Composition API lets to write vanilla javascript, and let it interact with Vue without options API. The easy organisation and abstraction such as `data: ()=>({})`, `methods` etc., is no longer available.

You define your code in a new `setup()` context as follows:

```html
<script>
import { reactive } from "vue";

export default {
  async setup() { // The #1 thing to happen in Lifecycle Events
		const state = reactive({
			regionName = 'tenali'
		});  // previously was on data
    const url = 'https://pokeapi.co/api/v2/pokemon?limit=151'
    let response = await fetch(url) // previouslay on method
    let pokedex = await response.json()
		console.log(state.regionName); // 'tenali' note: no-this within setup()
    return {
      state, // treated as reactive data
      regionName
    }
  },
  created() {
    console.log(this.pokedex) // other parts of API can reference them
    console.log(this.state.regionName) // more like data from options API
  }
}
</script>

<template>
  <pre> {{ pokedex }}</pre>
</template>
```

In short, we make the async API call at the very first life cycle event of the Vue component, and listed the response to the UI. 

## Suspense

************Experimental************

```bash
.
components/pokedex.vue 
App.vue
```

<aside>
💡 In the root `App.vue` file, do not use `async setup()` because, vue doesn’t know what to render until `setup()` lifecycle events has all of its async operations finish. So it’s always a must to pass async operations and related template to pass to a diffferent component. in this case `pokedex.vue`

</aside>

When you move the logic of making API call and rendering to the `pokedex.vue` component, then what does the Root `App.vue` should render? That’s where `<Suspense> </Suspense>` comes into picture. This should be used as follows:

```html
<script>
import Pokedex from '@/components/pokedex.vue'

export default {
  components: {
    Pokedex
  }
}
</script>

<template>
  <h1>New App</h1>
  <Suspense>
    <Pokedex /> 
		<!-- the following template will be loaded until
     Pokedex above finshes its async operations -->
    <template [v-slot:fallback](https://www.notion.so/Module-aware-installation-parameters-b58a33c1e29d4c3085ccf14d6df29cbe?pvs=21)> Pokemon is Loading... </template> 
  </Suspense>
</template>
```

## script setup

```html
<script>
import { ref, computed, reactive } from 'vue'

export default {
  async setup() {
    const regionName = ref('Tenali')

    const state = reactive({
      placeType: 'Typically Hot',
      book: '12 rules for life'
    })

    const listInCaps = computed(() => {
      return state.placeType.toUpperCase()
    })

    const url = 'https://pokeapi.co/api/v2/pokemon?limit=151'
    let response = await fetch(url)
    let pokedex = await response.json()

    console.log(state.placeType)
    return {
      pokedex,
      regionName,
      state,
      listInCaps
    }
  },
  methods: {
    changeRegionName() {
      this.regionName = 'Himalayas'
    }
  },
  created() {
    // console.log(this.pokedex)
    // console.log(this.regionName)
    // console.log(state.placeType)
  }
}
</script>

<template>
  <h1>{{ regionName }}</h1>
  <h2>{{ state.book }}:{{ listInCaps }}</h2>
  <button @click="changeRegionName">Change Region Name</button>
  <pre> {{ pokedex }}</pre>
</template>
```

```html
<script setup>
import { ref, reactive, computed } from 'vue'
import BaseCounter from '@/components/base-counter.vue'

const regionName = ref('Tenali')

const state = reactive({
  placeType: 'Typically Hot',
  book: '12 rules for life'
})

const listInCaps = computed(() => {
  return state.placeType.toUpperCase()
})

const changeRegionName = () => {
  regionName.value = 'Himalayas'
}

const url = 'https://pokeapi.co/api/v2/pokemon?limit=151'
let response = await fetch(url)
let pokedex = await response.json()
</script>

<template>
  <h1>{{ regionName }}</h1>
  <h2>{{ state.book }}:{{ listInCaps }}</h2>
  <BaseCounter />
  <button @click="changeRegionName">Change Region Name</button>
  <pre> {{ pokedex }}</pre>
</template>
```

1. adding setup within in the script tag
2. natuarlly write javascript
3. even add another component without even registering it

```html
<script setup>
import BaseButton from "./base-button.vue";
import { computed, defineProps, defineEmits, ref, reactive } from "vue";

const emits = defineEmits(["change-region"]);

const props = defineProps({
  region: {
    type: String,
  },
});

const regionName = ref("Kanto");

const state = reactive({
  elementType: "lightning",
});

const elementTypeAllCaps = computed(() => {
  return state.elementType.toUpperCase() + props.region;
});

const pokedex = await fetch("https://pokeapi.co/api/v2/pokemon?limit=151").then(
  (response) => response.json()
);

const changeRegionName = () => {
  regionName.value = "Hoenn";
  emits("change-region");
};
</script>

<template>
  <h2>{{ regionName }}</h2>
  <BaseButton />
  <h3>{{ elementTypeAllCaps }}</h3>
  <button @click="changeRegionName">Change Region Name</button>
  <pre>{{ pokedex }}</pre>
</template>
```

1. No return statement
2. No setup() declaration

## Composables

We built an app that has a increment counter attached to `base-counter.vue`, but what if we want the same increment functionality to be part of `user-card.vue` functionality. 

![Untitled](Vue%20js%20(Fundamentals)%20b1ecba415912402cb8102b8038a5eee1/Untitled.png)

We want the 190 (newCount) value of two different components (User: new title divides two components - above & below) to be incremented simultaneously.

How do we accomplish it? We use a pattern called composables. 

```jsx
import { ref } from 'vue'
export const newCount = ref(100) // both components referencing this variable "newCount" get updated.
```

```html
<script setup>
import BaseCounter from '@/components/base-counter.vue'
import UserCard from '@/components/user-card.vue'
</script>

<template>
  <BaseCounter />
  <UserCard counterTitle="New title" />
</template>
```

```html
<script>
import { newCount } from '@/composables/countStore'
export default {
  setup() {
    return {
      newCount
    }
  },
  data() {
    return {
      name: 'Anjana'
    }
  },
  methods: {
    changeName() {
      this.name = 'Anjana Vakil'
    }
  },
  props: {
    counterTitle: {
      type: String,
      required: true
    }
  },
  emits: ['change-name']
}
</script>

<template>
  <h1>User: {{ counterTitle }}</h1>
  <h1>New Count : {{ newCount }}</h1>
  <button @click="$emit('change-name')">change name</button>
</template>
```

```html
<script>
import { newCount } from '../composables/countStore'
export default {
  setup() {
    return {
      newCount
    }
  },
  data() {
    return {
      count: 10,
      counterTitle: 'Counter Standard',
      incrementAmount: 8,
      message: 'Hello it works'
    }
  },
  computed: {
    displayTitle() {
      if (this.count > 20) {
        return 'Counter Standard - Very Long'
      } else {
        return 'Counter Standard'
      }
    },
    optimizedIncrementAmount() {
      return this.displayTitle.length * this.incrementAmount
    }
  },
  methods: {
    incrementCount(newAmount, event) {
      console.log(newAmount)
      console.log(event)
      this.count += this.optimizedIncrementAmount
      this.newCount += 10
    }
  }
}
</script>

<template>
  <h1>{{ displayTitle }}</h1>
  <h2>{{ newCount }}</h2>
  <p :data-increment-by="incrementAmount">{{ count }}</p>
  <button @click="incrementCount">Increment Count</button>
  <h1>{{ incrementAmount }}</h1>
  <p>{{ optimizedIncrementAmount }}</p>
  <div>
    <label for="incrementAmount">Increment by:</label>
    <input type="text" v-model="incrementAmount" aria-label="incrementAmt" />
  </div>
</template>

```

# Styling Components

## Global vs. Scoped Styles

If you have 10 components in your app, anywhere you apply the CSS, it gets cascaded globally and applies to all the components.

Vue can scope the css rules to per component if you use as following

```html
<style scoped>
html {
  background-color: papayawhip;
}
</style>
```

1. If someone uses `!important` scope will go away and gets applied globally again.
2. `scoped` simply adds an unique data attribute to HTML elements within the component, so that your CSS rules apply specifically.

```html
<template>
	<button :class="$style.button">Sign up</button>
</template>
<style module>
	.button{
		border: 10px solid green
	}
</style>
```

1. Use `module`
2. Bind the class using `v-bind:class` or `:class`
3. Use `$style.button` to bind `.button`
4. This creates a unique class name specific to component. 

## v-bind in CSS

```html
<script setup>
import BaseCounter from "./components/base-counter.vue";
import UserCard from "./components/user-card.vue";
import { ref } from "vue";

const colorPreference = ref("black");
</script>

<template>
  <div class="wrapper">
    <h2>{{ colorPreference }}</h2>
    <input type="color" v-model="colorPreference" />
    <BaseCounter />
    <UserCard :user="{ name: 'Ben', food: 'Ramen' }" />
  </div>
</template>

<style>
html {
  background-color: papayawhip;
}

.wrapper {
  background-color: v-bind(colorPreference);
}

.button {
  border: 10px solid red !important;
}
</style>w
```

```html
<script setup>
import { useVisitorCount } from "../composables/useVisitorCount";
import { onMounted, ref } from "vue";

let homeVisitors = useVisitorCount();
const colorPref = ref("papaywhip");

onMounted(() => {
  console.log("the component is mounted");
  homeVisitors.incrementGlobalVisitorCount();
});
</script>

<template>
  <main>
    <h1>Welcome to <br />C'est La Vue</h1>
    <p>
      This is a place to manage various things: todos, users, posts, etc.
      Whatever your mind desires!
    </p>
    <hr />
    <i>Visitor count (Home): {{ homeVisitors.globalVisitorCount }}</i>
    <i>Visitor count (Login): {{ homeVisitors.localVisitorCount }}</i>
    <hr />
    <i>{{ colorPref }}</i>
    <input
      type="color"
      name="colorPref"
      id="no"
      aria-label="there something here"
      v-model="colorPref"
    />
  </main>
</template>

<style scoped>
main h1 {
  margin-top: 10vh;
  margin-bottom: 20px;
  background-color: v-bind(colorPref);
}
</style>
```

# Vue Router

[Vue Router](https://router.vuejs.org/)

```jsx
import { createApp } from "vue";
import { createRouter, createWebHashHistory } from "vue-router";
import { routes } from "./routes";
import App from "./App.vue";

const app = createApp(App);
const router = createRouter({
  history: createWebHashHistory(),
  routes,
});

app.use(router);
app.mount("#app");
```

```jsx
import HomePage from "./components/HomePage.vue";
import LoginPage from "./components/LoginPage.vue"; // no needed if you lazyload
import UserPage from "./components/UserPage.vue";

export const routes = [
  { path: "/", component: HomePage },
  { path: "/login", component: () => import("./components/LoginPage.vue") }, // you can also do it this way. It is a optimization (lazy loads)
  { path: "/user", component: UserPage },
];
```

```html
<script>
import HomePage from "./components/HomePage.vue";
import LoginPage from "./components/LoginPage.vue";
import UserPage from "./components/UserPage.vue";

export default {
  components: {
    HomePage,
    LoginPage,
    UserPage,
  },
  data: () => ({
  }),
  computed: {
  },
  methods: {
  },
}
</script>

<template>
  <header class="header">
    <span class="logo">
      <img src="@/assets/vue-heart.png" width="30" alt="logo is asdf" />
      C'est La Vue
    </span>
    <nav class="nav">
      <router-link to="/">Home</router-link> // based on these links
      <router-link to="/login">Login</router-link>
      <router-link to="/users">User</router-link>
    </nav>
  </header>
  <div v-if="renderPage === 'UserPage'">
    <Suspense>
      <router-view /> // Renders the right component
      <template v-slot:fallback> UserList is loading.. </template>
    </Suspense>
  </div>
  <div v-else>
    <component :is="renderPage" :key="renderPage" />
  </div>
</template>

<style>
</style>
```

## Programmatic Routing

```html
<script setup>
import BaseCounter from "./components/base-counter.vue";
import UserCard from "./components/user-card.vue";
import { useCount } from "./composables/countStore";
import { ref, watch } from "vue";
import { useRouter } from "vue-router";

const countStore = useCount();
const router = useRouter(); // invoke 
const colorPreference = ref("white");

watch(countStore.globalCount, (val) => {
  if (val > 2000) {
    console.log(val); // when this value is changed + > 2000
    router.push("/pokedex"); // this updates the route automatically
  }
});
</script>
```

## State management with Pinia

[Pinia 🍍](https://pinia.vuejs.org/)

```jsx
import { defineStore } from "pinia";

export const useCountStore = defineStore("CountStore", {
  // Data
  state: () => ({
    count: 0,
    incrementAmount: 80,
  }),
  // Computed
  getters: {
    doubleCount: (state) => {
      return state.count * 2;
    },
  },
  // Methods
  actions: {
    increment() {
      this.count += this.incrementAmount;
    },
  },
});
```

```jsx
import { createApp } from 'vue'
import { createRouter, createWebHashHistory } from 'vue-router'
import { createPinia } from 'pinia'
import { routes } from './router'
import App from './App.vue'

const app = createApp(App)
const router = createRouter({
   history: createWebHashHistory(),
   routes
})
const pinia = createPinia()

app.use(router)
app.use(pinia)
app.mount('#app')
```

# VueUse

[VueUse](https://vueuse.org/)
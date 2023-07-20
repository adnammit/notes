# Vue

**start with: vueforms**
https://www.codecademy.com/courses/learn-vue-js/lessons/vue-forms/exercises/v-model?action=resume_content_item

## Vue3
* vue3 is a thing now! it's LTS so start using it
* vuetify3 is in beta but seems to be working ok
* try Pinia over Vuex for state management -- simplified interface, no mutations (you just change the data in actions)
* Vite is the preferred optimized compiler for Vue now over vue-cli (which is webpack under the hood)
* [Udemy course on Vue3, Pinia and Firebase](https://www.udemy.com/course/vue-js-3-composition-api/)

### Reactivity
* we now have `refs` and `reactive` objects for two-way binding
* `reactive` is used for objects, `ref` is used for primitive types and arrays (everything but objects)
* not all data needs to reactive and if it doesn't need to be, it shouldn't as non reactive data is more performant
	- example of things that don't need to be reactive (just `data`) are things that are static once the component loads -- values that are set and then don't change

### Lifecycle Hooks
* mounted: what to do as the component is being created
	- onBeforeMount, onMount, onBeforeUnmount, onUnmount
* activate: fired if component is being kept alive (in background)
* update


### Custom Directives



### Composables



### Modules




### Vue3 Setup Cheatsheet
```html
<script setup lang="ts">
import { computed, defineComponent, ref } from 'vue'
import { useMainStore } from '@/store'
import { useDisplay } from 'vuetify'

const itemProps = defineProps({
	value: {
		type: Boolean,
		required: true
	},
	titleText: {
		type: String,
		default: 'Alert!'
	},
})

const store = useMainStore()
const { name } = useDisplay()

// reactive data:
const dialog = ref(false)
const formData = reactive({
	name: '',
	email: '',
	isDisabled: false,
})

// non-reactive data:
const posterUrl = `${import.meta.env.VITE_POSTER_BASE_PATH}${itemProps.poster}`
const maxCharLimit = 50

const quickText = () => {
	let words = itemProps.summary?.split(' ') ?? []
	if (words?.length > maxPreviewWordLength) {
		words = words.slice(0, maxPreviewWordLength)
		words.push('(...)')
	}
	return words.join(' ')
}

// computed:
const isSmallScreen = computed(() => {
	return name.value == 'xs'
})

</script>
```

## Creating A Vue App
* Vue is a js library that makes building complex, responsive websites simpler, faster and easier
* a Vue app essentially needs two things to work:
	- a place to store the data we'll be working with (the `data` property of the **options object**)
	- a syntax for displaying the information (templating)
* there are several ways to source Vue:
	- import it from CDN directly into your html file. Any other scripts that use Vue should be loaded after the Vue script so it's there when it tries to use it.
		* note that `defer` is used to prevent the scripts from loading until the html document is fully loaded (to prevent the page from loading too slowly)
		```html
			<!-- index.html -->
			<head>
				<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js" defer></script>
				<script src="./js/app.js" defer></script>
			</head>
			<body>
				<div id="app">
					<!-- sweet content -->
				</div>
			</body>
		```
	- install using `npm` or `bower`
	- using `vite` is now preferred to vue-cli (which uses webpack under the hood) as it is more performant
* once it's imported, you can use Vue to create a new app by exporting a class called `Vue`
	- create new instances of the class using the `new` keyword
	- each instance is a fully-functional Vue app
* the Vue constructor does not take separate arguments -- rather it takes them all as a single object called the **options object**
	- the options object contains a set of key-value pairs
	- options include **el** which specifies which element has access to Vue's functionality. in this example, an element with id == 'app' is selected. it is a good idea to always use an element id for this selector
	- the app also needs a set of **data** to work with. in most cases, this data would come from a database
	```javascript
		// app.js
		const app = new Vue({ // here is the `options object`:
			el: '#app',
			data: { username: 'micha', email: 'm32@mail.com' },
			...
		});
	```


## Templates
* the values of the data stored in our Vue app will potentially change, so we need a way of dynamically referencing that data
* use of templating is done inside of html. here `username` is subbed out for the username value in the app data
	```html
		<div id="app">
		  <h2>Hello, {{ username }}</h2>
		</div>
	```

## Directives
* [directives](https://012.vuejs.org/guide/directives.html) are special tokens in the markup that tells the Vue library to do something in a DOM element. [view the full list of directives here](https://vuejs.org/v2/api/#Directives)
* the `v-on` directive can be used to handle events. `<div v-on="click : clickHandler"></div>` will cause the ViewModel's `clickHandler` method to be called on click.
* the `v-if` and `v-else` directives are used to handle conditionals
* the `v-for` directive can be used to iterate through a collection of items and dynamically update the list as new items are added:
	```html
		<ul>
		  <li v-for="todo in todoList">{{ todo }}</li>
		</ul>
	```
* the `v-model` directive is used to any form field to connect to Vue data so that when the field is updated, the Vue data is updated automatically
	```html
		<input v-model="username" />
	```
* multiple clauses can be used like so:
	```html
		<div v-on="
		  click   : onClick,
		  keyup   : onKeyup,
		  keydown : onKeydown
		">
		</div>
	```

## Global Components
* it's very common to reuse components throughout a front-end web application
* when you create a component using Vue, you provide a template that should be rendered whenever the component is used in html. you also specify which pieces of dynamic information (**props**) the component uses to fill in the template
* **props** look like normal html attributes
* components look something like
	```javascript
		// in Tweet.js
		const Tweet = Vue.component('tweet', {
			props: ['author', 'message'],
			template: '<div class="tweet"><h3>{{ author }}</h3><p>{{ message }}</p></div>'
		});
		// in index.html:
		<div class="tweets">
		  <tweet v-for="tweet in tweets"
				 v-bind:message="tweet">
				 v-bind:author="username"></tweet>
		</div>
	```

## Single File Components
* single file components accomplish the same thing as the above, have a different structure and have a few advantages.
* [Single File Components](https://vuejs.org/v2/guide/single-file-components.html) are components that are defined individually in files ending with a `.vue` extension as opposed to a `.js` file using the `new Vue({ el: '#container' })` syntax.
* SFCs are made possible by using a build tool like Browserify or Webpack
* advantages:
	- names do not need to be unique
	- syntax highlighting: string templates are ugly -- `.vue` templates are easier to read
	- global components modularize html and js, but css is not. with SFCs, css can be **scoped**
	- commonJS modules: global components restrict us to using HTML and ES5. with SFCs we can use preprocessors like Babel


## Virtual DOM
* DOM review:
	- the Document Object Model is a tree structure that is generated by the browser which represents the various elements in the html document
	- text is represented by its own child nodes
	- if a node is removed, all its children are removed as well
* when a change is made to the DOM, the tree must be updated to reflect that change
	- usually the whole tree is re-rendered because that is faster than modifying and rearranging the DOM
	- _UNLESS_ you use a virtual DOM~
* a **virtual DOM** is a lightweight js copy of the DOM.
	- the framework makes a copy of the js DOM and when a change is made, it diffs the two and informs the browser of the change that needs to be made so that only the affected element is re-rendered
	- this means that:
		1) unnecessary re-renders are prevented
		2) only the necessary elements are re-rendered
		3) renders are grouped together so that one small change does not trigger multiple renders

## Computed Data
* the options object also contains **computed** data: that is, data that is not directly matched to a value. for example:
	```javascript
		const app = new Vue({
			el: '#app',
			data: {
				hoursStudied: 274
			},
			computed: {
				languageLevel: function() {
					if (this.hoursStudied < 100) {
						return 'Beginner';
					} else if (this.hoursStudied < 1000) {
						return 'Intermediate';
					} else {
						return 'Expert';
					}
				}
			}
		});
	```
* the data flow goes both ways with computed data -- that is, the `data` values that are related to computed values can be set dependent on the computation. for example:
	```javascript
		const app = new Vue({
			el: '#app',
			data: {
				hoursStudied: 274
			},
			computed: {
				languageLevel: {
					get: function() {
						if (this.hoursStudied < 100) {
							return 'Beginner';
						} else if (this.hoursStudied < 1000) {
							return 'Intermediate';
						} else {
							return 'Expert';
						}
					},
					set: function(newLanguageLevel) {
						if (newLanguageLevel === 'Beginner') {
							this.hoursStudied = 0;
						} else if (newLanguageLevel === Intermediate) {
							this.hoursStudied = 100;
						} else if (newLanguageLevel === 'Expert') {
							this.hoursStudied = 1000;
						}
					}
				}
			}
		});
	```
	```html
		<!-- index.html: -->
		<div id=“app”>
			<p>You have studied for {{ hoursStudied }} hours. You have {{ languageLevel }}-level mastery.</p>
			<span>Change Level:</span>
			<select v-model="languageLevel">
				<option>Beginner</option>
				<option>Intermediate</option>
				<option>Expert</option>
			</select>
		</div>
	```


## Watchers
* so far we know that `data` is for storing dynamic data and `computed` is used to get and set calculated values, however there is one caveat: a computed value will only recompute when a dynamic value inside its `getter` function changes, so what do we do if we want to update the app without explicitly using a value in a `computed` function? well that's where `watch` comes in
* there are many situations in which it seems like you could use either `computed` or `watch` -- use `computed` wherever possible as it updates values more efficiently

## Methods
* a Vue app's option object also contains `methods` which allows us to store **instance methods**. these are methods that can be used in many different situations, such as helper functions used in other methods or event handlers
* methods is an object of key-value pairs where the keys are the names of the methods and the pair is the function
	```javascript
		const app = new Vue({
			el: "#app",
			data: {
				hoursStudied: 300
			}
			methods: {
				resetProgress: function () {
					this.hoursStudied = 0;
				}
			}
		});
	```
	```html
		<button v-on:click="resetProgress">Reset Progress</button>
	```


## Forms

### Radio Buttons
* radio buttons are weird in that only one can be selected at a time, yet they all correspond to a single value in the app's data. the `v-model` directive for each radio button will refer to the same value
* in this example we have three different radio buttons that map to three different values, but all map to the same property:
	```html
		<legend>How was your experience?</legend>

		<input type="radio" id="goodReview" value="good" v-model="experienceReview" />
		<label for="goodReview">Good</label>

		<input type="radio" id="neutralReview" value="neutral" v-model="experienceReview" />
		<label for="neutralReview">Neutral</label>

		<input type="radio" id="badReview" value="bad" v-model="experienceReview" />
		<label for="badReview">Bad</label>
	```
	```javascript
		const app = new Vue({
			el: '#app',
			data: { experienceReview: '' }
		});
	```



## Stores And State Management
* UPDATE: rather than Vuex, we use Pinia now 🍍 it's cleaner/simpler to use than Vuex but the same principles apply
* we can also use composables for state management
	- using composables is simple and you don't have to install any packages
	- however using Vuex or Pinia is more optimized and provides devTool support in chrome

### Pinia 🍍
* pretty similar to Vuex with some distinguishing differences:
	- Pinia is simpler
	- mutations have been completely removed -- state can be directly updated with actions
	- actions don't need context
	- you can subscribe to actions to observe their outcomes
	- getters that reference other getters
	- optimized for composition API
* there are three components of a pinia store:
	- **state**: the data held in the store
	- **actions**: actions taken on the state which may mutate the data
	- **getters**: sort of like computed properties for the state
		* you don't need to use getters for state properties -- you can just reference them directly, getters are just for convenience
		* the value of getters are cached and only updated when their dependencies are updates
* see [this article](https://pinia.vuejs.org/cookbook/hot-module-replacement.html) for info about how to enable store hot loading

### Vuex
* the **store** provides an interface for storing, mutating data, and managing complex computed values with getters
* at the heart of the store concept is **state**
	- the state tree is a single object that contains all the state information
	- there is one state object per store
	- state data can be used throughout the app
	- this is used for vue's change detection
* vue state consists of **mutations**, **actions**, and **getters**
* **actions** and **mutations** work hand-in-hand to modify state data
* once the store is created, changes to the data are made via **mutations**
	- all modifications to state data must be done through mutations
	- mutations are **synchronous** -- this keeps the state up-to-date, which helps with change detection
* **actions** are used to make **asynchronous** modifications to the data (or other async operations)
	- actions use async `await` and `promises` to commit mutations to the store
* **getters** are used to retrieve complex, computed data from the store
	- this allows you to put filtering/computing logic in once place, rather than in all the places that use the data
* the state of large applications can be broken into individual modules to be a bit more wieldy but tread carefully with namespacing

#### Mutations
* mutations take two args: the state, and optionally any other args you want to pass
	```typescript
		state: {
			cart: [],
		},
		mutations: {
			addToCart(state, items) {
				state.cart.push(items);
			},
		},
	```

#### Actions
* the first param to an action function is a **context** item. context exposes several items for working with the state
	- it provides state, getters, commit, and dispatch functions
	- you can pass in the context or deconstruct it to immediately have what you need
	```typescript
		// whole context item:
		actions: {
			getParts(context) {
			},
		},
		// versus:
		actions: {
			getParts({ state, commit }) {
			},
		},
	```

#### Vuex Modules
* you might have code for different components in your app that have their own getters, actions and mutations, so if you call a getter, how does vuex know what to call?
	- it doesn't -- it will call any or all getters/actions/mutation that have that name
	- not great if you accidentally have two things that have the same name
* solution: use namespacing
	```javascript
		// in component:
		this.$store.dispatch('users/addToCart', items);
		// in users/index.js:
		export default {
			namespaced: true,
			actions: {
				addToCart({ state, commit }, items){
					// do stuff
				},
			},
		}
	```


## Vuetify

### Helper Classes
* vuetify has some shorthand to help you control the layout of elements in-place without using inline css


## Grid System
* any grid system you create will usually be placed inside a container, using `<v-container>` tags
	- the container will automatically center itself on the page and you may use the `fluid` prop to fully extend its width.
* Vuetify’s grid system is made up of a 12 point system, meaning that each row in your grid is split equally into a *maximum* of 12 columns
	- so what happens if you have more than 12 column's worth of stuff?
	- weird shit. use `<v-layout row wrap>` to force content that is wider than 12 rows of content onto a second row.
* **rows** are delineated with `<v-layout>` which contain a `row` prop
* **v-flex** units are "boxes" that go inside of rows -- they can occupy at least 1 column in the row and may span more columns depending on what you specify using media size (breakpoints)[https://vuetifyjs.com/en/framework/breakpoints]
* use the **d-{type}** directive to specify display (`d-flex`, `d-block`, etc)


## Breakpoints
* breakpoints are an integral part of the `flex-box` system
* when you specify a break point (say, `xs6`) what that is saying is that "for screens of size extra-small and larger, this element will span 6 columns"


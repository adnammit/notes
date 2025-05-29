# React

## Overview
* React is a JS library for creating UIs
* SPA (single page applications) are the most common use case for React -- the url changes which maps to different components/views on the same page (no reloading)
* unlike Angular, data flows only one way in React: from parent to child. this prevents side-effects and is more predictable
* React is declarative: you describe what you want to happen and React takes care of the rest. think of the UI as a function of the state over time -- as the state changes, the UI changes -- you don't have to manage the UI, React will change it for you as the state changes
* React is unopinionated about how you style your components - it provides the structure (the DOM), but you still get to style it

## Getting Started
* [official doc](https://react.dev/)
* [typescript with react](https://react.dev/learn/typescript)
* recommended setup ([more](https://react.dev/learn/editor-setup))
	* vscode extensions: prettier, eslint
	* node v18+
	* [chrome devtools extension](https://chromewebstore.google.com/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi?hl=en)
* [managing state](https://react.dev/learn/managing-state)
* [react-router](https://v5.reactrouter.com/web/guides/quick-start)

### Quick Start
I should probably create a boilerplate for this, eh?

Create a React typescript app with Vite build tool, react router and prettier:
```bash
	npm create vite@latest my-cool-app -- --template react-ts
	cd my-cool-app
	npm i
	npm i react-router-dom
	npm i -D @types/react-router-dom
	npm i -D prettier eslint-config-prettier
```
Add `prettier` to your `.eslintrc.cjs` "extends" array
```js
	module.exports = {
		extends: [
			...
			"prettier",
		],
	};
```
Add a prettier.config.js file 
```js
	export default {
		singleQuote: true,
	};
```
Add `.vscode/settings.json`
```json
	{
		"editor.formatOnSave": true,
	}
```


## Thinking in React
Steps in the process for designing a React app (steps 3 and 4 are essentially **state management**):
1. break the desired UI into **components** with an established component tree. think about reusability
2. build a **static** version in React without state
3. think about **state**:
	* when to use state
	* types of state (global vs local)
	* where to place each piece of state
4. establish a **data flow** - identify:
	* one-way data flow
	* child-to-parent communication
	* accessing global state


## Best Practices, Tips and Tricks
* `StrictMode`: you can and should wrap your `App` component in `StrictMode` to help catch common bugs, look for outdated things, etc. strict mode calls your render logic twice to check for badly handled side effects
* for styling your app, use one of these (in order of "weight"): css modules, tailwind, UI library (like material)
* use a Vite build unless you have a specific use case to warrant a full framework like Next.js or Gatsby (see [docs](https://vitejs.dev/guide/#scaffolding-your-first-vite-project))
* use **functional components**, not **class components** (classes should be defined as functions, not classes)
* use **map** to render an array of components
* **react fragments** (`<></>`) solve the single root element problem without adding elements to the DOM
* when setting state, use a callback function rather than referencing the current state value:
	```js
		// instead of
		setCount(count + 1);
		// do this
		setCount(c => c + 1);
	```	
* only use state when data is changing that is reflected in the UI -- changing state causes a re-render, so using state for everything is needlessly expensive
* use **controlled elements** to track your form inputs in React state
* if you need a **side effect** (like loading data), you can do it in one of two places: in an **event handler** or in a **lifecycle event** using `useEffect`
* router
	* you can use **declarative routing** with react-router -- that is, use the provided components like `Router`, `Route`, `Link`, etc to define your routes
	* or you can use **imperative routing** if you need to load or submit data linked to navigation (look at `createBrowserRouter`)
	* preference to use `NavLink` over `Link` as the former indicates tacks on an `active` attribute
	* use `react-router` and loaders to "render-as-you-fetch"

## Implementation Options
* in the simplest "pure" vanilla js implementation, you can import react scripts into an html file ([template](https://gist.githubusercontent.com/gaearon/0275b1e1518599bbeafcde4722e79ed1/raw/db72dcbf3384ee1708c4a07d3be79860db04bff0/example.html))
* the easiest way to get started creating a new app is to use [Create React App](https://create-react-app.dev/) -- configures eslint for you and is a good easy way to play around, but uses webpack/is slow and not advised for production
* there are several production-grade React frameworks that all have [official cli templates](https://react.dev/learn/start-a-new-react-project). these have eslint, prettier, jest etc already configured:
	* [Next.js](https://nextjs.org/): provides routing
	* [Gatsby](https://www.gatsbyjs.com/)
	* [Remix](https://remix.run/)
* if you need a simpler yet performant solution, use [Vite](https://vitejs.dev/) which is far less complicated than any of the React-based frameworks. however it requires manual configuration of eslint, testing, etc
* [add react to an existing project](https://react.dev/learn/add-react-to-an-existing-project)

## Key Concepts
* React is a JS library (not a framework) for creating UIs. there are several full-fledged frameworks based on React such as Next.js, Gatsby, and Remix
* **components**: like many other js frameworks and libraries, react uses reusable components as building blocks
* **JSX**: JavaScript XML: the react templating language which allows you to write html-like code in your js files which is transpiled to js
* **virtual dom**: a lightweight JS representation of the actual DOM used to determine which components in the actual DOM should be updated, greatly reducing DOM updates
* **props**: the data which is passed to the child component from the parent
* **lifecycle methods**: methods that are called at different points in the lifecycle of a component. class-based components use lifecycle methods, but functional components can achieve similar functionality using React Hooks like `useEffect`
* **hooks**: a feature added in React 16.8 that allows you to use state and other React features previously only available to class components in functional components. these include `useState`, `useEffect`, `useContext`, and `useReducer`
* **router**: in SPAs, the page is never reloaded -- instead we rely on urls mapped to different views (routes). when a user clicks on something, JS is used to update the DOM and modify the view appearance. react-router is a popular library for managing client-side routing
* **state**: the internal data store of a component. hooks, Context API and Redux are used to manage state
* **side effects** are any interactions between a React component and the world outside the component (data fetching, setting up subscriptions, setting up timers, manually accessing the DOM, etc). render logic should *not* contain side effects


## React Virtual DOM
* The React DOM is a lightweight representation of the real thing
* The real DOM is updated as little as possible, as rearranging it comes at a high performance cost
* using ReactDOM we add rendered components to our page
* React keeps tabs on the previous virtual DOM, so when changes are made, it ‘diffs’ the two DOMs and changes only what is necessary
* ReactDOM.render takes two args:
	* the component to render
	* the DOM node where you want to render the component
* you usually only have to call ReactDOM.render once because of the parent child relationship of the components: call 'render' on the root (most parent) element and all descendents will be rendered as well
* these components get added to a virtual DOM, or a lightweight JS representation of the actual DOM
* this gets you the accessibility of templates with the power of js
* this also allows React to track differences between the current virtual DOM state and the previous virtual DOM state so that only the necessary elements of the actual DOM are altered (changing the actual DOM is a comparatively slow process).
* if you want your whole app to be React, you would render the parent component to document.body


## Data Overview: Props, State, Context, and Hooks
* it's heckin' confusing trying to keep all the ways of managing data in React straight, so here's a rundown
* **props** are data that is sent one-way from parent to the child component. props are immutable.
* **state** is the internal data of a component that is updated by the component's logic. state is mutable. state is managed using hooks, Context API or Redux
* a parent component can pass its own **state** as a **prop** to a child component
	* when the parent's state changes, the parent will re-render
	* this will also trigger the child component to re-render with the new prop value
	* when a child component is aware of state via props provided from a parent's state, it is called **lifting state up**


## Side Effects
* **side effects** are any interactions between a React component and the world outside the component (data fetching, setting up subscriptions, setting up timers, manually accessing the DOM, etc). render logic should *not* contain side effects, so we can use the `useEffect` hook to manage side effects
* there are two places where we can use side effects:
	* **event handlers**: the user clicks on something -> fetch data; a form is submitted -> save data, etc
	* **on lifecycle events**: when the component mounts, re-renders or unmounts; done by using the `useEffect` hook
* when we use `useEffect` what we're really concerned with is **synchronization** -- keeping the component synchronized with some external source of data
* it is preferable to handle your effects inside event handlers wherever possible


# Components, Props and JSX

## Components
* components are the building blocks of a React app
* a component contains its own data, logic and appearance
* components can be nested within other components
* components should be defined as a function, not a class

### Component Lifecycle
There are several different phases in the lifecycle of a compenent that a developer can "hook" into
* **mount**
	* component instance is created and rendered for the first time
	* fresh state and props are created
* **re-render**
	* state changes
	* props change
	* parent re-renders
	* context changes
* **unmount**
	* component instance is destroyed and removed
	* state and props are destroyed


## Props
* [docs](https://react.dev/learn/passing-props-to-a-component)
* **props** are a way of passing data between a parent component and its children
* anything can be a prop: single values, arrays, objects, functions, and other components
* data passes one-way and is *immutable* -- the child cannot change props
	* if you find yourself needing to change the prop values, what you really should be using is **state**
	* recall that external **props** are different from **state** which is data managed internally by the component itself
* props may come from the state of the parent component -- so when the state changes, the new value is passed down as a prop and the child component is re-rendered
* you can provide **default prop values**
	```js
		const Text = ({fontSize = "20px"}) => {
			return <p style={{fontsize: fontSize}}>Some text I guess</p>
		}
	```
* with typescript, you will need to provide an interface for your props:
	```js
		interface Props {
			title: string,
			color?: string,
		}
		const Text = ({title, color = "black"}: Props) => {
			return <p style={{color: color}}>{text}</p>
		}
	```

## JSX
* JSX is a syntax extension that allows us to write JS in html-like script -- a declarative syntax to describe what components look like and how they work
* what's a **declarative syntax**? refresher on imperative vs declarative:
	* **Imperative**: in JS, you describe a process, with individual steps and logic (modifying variables and calling functions) that describe how you’re going to get to your result
	* **Declarative**: with JSX and HTML, you just declare the objects and the browser renders them
	* so, by using JSX, developers don't have to touch or the DOM directly -- they just declare what they want to happen and React takes care of the rest
* why use it?
	* opening and closing tags make trees easier to read than function calls or object literals
	* this html-like syntax makes it easier to define the commonly nested relationships of components
	* we can combine regular html, css, js and react components all together 
	* JSX is NOT html though -- it really is just another way of calling js functions
* a transpiler like Babel transforms JSX into lightweight JS objects. you can see that once we start nesting, the jsx will be much more readable/maintainable
	```js
		// Babel transpiles this:
		<header>
			<h1 style="color: red">
				Hello, world!
			</h1>;
		</header>

		// is transpiled to this:
		React.createElement(
			'header',
			null,
			React.createElement(
				'h1',
				{ style: { color: 'red' } },
				'Hello, world!'
			)
		);
	```
* it is optional -- you could just use plain JS instead. the JSX precompiler will translate your JSX into lightweight JS objects so either way works -- JSX is just easier to read, generally

### Rules for JSX
* an instance of JSX must have **one single root element** -- you can't have multiple root elements
	* if you need more than one element in your JSX, wrap them in the **Fragment** component (`<React.Fragment />` or `<></>` for short)
* within JSX you can drop in JS expressions by wrapping them in `{ }`
	* An expression is anything that returns a value
	* you cannot use statements (like if/else, switch)
	* `{ }` can also contain other pieces of JSX -- JSX can be written anywhere in a component
	* what's up with `{{ }}`? remember that everything inside of brackets is javascript -- so we're passing in an object to the outer curly brackets
* rendering lists: use `map`
	```js
		<ul>
			{pizzas.map((pizza) => (
				<Pizza
				name={pizza.name}
				ingredients={pizza.ingredients}
				price={pizza.price}
				/>
			))}
		</ul>
	```
* listening for events: JSX has it's own special syntax for that too -- use `onClick()` or `onChange()` or `onMouseEnter()` (hover)
	* [docs](https://react.dev/learn/responding-to-events#adding-event-handlers)
	* by convention, name handlers as `handle{EventName}`
	```js
		const handleClick = () => { alert('Hello, world!') }
		<button onClick={handleClick}>
			Click me!
		</button>
	```

# State
* state is the internal data store of a component -- it can be changed as the user interacts with the page, or as new data is received. **state is therefore mutable**
* **updating a component's state causes the component to re-render**
* state can be comprised of **state variables** or **pieces of state** -- individual pieces of data that each represent some different thing we want to keep track of (ex: notification count, search term, and active tab all track something different)
* state allows developers to:
	* update the component view by re-rendering it
	* persist local variables between renders
* state applies to an individual component, but we also want to think about the overall state of the application spread across many components
* **when should we use state?**
	* use state whenever there is something the component should keep track of over time -- data that will change at some point
	* if something in the component is dynamic, create a piece of state related to that thing and update state when that thing should change
	* if you want to change the way a component looks or the data it displays, update the correlating state variable
	* usually you would do this in an **event handler**
	* ex: open/close a modal 
* **when to not use state?**
	* only use state when that data triggers a change in the UI -- remember that changing state causes a re-render, which is costly. 
* **how do we manage state?**
	* developers use a practice known as **prop drilling** to pass state down through the component tree
		* the parent passes state to a child which passes state to its child, etc
		* data may pass through multiple layers of components that don't need the data before reaching the component that does
		* this practice is very manual, requiring a lot of code, and can be difficult to debug
		* similar to **lifting state up** by passing state from a parent to a child as props
	* to make our lives easier, there are several different solutions for managing state within an application that all have their own use cases
		* **react hooks** are the most common way to manage state in a functional component
		* **context API** can provide state to child components without using props or prop drilling
		* **redux** is a state management library that can be used to manage state across the entire application


## React Hooks
* Hooks are a new addition in React 16.8. They let you use state and other React features without writing a class
* things that start with `use` are Hooks: `useState`, `useEffect`, `useReducer`
* Hooks can only be called at the top-level of your component -- they cannot be called inside loops, conditions, or functions (aside from your component function)

### Hooks Cheat Sheet
* **useState**: adds state to a functional component
* **useEffect**: runs after every render, handles side effects -- good for data fetching, synchronization after data changes
* **useReducer**: an alternative to `useState` -- good for managing state objects that contain multiple sub-values


### Setting State with useState
* to set the initial state of a component, use the [`useState` hook](https://react.dev/reference/react/useState#usestate)
to set the initial value and declare a function to update it
* `useState` takes an initial value (`initialState`) as an argument (`useState(0)` sets initialValue to 0)
	* `initialState` can be a function but it will be treated as an initializer function -- it should be pure, take no args, and return a value of any type
* it returns an array with exactly two elements:
	* the current state variable (`count`) - during the first render, this will be the same as the `initialState` value
	* the `set` function that allows you to update the state (`setCount`)
* the return values of `useState` are typically set using array destructuring and are named `[{name}, {setName}]`
* the `set` function can pass the next state value directly, or it can pass a function that calculates it from the previous state
	```js
		function ResetCount() {
			setCount(0);
		}
		function IncrementCount() {
			setCount(c => c + 1);
		}
	```
* you can set the state using the current state value, but it's better practice to use a function
	```js
		// if you called this twice for some reason, the count would only increment by 1 -- the second call would use the original value of count
		setCount(count + 1);
		setCount(count + 1);

		// instead use a function to calculate the next state value - this will increment by 2
		setCount(c => c + 1);
		setCount(c => c + 1);
	```
* it is possible to modify state without using the `set` function -- **do not do this**
* putting that all together:
```js
export default function App() {
	// set count to 0 and declare a function to update it
	const [count, setCount] = useState(0);

	return (
		<div>
			<p>You clicked {count} times</p>
			<button onClick={() => setCount(c => c + 1)}>
				Add 1
			</button>
			<button onClick={() => setCount(0)}>
				Reset Count
			</button>
		</div>
	);
}
```
* some caveats about `useState`:
	* the initial state is only used during the first render -- subsequent renders will ignore the `initialState` argument
	* if the new state value is the same as the previous value (as determined by `Object.is` comparison), react will skip rerendering the component and its children
	* **React batches state updates** -- it updates the screen only after all event handlers have run and called their `set` functions to prevent unnecessary renders. if you need to override this behavior, use `flushSync`
	* in strict mode, initializer functions and set functions will be called twice

### Setting State with useEffect
* use `useEffect` when some data changes and you need to update something else, or when you need to fetch data
	* when a user performs some action, that might cause something else to change
	* when you render a component, you may want to load some async data -- for example, fetch some data from an api to display in your component
* you may want to `fetch` the data in your component function and then store the result in state, but that's a terrible idea -- the component function is the **render logic** and whenever the state is updated, the render logic is called... and then the component is re-rendered, the render logic is called, the state is updated, the component is re-rendered, etc -- you've basically created an infinite loop
* that's where the `useEffect` hook comes in -- `useEffect` is a hook that allows you to perform side effects in your function components that execute after the component renders
* `useEffect` takes two arguments:
	* the first argument is a function that performs the side effect -- this function fetches the data and then sets state
	* the second argument the **dependency list** -- if any of the dependencies change, the side effect will run again
	```js
		export default function App() {
			const [data, setData] = useState(null);

			async function fetchData() {
				const res = await fetch('https://api.example.com/data');
				const data = await res.json();
				setData(data);
			}
			useEffect(() => {
				fetchData();
			}, []); // [] -> empty dependency list, the effect will only run once

			// then we can also update our data with a click or other input event
			function handleClick() {
				fetchData();
			}
		}
	```
#### The useEffect Dependency Array
* when an empty dependency list is provided, the effect will run once after each time the component renders
* so by default (with no dependencies), effects run after every render
* if dependencies are provided, **React will execute the effect only when those dependencies change**
* **every state variable and prop used inside the effect must be included in the dependency array**
* here is an example of an effect behaving like a computed property -- whenever the title or userRating changes, the effect will run
	```js
		const title = props.movie.Title;
		const [userRating, setUserRating ] = useState('');

		useEffect(() => {
			if (!title) return;
			document.title = `${title} ${userRating && `(Rated ${userRating})`}`;
			return () => {
				document.title = 'React App';
			};
		}, [title, userRating]);
	
	```

### Setting State with useReducer




## React Context API
* supported in React 16.3 or greater
* Context provides a way to pass data through the component tree without having to pass props down manually at every level (simplified prop drilling)
* keep context as small as possible -- it's not meant to be global
* start with component, if that's not enough, use context (second-citizen)
	* "local global" states
	* you can have compound components and context makes that nicer -- you can have a context for all the sub-components
	* nested contexts
* Provider sets context value, Consumer interprets that value and renders
```javascript
const { Provider, Consumer } = React.createContext("");
<Provider value="Pat">
	<Consumer>
		{value}
	</Consumer>
</Provider>
```
* learn more from react documentation on context and render props
	* You Might Not Need Redux
	* use Context to pass data that could be considered 'global' through multiple levels of components
		* theme, authenticated user or preferred language could be examples
	* don't do this just to avoid passing props down -- stick to cases where the data is used on multiple levels

## Redux
* Redux is an open source js library for managing global application state
* react contains a mechanism for managing state, but the Redux library is designed specifically to manage and maintain application state alongside other frameworks
* **key concepts**:
	* state is stored in a `store`
	* state is read-only. to modify the store, emit an action
	* `reducers` are pure functions that take the current state and an action and return a new state
* Redux is more verbose and more challenging to master -- requires high complexity which the app must leverage to be worth it
* Redux can run in different environments, is easy to test, and provides good developer tools


# Routing
* client-side routing matches different urls to different UI views
* this keeps the UI in sync with the current browser URL
* React relies on third-party packages for routing, the most popular of which is `react-router`
* for SPA applications, rather than completely reloading/navigating to a new page, the URL changes and javascript updates the view components (DOM)
* when a user clicks a link, `react-router` changes the url, which triggers a DOM update
* installing react-router:
	```bash
		npm i react-router-dom
		npm i types @types/react-router-dom
	```
* there are actually [different kinds of routers you can use](https://reactrouter.com/en/main/routers/picking-a-router) but web projects should use `createBrowserRouter`
* there are two ways to define routes:
	* **static routing**: what you're used to -- define routes in a configuration file to map routes to components. allows you to specify loaders and actions
	* **declarative route definitions**: use components provided by `react-router-dom` to define our routes right in the JSX, such as `BrowserRouter`, `Route`, and `Link`
	* as of React Router v4, declarative/dynamic routing is preferred ([source](https://v5.reactrouter.com/web/guides/philosophy))

## "State" in the URL
* why would you put data in the URL vs in the component state?
	* by putting info in the url, you can bookmark a page and return to it later or share it
	* easy way to share global state **with all components** in the app
	* pass data from one page to the next page
* what would you put in the url? resource identifiers of course, also view-specific things like open/closed panels, currently selected item, sorting order, applied filters, etc
* by putting "stately" information in the URL, you can share the URL with someone else and they will see the same thing you do
* for example, the url `https://example.com/bookings?city=paris&startdate=20240704&enddate=20240708` could show all available bookings for your trip, and you could share this link with others
* url components: given this url: `www.example.com/app/cities/lisbon?lat=38.7223&long=-9.1393`
	* `www.example.com` is the base url
	* `app/cities` is the **path** that maps to a component
	* `lisbon` is a **parameter** that customizes the component view/data
	* `lat` and `long` are **query parameters**/`lat=38.7223&long=-9.1393` is the **query string**

## Route Parameters
* **dynamic segments** are placeholders that are parsed as parameters or query params (`/cities/:cityName`)
* route parameters are dynamic parts of the URL that can be used to identify what is displayed on the page. for example, `/cities/:cityName` would match `/cities/paris` and `/cities/london`
* to access route parameters, use the `useParams` hook
	```js
		// in router: 
		<Route path="/cities/:cityName" element={<City />} />
		// in City component:
		import { useParams } from 'react-router-dom';
		const { cityName } = useParams();
	```

## Query Parameters
* query parameters are used to pass data to a route

	```js
		// in component
		import { useSearchParams } from 'react-router-dom';
		const [searchParams, setSearchParams] = useSearchParams();
		const cityName = searchParams.get('cityName');
	```

# Fetching Data
There are several patterns for fetching data that depend on the use case
* **fetching as you render**: use `react-router` and loaders to load the required data as the component renders
	* use this if the data that needs to be fetched is bound to the route, i.e. if the current route and parameters are enough to form the request for the data (given route `/products?id=123`, you know you need to load data for product 123)
* **fetching data on mount**: use `useEffect` to fetch data when the component mounts
* **fetching data on user interaction**: use an event handler to fetch data when the user interacts with the component




# Building an App

## Planning Your Application

### Structuring Your Project
* one common way of organizing within your `/src` directory. let's think of a menu app
	* **features**: components that represent a concept or business entity
		* one sub-folder for each feature that contains jsx, css, hooks, etc
		* ex: User, Menu, Cart, Order
		* these features have sub-components like MenuItem, OrderLine, etc and data can be shared within each "domain" (i.e. Menu)
	* **pages**: components that represent a page in your app
		* ex: Home, Login/Signup, Menu, Cart, New Order, Order Detail
	* **ui**: presentational shared components
		* ex: Button, Input, Modal, Spinner
	* **services**: functions that interact with the outside world
		* ex: api, auth, storage
	* **utils**: stateless helper functions
		* ex: date, string, number

### State Management
* again for our menu app, we can organize our state needs like so:
	* User: Global remote state (fetched from API)
	* Menu: Global remote state (fetched from API)
	* Cart: Global UI state (just stored in app)
	* Order: Global remote state (fetched from API)
* **Remote State Management**: 
	* use react router to **"render-as-you-fetch"** -- this isn't really state management, but it supports remote state management
	* **ReactQuery** is another option
* **UI State Management**
	* use Hooks or Context API for simple state management cases
	* you may choose to use **Redux** for global state management, but it's not necessary for most apps



## Navigation
* specify your `BrowserRouter`, `Routes` and `Route` components in the `App` component
* create a separate Navigation component to hold the nav links (the clicky bits)
	* use `NavLink` to link to different routes rather than `Link` -- `NavLink` gives you a class of `active` when the link is active so you can style the navbar to show the current page you're on
* import the Navigation component into each page you want to use it
	* **really? seems like there should be a better way**


## Styling
* React is unopinionated about how you style your components
* options include:
	* inline css: written in the jsx file, scope limited to jsx file
	* css or sass: defined in an external file, global scope
	* css modules: defined in an external file, scope limited to component
	* css-in-js: external file or component file, scope limited to component
	* utility-first css: defined in jsx, scope limited to jsx file. example: tailwind
	* or instead of using css, you could use a UI library with prebuilt/prestyled components, such as materialUI, chakraUI, Mantine, etc

### Conditional Styling
* you can conditionally apply styles to a component by using a ternary operator
	```js
		<div style={{ color: isRed ? 'red' : 'blue' }}>
			Hello, world!
		</div>
	```
* you can also use a function to determine the style
	```js
		const getStyle = () => {
			if (isRed) {
				return { color: 'red' };
			} else {
				return { color: 'blue' };
			}
		};
		<div style={getStyle()}>
			Hello, world!
		</div>
	```



## Images
* where do you put them? 
	* typically you'll have a `public` folder in your project root -- by default, put images there
	* if you need to import an image in your app code, put it in `src/assets`


## Forms
* in a typical form, the DOM elements would control and manage the state of the form
* that makes it really hard for React to know the value of the form elements
* so instead, we use **controlled elements** -- the form elements are controlled by React so the rest of the application knows what the value of the form elements are
* **controlled elements** are simply form elements whose value is mapped to a state variable with an event listener that calls the set function and sets it to the value of the event target (which is our input element)
	```js
		const [name, setName] = useState('');
		<input type="text" value={name} onChange={(e) => setName(e.target.value)} />
	```


# Debugging/Troubleshooting
* is your page not refreshing after making changes? hard reload -- HMR breaks periodically and this will reset it



































********************************************************************************************************************
stuff below is old and has not been vetted/reviewed


## Basics
* React.createClass: create a new component (type?)
* render(method): what we would like our html component to look like
* ReactDOM.render: renders a React component to a DOM node
* getInitialState: the way in which you set the initial state of a component
* getState: a helper method for altering the state of a component
* props: the data which is passed to the child component from the parent
* propTypes: allow you to control the presence or types of certain props passed to the child component
* getDefaultProps: the way in which you set the default props of a component
* component life cycle:
	* componentWillMount - fired before component mounts
	* componentDidMount - fired after component mounts
	* componentWillReceiveProps - fired whenever there is a change to props
	* componentWillUnmount - fired before the component will unmount
* events:
	* onClick
	* onSubmit
	* onChange




## INTRO TO REACT
* remember MVC? No? well it stands for Model View Controller
	* this is a software architecture pattern for implementing UIs
	* it separates software into three distinct parts with three distinct functions
	* this is especially popular with GUIs and web design, such as single-page apps
* React is a JS library for creating UIs -- it is the 'V' in 'MVC’
	* it is not an MVC framework like Angular
	* focus is on creating composable user interfaces which present data that changes over time and reacts to the user and to changes in data in real-time
* Imperative vs declarative:
	* Imperative: in JS, you describe a process, with individual steps and logic (modifying variables and calling functions) that describe how you’re going to get to your result
	* Declarative: with React and HTML, you just declare the objects and the browser renders them
* React is inherently dynamic -- when data changes, React updates the altered components
	* whenever something is changed, the 'render' method is called
	* the current call of 'render' is diffed with the previous call so that only the necessary changes are made to the DOM
	* what does 'render' actually return? a lightweight description of what the DOM should look like
	* This makes it much easier to keep your data model in sync with the UI
* React components are composable, reusable, and encapsulated
	* think of OOP: all the functions you need to get and set data are within that object. Objects can be manipulated by their parents and can manipulate their children
* React allows you to easily manage the many varied states of typical interface components
* React doesn't use templates. React uses components instead, because:
	* JS allows for abstraction
	* view logic unifies markup, making views easier to extend and maintain
* React code is great for team development -- the code is readable and maintainable
* cons:
	* React is not supported by any browser below IE8
	* unless your website has a lot of dynamic elements, the overhead of React won't pay off.
* What files/filestructure do you need to start off with?
	* Html and css files
	* In a subfolder (probably) you would/might include:
		* react.js - our react library file
		* react-dom.js - logic for turning react DOM into the real DOM
		* babel - allows us to convert JSX to JS
		* A reference to our script (\*.jsx)
		* Example set of script tags might look like:
		```html
		<body>
		<div id=”container”>Loading…</div>
		<script src=”./vendor/react.js”></script>
		<script src=”./vendor/react-dom.js”></script>
		<script src=”./vendor/babel-browser.min.js></script>
		<script type=”text/babel” src=”./app.jsx”></script>
		</body>
		```
   - Notice that our code file ends with the jsx extension to signal to babel that it needs to be compiled. This would not normally be done for production (it would be precompiled)
   - The ‘div’ tag serves as a blank container -- we’re going to use react to stuff it full of whatever we want.


## D3
* it's a helper lib
* cache data models on component state in response to user input
* "Data Driven Documents"
	* data is cool! how do we visualize it?
	* bl.ocks.org
* many use it frequently
* d3 is for data manip -- React is for DOM manip
* manipulating data:
	* don't use enter, join, select
	* organize d3 code into component methods that we can call from lifecycle events
	* do the heavy lifting here -- leave the UI to React





## REACT TOOLS:
* The DOM generated by react (viewed in the browser developer tools) can be more difficult to understand than your average DOM. install the React DevTools browser extension to get more info in your dev tools (plus a whole new “React” tab)
* React includes a mechanism for managing state within components, but it doesn’t do everything. Use other libraries for things like AJAX, persistence and state management
* FLUX and Redux are two such state management libraries
* FLUX: The concept "Flux" is simply that your view triggers an event (say, after user types a name in a text field), that event updates a model, then the model triggers an event, and the view responds to that model's event by re-rendering with the latest data. That's it.
* Grunt or Gulp can help
* Browserify and Webpack are more feature-rich build tools that can help us compile our JSX and provide a module system similar to that used in the Node.js platform, merging all JS files into one
* Recommendation: start with just babel and webpack, without using Gulp or Grunt


## BABEL
* install babel independently for each project b/c different projects might require different versions.
* install locally by running:`
```
$ npm install --save-dev babel-core babel-loader
#
```


## REACT COMPONENTS
Describing virtual DOM elements: there are (up to) three parts to each element:
Name: div, span, script, a, etc
Attributes: key value pairs that add more information (like href=”www” or ‘type=’)
Children: for example, you can put more text inside an ‘a’ or ‘div’ tag.
A React element may or may not have children
Example: (name: ‘a’, attribute: link, child: “Treehouse”)
				var myLink= React.createElement(‘a’, {
					href: “https://teamtreehouse.com”
				}, “Treehouse”);
Elements written in JSX look an awful lot like html, but they’re not -- it’s just another way of calling js functions. The above example looks like:
				var myLink = (
					<a href=”https://teamtreehouse.com”>
						Treehouse
					</a>
				);
Doing this means we need to put an extra bit of build code in to translate JSX to JS. This could be something like Babel, which is great because it can make new JS compatible with old browsers
Each component is composed of layout code and behavioural code
Traditionally these two have been separated between html and js, but in react JSX, they become more holistically entwined together
by convention, html uses lowercase variable names, and react component var names start with an upper case letter
react components have a state and props
state is changing -- it always becomes props in the end (the thing is changing and then you hand it off to someone else and it doesn't change anymore
React components don’t know about their parent components - they only know about the props that are passed to them and their child components

CREATING REACT COMPONENTS:
the 'render' method kicks it off -- it returns a tree of React components which will eventually render to html
every component is required to have a render method
ReactDOM.render([virtual DOM element], [real DOM element]) instantiates the root element, starts the framework, and injects the markup into a raw DOM element, provided as the second element:
	ReactDOM.render(<h1>Hello!</h1>, document.getElementById(‘container’));
Remember that these tags are not actual DOM nodes -- they are instantiations of React div components
these are markers that React knows how to handle
divs can be the 'skeletons' for React components
Now we don’t want to make our program by just making a million ReactDOM.render calls, so we wrap this processing up in an application that manages and returns our components.
Note that our component Application is capitalized in order to differentiate it from the other components like h1, div, etc
a component function call can return only one element, so cheat by wrapping your individual elements in a div.
creating your first react component:
var HelloWorld = React.createClass({
render: function(){
		return (
		<div>
			Hello World!
		</div>
		)
	}
});
ReactDOM.render(<HelloWorld />, document.getElementById('app'));
what did we just do?
React.createClass is a constructor
createClass takes in an object (returned by the 'render' function)
the result of calling the constructor is stored in a variable called HelloWorld because later we will need to tell React to which element our component should be rendered.
then we add it to the big picture of our page using ReactDOM
When you define the class of a jsx component, you use the keyword ‘className’ as ‘class’ is a reserved word.
You may start with a monolithic component, but it’s good design to ‘decompose’ it into smaller, reusable pieces


PROPS
When you’re defining a component, you don’t necessarily know what the values for your elements will be.
A stateless component function can take an argument (props) which contains any properties that are passed in in the ‘render’ call, which can be used to create the component
A component class contains ‘props’ as one of its own properties and can be accessed with the ‘this’ keyword
We can set defaults for props, set types, and create defaults for props by defining the prop types below the component function.
This is very useful for error handling as it allows us to control what happens if we don’t get the information we’re expecting
For example, you can set a few of your prop types like so:
Application.propTypes = {
	title: React.PropTypes.string,
	players: React.PropTypes.arrayOf(React.PropTypes.object).isRequired,
};
		Note that ‘players’ is an array of objects, but that’s optional -- it could just be an array of anything.
You can go one step further and specify what “shape” those components should be (that is, what the individual properties of each object should be:
Application.propTypes = {
	title: React.PropTypes.string,
	players: React.PropTypes.arrayOf(React.PropTypes.shape({
		name: React.PropTypes.string.isRequired,
		score: React.PropTypes.number.isRequired,
	})).isRequired,
};
It’s a good idea to set propTypes for each component, but you only need to set defaults as needed.
It doesn’t make a lot of sense to make a property both required and set a default b/c it will always have a value.
Props passed to a stateless component should not be changed by that component -- props are immutable
Once you start breaking an application down to many sub-components, it is very common for props to be passed around



A MORE COMPLEX EXAMPLE:
Note that function Application takes an argument (props) which contains any properties that are passed in in the ‘render’ call (such as ‘title=”My Scoreboard”’)
Note that we are using the keyword ‘className’ (for JSX) rather than the html keyword ‘class’
Note that we can insert JS expressions inside {}


function Application (props) {
  return (
	<div className=”scoreboard”>
	  <div className=”header”>
	  <h1>{props.title}</h1>
	</div>
	</div>
  );
}

Application.propTypes = {
	title: React.PropTypes.string.isRequired,
};

Application.defaultProps = {
	title: "Someone's Scoreboard",
};

ReactDOM.render(<Application title=”My Scoreboard” />, document.getElementById(‘container’));


OBJECTS:
As mentioned, we can use arrays of objects as our data
React demands that you use unique ‘key’ props for your objects which makes it possible to track and modify elements when the DOM is updated.
We can’t use JS loops in jsx, but we can use the JS map function to create a component for each object in an array: 	    <div className="players">
		{props.players.map(function(player) {
			return <Player name={player.name} score={player.score} key={player.id}/>
		})}
	</div>

STATE AND COMPONENT CLASSES
While props are immutable and owned by the parent, each component has a state which is mutable and private
It’s easy to throw together SFCs (Stateless Functional Components) but next we need to add state to make our components responsive and dynamic using Component Classes
Making a Component Class:
A component class may use several methods, but the only one that is required is ‘render’
Defining a component class is very similar to defining an SFC except you’re defining a variable which is the result of the function call React.createClass. You’re also defining the propTypes within the component class declaration:
var Counter = React.createClass({
  propTypes: {
	score: React.PropTypes.number.isRequired,
  },
  render: function() {
	return (
	  <div className="counter">
	<button className="counter-action decrement"> - </button>
	<div className="counter-score">
	  {this.props.score}
	</div>
	<button className="counter-action increment"> + </button>
		  </div>
	);
  }
});
Being able to translate an SFC to a Component Class is very useful and common
Generally speaking, start with SFCs and convert them to Component Classes when state is required.
In the Scoreboard Treehouse example, we changed the score counter because score changes.
So  now that you have a stateful component, how do you get it to do stuff?
Essentially replace ‘this.props’ with ‘this.state’, and then make modifications to it.
getInitialState is the way to set the state of a component, which can then be modified by calling this.setState()
calling setState() causes the virtual DOM to re-render the component, the diff algorithm runs, and the real DOM is updated with any necessary changes
You can define various methods within the component class which act as listeners for events, and then call setState().
Note that while incrementScore takes the event as an argument, you very well may not do anything with it. It might be a good idea to do prevent default
Also you could change the score by simply saying this.state.score+=1; and this would in fact change the score, however the component will not re-render itself unless we call setState

 	incrementScore: function (e) {
	  this.setState({
		score: (this.state.score + 1),
	  })
	},

	render: function() {
	  return (
		<div className="counter">
		  <button className="counter-action decrement"> - </button>
		  <div className="counter-score">
  		  {this.state.score}
		  </div>
		  <button className="counter-action increment" onClick={this.incrementScore}>
  		+
	  </button>
		 </div>
	  );
	}
There are two different kinds of state to consider when building react applications:
Application state: in our scoreboard example, this includes the list of players and their scores
Application state should be managed as high up as possible due to the natural top-to-bottom flow of react virtual DOM elements
Component state: internal, isolated state within a single component that is only necessary for it to do it’s job -- the rest of the app doesn’t need to know about it
Recall that children don’t know anything about their parents -- only the props that are passed to them, and their children. This results in Unidirectional Data Flow - data comes from one place: the top. When a change occurs, the data flows down through the component tree
This makes it difficult for changes to happen at a lower level: a child component can’t change data that is managed by a parent component, so callback functions are used

HTML ELEMENTS AND REACT:
Tables:
always include a tbody and thead b/c otherwise one will be inserted for you, which will create problems
Forms pretty much always need to be stateful b/c obvs

MOUNTING COMPONENTS
If you mount a component and use something like ‘interval’, make sure to unmount it to prevent memory leaks


REACT BACK END
React doesn’t care what’s happening out back -- you could be using Ruby, Python, Go, PHP, etc, but if you want to take advantage of server-side rendering, Node.js will be the way to go, as it also speaks/is JS.
Isomorphic app: code which can be run both on the client and server



CHANGES AND EVENTS
o setState: alters a component's data, causes 'render' to be called and
  the DOM is updated
	* called from a function within a component
o handleChange:
	* defined within the component
	* gets called when onChange is called on the component
	* handleChange calls setState to refresh the DOM with the new data
o onChange:
	* an attribute which will call the specified method every time the
	  value of it's associated element changes
	* for example, you may want to call the element's handleChange method

UNIVERSAL RENDERING
o it is cool -- makes your single page app possible without (or with
  fewer) server requests
o check out
	* webpack isomorphic tools
	* redux async connect
	* react-redux-universal-hot-example





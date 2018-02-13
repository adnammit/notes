## KEY CONCEPTS
* JSX: allows us to write JS in html-like script which gets transformed
  into lightweight JS objects
* VIRTUAL DOM: a lightweight JS representation of the actual DOM
* React.createClass: create a new component (type?)
* render(method): what we would like our html component to look like
* ReactDOM.render: renders a React component to a DOM node
* state: the internal data store (object) of a component
* getInitialState: the way in which you set the initial state of a
  component
* getState: a helper method for altering the state of a component
* props: the data which is passed to the child component from the parent
* propTypes: allow you to control the presence or types of certain props
  passed to the child component
* getDefaultProps: the way in which you set the default props of a
  component
* component life cycle:
    - componentWillMount - fired before component mounts
    - componentDidMount - fired after component mounts
    - componentWillReceiveProps - fired whenever there is a change to
      props
    - componentWillUnmount - fired before the component will unmount
* events:
    - onClick
    - onSubmit
    - onChange


## INTRO TO REACT
* remember MVC? No? well it stands for Model View Controller
    - this is a software architecture pattern for implementing UIs
    - it separates software into three distinct parts with three distinct functions
    - this is especially popular with GUIs and web design, such as single-page apps
* React is a JS library for creating UIs -- it is the 'V' in 'MVC’
    - it is not an MVC framework like Angular
    - focus is on creating composable user interfaces which present data that changes over time and reacts to the user and to changes in data in real-time
* Imperative vs declarative:
    - Imperative: in JS, you describe a process, with individual steps and logic (modifying variables and calling functions) that describe how you’re going to get to your result
    - Declarative: with React and HTML, you just declare the objects and the browser renders them
* React is inherently dynamic -- when data changes, React updates the altered components
    - whenever something is changed, the 'render' method is called
    - the current call of 'render' is diffed with the previous call so that only the necessary changes are made to the DOM
    - what does 'render' actually return? a lightweight description of what the DOM should look like
    - This makes it much easier to keep your data model in sync with the UI
* React components are composable, reusable, and encapsulated
    - think of OOP: all the functions you need to get and set data are within that object. Objects can be manipulated by their parents and can manipulate their children
* React allows you to easily manage the many varied states of typical interface components
* React doesn't use templates. React uses components instead, because:
    - JS allows for abstraction
    - view logic unifies markup, making views easier to extend and maintain
* React code is great for team development -- the code is readable and maintainable
* cons:
    - React is not supported by any browser below IE8
    - unless your website has a lot of dynamic elements, the overhead of React won't pay off.
* What files/filestructure do you need to start off with?
    - Html and css files
    - In a subfolder (probably) you would/might include:
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

## REACT VIRTUAL DOM
* The React DOM is a lightweight representation of the real thing
* The real DOM is updated as little as possible, as rearranging it comes at a high performance cost
* using ReactDOM we add rendered components to our page
* React keeps tabs on the previous virtual DOM, so when changes are made, it ‘diffs’ the two DOMs and changes only what is necessary
* ReactDOM.render takes two args:
    - the component to render
    - the DOM node where you want to render the component
* you usually only have to call ReactDOM.render once because of the parent child relationship of the components: call 'render' on the root (most parent) element and all descendents will be rendered as well
* these components get added to a virtual DOM, or a lightweight JS representation of the actual DOM
* this gets you the accessibility of templates with the power of js
* this also allows React to track differences between the current virtual DOM state and the previous virtual DOM state so that only the necessary elements of the actual DOM are altered (changing the actual DOM is a comparatively slow process).
* if you want your whole app to be React, you would render the parent component to document.body

## JSX
an optional syntax extension -- you could just use plain JS instead
looks similar to XML or HTML
allows us to write JS in html-like script which gets transformed into lightweight JS objects
the JSX precompiler will translate your JSX into lightweight JS objects so either way works -- JSX is just easier to read, generally
Understand that JSX is NOT html though -- it really is just another way of calling js functions
advantage to using JSX:
it is a concise syntax for designing tree structures with attributes
opening and closing tags make trees easier to read than function calls or object literals
it doesn't alter the semantics of JS
within JSX you can drop in JS expressions by wrapping them in {}
An expression is anything that returns a value, as opposed to statements (like if/else statements)
this allows you to drop react components into the tree
```html
<div className="comment">
 <h2 className="commentAuthor">
{this.props.author}
 </h2>
{this.props.children}
</div>
```

## REACT TOOLS:
* The DOM generated by react (viewed in the browser developer tools) can be more difficult to understand than your average DOM. install the React DevTools browser extension to get more info in your dev tools (plus a whole new “React” tab)
* React includes a mechanism for managing state within components, but it doesn’t do everything. Use other libraries for things like AJAX, persistence and state management
* FLUX and Redux are two such state management libraries
* FLUX: The concept "Flux" is simply that your view triggers an event (say, after user types a name in a text field), that event updates a model, then the model triggers an event, and the view responds to that model's event by re-rendering with the latest data. That's it.
* Grunt or Gulp can help
* Browserify and Webpack are more feature-rich build tools that can help us compile our JSX and provide a module system similar to that used in the Node.js platform, merging all JS files into one
* Recommendation: start with just babel and webpack, without using Gulp or Grunt


## REDUX
* Like we mentioned, react contains a mechanism for managing state, but the Redux library is designed specifically to manage and maintain application state alongside other frameworks

## WEBPACK
* Used to load modules, minify code and produce a bundle that’s ready for production
* Common build steps include:
    - Module loading: look for ‘require’ and ‘import’ statements in the source code to gather those dependencies automatically
    - Concatenation: combine multiple files into one for more efficient loading
    - Minification: removal of whitespace and comments to shrink the size of the code
* installation and setup:
    - install `webpack-dev-server` globally
    - install `webpack-dev-server webpack babel-core babel-loader babel-preset-env babel-polyfill` locally.
    - put all your source files in a source dir (like `src`), set up your config file (below) and then run this from inside the project directory to kick it off:
        `webpack-dev-server --content-base src src/main.js`
* webpack.config.js:
    - There are many ways to configure webpack, but here are some key features:
        * Entry: the array of files to be run at startup. Dependencies will be built from there
            - self-contained scripts go here
            - ex: you might want to run `babel-polyfill` or something and then your `main.js`
        * Output: defines where the application will go once build is performed, when it’s time to deploy. Output is specified by ‘path’ (a directory) and ‘filename’ (the name of the bundled file)
            - the browser gets the output rather than the source files
        * Module: things that transform your code. `module.loaders` has many properties, but we’re mostly concerned with loaders for now
            - for example: "run .js files in src/ using babel-loader and use es2015 plugin to transform ES6 to ES5"
        * devtool: select options such as `source-map` to display the original source in the browser console (rather than compiled code) for ease of debugging
    - the webpack.config.js file is just a regular JS file evaluated by node.js
    - you can do normal things here like `require` and check env variables
    - webpack's config is placed in the object that the file exports to
        * export object is whatever you assign to `module.exports`
    - BONUS STUFF:
        * add something like `'webpack-dev-server/client?http://localhost:8080'` to entry to enable automatic reloading. Then you'll only need to refresh your `webpack-dev-server` process when you change the config file  
        * add the following to your webpack.config.js file to omit passing in the `--content-base` flag when you start the server:
        ```
            devServer: {
              contentBase: "./src"
            }
        ```
* make it even better with `package.json`
    - using `package.json` allows you to record your dependencies for webpack and babel and install them again with `npm install`
    - you can define a "start" script so you don't even have to type out `webpack-dev-server etc` -- just do `npm start`
    - see notes on node/npm to find out more




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
    - called from a function within a component
o handleChange:
    - defined within the component
    - gets called when onChange is called on the component
    - handleChange calls setState to refresh the DOM with the new data
o onChange:
    - an attribute which will call the specified method every time the
      value of it's associated element changes
    - for example, you may want to call the element's handleChange method

UNIVERSAL RENDERING
o it is cool -- makes your single page app possible without (or with
  fewer) server requests
o check out
    - webpack isomorphic tools
    - redux async connect
    - react-redux-universal-hot-example



REACT NATIVE
looks different -- but it's built on reactJS
Instead of rendering to DOM elements, native platform-specific interface components are rendered
writing an app: use command+d to pull up debugging tools


NOTES:
o what is blurring a field?
o what is interpolation?
o context feature of React -- wires everything up automatically

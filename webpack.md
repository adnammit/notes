# Webpack

## Overview
* Webpack is a module bundler for modern JS applications
* Used to load modules, minify code and produce a bundle that’s ready for production
* Common build steps include:
	* Module loading: look for ‘require’ and ‘import’ statements in the source code to gather those dependencies automatically
	* Concatenation: combine multiple files into one for more efficient loading
	* Minification: removal of whitespace and comments to shrink the size of the code
* **vite** is now the preferred build tool for many modern frameworks, but webpack is still widely used

## How To
* installation and setup:
	* install `webpack-dev-server` globally
	* install `webpack-dev-server webpack babel-core babel-loader babel-preset-env babel-polyfill` locally.
	* put all your source files in a source dir (like `src`), set up your config file (below) and then run this from inside the project directory to kick it off:
		`webpack-dev-server --content-base src src/main.js`
* webpack.config.js:
	* There are many ways to configure webpack, but here are some key features:
		* Entry: the array of files to be run at startup. Dependencies will be built from there
			* self-contained scripts go here
			* ex: you might want to run `babel-polyfill` or something and then your `main.js`
		* Output: defines where the application will go once build is performed, when it’s time to deploy. Output is specified by ‘path’ (a directory) and ‘filename’ (the name of the bundled file)
			* the browser gets the output rather than the source files
		* Module: things that transform your code. `module.loaders` has many properties, but we’re mostly concerned with loaders for now
			* for example: "run .js files in src/ using babel-loader and use es2015 plugin to transform ES6 to ES5"
		* devtool: select options such as `source-map` to display the original source in the browser console (rather than compiled code) for ease of debugging
	* the webpack.config.js file is just a regular JS file evaluated by node.js
	* you can do normal things here like `require` and check env variables
	* webpack's config is placed in the object that the file exports to
		* export object is whatever you assign to `module.exports`
	* BONUS STUFF:
		* add something like `'webpack-dev-server/client?http://localhost:8080'` to entry to enable automatic reloading. Then you'll only need to refresh your `webpack-dev-server` process when you change the config file
		* add the following to your webpack.config.js file to omit passing in the `--content-base` flag when you start the server:
		```
			devServer: {
			  contentBase: "./src"
			}
		```

## Using package.json
* using `package.json` allows you to record your dependencies for webpack and babel and install them again with `npm install`
* you can define a "start" script so you don't even have to type out `webpack-dev-server etc` -- just do `npm start`
* see notes on node/npm to find out more

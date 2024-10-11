# Vite

## Overview
* [docs](https://vitejs.dev/guide/)
* Vite is a build tool and development server
	* provides hot module loading when working locally
	* provides bundling for production deployment
* replaces webpack, rollup, parcel, gulp, snowpack and others for developing with ts, vue, react, svelte and more!
* why is Vite so fast?
	* vite in development mode leverages modern browsers' support of module imports (ESM): import each single file as a module (`<script type="module">`) 
	* HMR (hot module replacement) via websockets
	* a change to one file does not require minifying/recompiling the entire project
	* ES modules are used in local build compilation (during development) for maximum speed, while also producing a production build in a backwards compatible format
* **plugin ecosystem**:
	* Vite supports rollup plugins
	* writing your own is not too hard
	* SSR plugins


## Vite vs Webpack
* vite only bundles the necessary parts of code during development and uses module imports -- webpack bundles all dependencies into a single file
* being more established, webpack has a much wider range of plugins and loaders
* though vite provides a faster development experience, webpack may be the more efficient option for production builds as it has more plugins available to optimize the build


## Config
* [config options](https://vitejs.dev/config/)
* the config is defined in `vite.config.js` -- this file is a regular JS module that exports an object. this can be done in various ways, including async and use of if/else statements

## Rollup
* Vite uses the module bundler [Rollup](https://rollupjs.org/configuration-options)


## Compilation
* Vite started by using Babel, but you now also have the option of using SWC -- a faster alternative to Babel


## Static Assets
* **static files** include files referenced in code that are not code -- primarily images
* there are two ways to use static assets in a vite application:
	* **/public folder**: files in the public directory will be copied to the output directory when you `vite build`. this is a good method for favicon and anything else that is not directly referenced by your js code
	* **importing**: you can import static assets in your code and vite will copy them to the output directory with a cache-busting name. this is preferred for images and other assets that are referenced in your js code
	```js
		import logo from './assets/logo.png'
		//...
		<img src={logo} />
	```


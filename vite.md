# Vite

## Overview
* [docs](https://vitejs.dev/guide/)
* Vite is a build tool and development server
	- provides hot module loading when working locally
	- provides bundling for production deployment
* replaces webpack, rollup, parcel, gulp, snowpack and others for developing with ts, vue, react, svelte and more!
* why is Vite so fast?
	- module imports (ESM): import each single file as a module
	- hot module reload via websockets
	- a change to one file does not require minifying/recompiling the entire project
* **esbuild**: used in local build compilation
	- what actually compiles files as Vite serves the assets
* **plugin ecosystem**:
	- Vite supports rollup plugins
	- writing your own is not too hard
	- SSR plugins

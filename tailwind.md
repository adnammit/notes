# Tailwind

## Resources
* [docs](https://tailwindcss.com/docs/installation)
* install the vscode Tailwind CSS intellisense extension to help with class name suggestions
* use the [tailwind prettier extension](https://github.com/tailwindlabs/prettier-plugin-tailwindcss) to sort/organize your class names

## Overview
* tailwind is a css framework packed with utility classes (like `flex`, `text-center`, etc) that can be used right in your html/jsx markup
* what is **utilty-first approach**? build lots of tiny classes with one single purpose, then combine them to create complex designs. similar to **atomic css**
* so you don't write any css, just use tailwind's classnames
* pros:
	* you don't need to think about class names
	* no need to jump between html and css files
	* you can immediately understand the styling of any file/project that uses tailwind
	* as a design system, many decisions have been made for you -- UIs look better and more consistent
	* it probably handles responsive design better than you do
	* built in [accessibility features](https://dev.to/devsatasurion/is-tailwind-css-accessible-52dc)
	* docs and vscode integration are world-class
* cons:
	* makes markup look unreadable -- you sometimes need to use many many class names to get what you need
	* lots of class names to learn
	* tailwind needs to be installed and setup for each new project
	* you're giving up on vanilla css: you won't be writing css anymore




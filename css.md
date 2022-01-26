# CSS
https://app.pluralsight.com/player?course=css-intro&author=scott-allen&name=css-box&clip=6&mode=live
***pick it up with display and visibility***

code academy:
x box model
x display and positioning

* **selector**
* **property name/value**

## BASIC PRINCIPLES

### SELECTORS
* a **selector** indicates the elements to which a style should be applied (`div`, `#menu`,`a:hover`,`.content h2` etc)
* **descendant** selectors apply only to elements which are descendants of other specified elements
* **child** selectors apply to elements who are direct descendants of other specified elements
* **attribute** selectors allow for the selection of elements with a given attribute
* **pseudo class** selectors which select based on some inherent characteristic of an element, such as `a:visited`


### PROPERTY VALUES
* **keywords**: when using certain selectors, you can provide "shorthand" property values like "thin", "thick", "larger" etc
* **physical measurements**:
	- inches (`in`)
	- points (`pt`)
	- picas (`pc`)
	- pixels (`px`)
* **relative measurements**:
	- percentage (`%`)
	- ems (`em`)
* **colors**:
	- hex (`#FFF`)
	- rbg (`rgb(r,g,b)`)
	- name ("blue")
* fonts
	- font name ("Times New Roman")
	- font family ("Times")
	- font type ("sans-serif")
* functional notation
	- `rbg(43,12,53)`
	- `background-image:url("img/bg.jpg")`


### IDS AND CLASSES
* there can only be one element per doc with a given id (`#menu`)
* there can be multiple elements per doc with a given class (`.header`)


### SYNTAX
* selector examples:
	```css
	/* element selector */
	body {}

	/* id selector */
	#menu {}

	/* class selector */
	.header {}

	/* select all p's that are descendants of divs */
	div p {}

	/* select all direct children h1 of header class */
	.header > h1 {}

	/* select any image elements with 'alt' attributes of "spacer" -- <img src="foo.jpg" alt="spacer" /> */
	img[alt=spacer] {}

	/* select any visited anchor elements */
	a:visited {}

	/* star selector: default for everythinnnnnnng */
	* {}
	```

### CASCADING
* css is cascading, so given several rules for a given element:
	- the **most specific** rule is the one that will apply
	- if they are both the same specificity, the **most recent** rule will apply -- the one that comes last will be used
		* if a page has two rules on the same selector
		* if there are two different css files used by the same page that use the same selector
		* if one css file imports another
	- **specificity** is determined by a rating that has three components. the higher the rating, the more important the rule. the components of the rating (A, B and C) are:
		* A: Count of ID selectors                          -- most important
		* B: Count of class and attribute selectors         -- kinda important
		* C: Count of type selectors                        -- least important
* additionally one must consider the **source** of the style. the source from which a style originates determines its weight (that is, the precedence it is given when it conflicts with another rule from another source).
* css sources include:
	- author stylesheets: provided by the site author via `<link>` or `@import`
		* these typically carry the most weight
	- user stylesheets: users may specify styles (such as accessibility settings etc)
		* carry some weight
		* if a user style sheet contains the `!important` flag, it overrides the author style (so if a user needs larger font, they can override the author's styles)
	- default stylesheets ("user-agent"): "use these when there are none in effect"
		* carry the least weight
* but actually there's one more _gotcha!_ -- an inline style will take the very most precedence
* having trouble figuring out where a style is coming from? use yr dev tools, yo - it'll tell you where the styles are coming from and if they're being overridden (strikethrough)
* if you have a very specific design, don't like that your style looks different in a particular browser or want more consistent results, you can use a **css reset** stylesheet to provide a spread of basic styles (and fewer "fall throughs" that are handled by the "user-agent", potentially in different ways)
* rule overlap: just because one higher precedence rule changes one property doesn't mean it has any effect on the other properties
	```css
		/* a p elem inside a div will be red but also 1.5 em because there is nothing in the first rule to contradict the second */
		div p {
			color: 'red';
		}
		p {
			color: 'green';
			font-size: 1.5em;
		}
	```

### INHERITANCE
* certain properties are inherited automatically from a parent element to its children without the need to specify
* inherited values can be overridden with rules
* can it be inherited? well some can and some can't. font size can, border props can't
	- margins, padding, border typically don't
	- font size and list-related styles tend to inherit


## SIZING AND POSITIONING ELEMENTS

### FONT SIZE
* `pixels`: an absolute measurement that is always the same size, regardless of screen size
* `ems`: ems are a relative unit based on the default font size
	- in most browsers, 16px is the default font size, so in most cases 1em == 16px, 2em == 32px, etc
	- ems are better suited to the cascading style of css
* `rems`: "root em", acts in a similar way to ems, but with regard to the root element (that is, all size is relative to the font size specified for the html element)


## DISPLAY AND POSITIONING

### HEIGHT AND WIDTH
* to deal with various sizes of view screens, we can use `min-width` and `min-height` to specify the smallest possible dimensions an element should be rendered as
* likewise, you might not want your element to be too large, so you can use `max-width` and `max-height`
* if your interior content is too big for the `max-width` you've specified, it can spill outside the box

### BOX MODEL
* understanding the box model is key to understanding how internet layout do
* **border**: the physical boundary of our element
* **padding**: the space between the border of an element and its children
* **margin**: the space between an element and its parent
	- **vertical margin collapse**: if one elem has a vertical margin of 5px and another below it also has a vertical margin of 5px, you'd think there would be 10px between them, right? WRONG!
		* vertical margin specifies how much space is at minimum guaranteed to be between the borders of vertically aligned elements, so if one element has a vertical margin of 10px and the one below it has a vertical margin of 30px, there will be 30px between them
		* this does not apply to horizontal margin or horizontally stacked elements -- if two elements have horizontal margins of 10px each, there will be 20px between them
* specifying margin and padding:
	- syntax: up to four values for "top right bottom left", two values for "top/bottom right/left" or one value for "on all sides":
		```
			padding: 10px 20px 30px 40px;   // different padding on all sides
			margin: 5px 20px 10px;          // top is 5, right and left are 20, bottom is 10
			padding: 10px 40px;             // 10px top/bottom and 40px right/left
			padding: 10px;                  // 10px all sides
		```
	- using the line `margin: 0 auto;` sets top and bottom margins to zero and balances the margin right and left -- a sneaky, effective way of centering your work. however! your element must also have a width (not 100%) for this to work
	- you can assign negative values.
		* for example: the following code moves the element up (-5px) from the place it normally would have sat with 0 margin, placed 0px left and right, and 0px on the bottom
			`margin: -5px 0 0;`
	- you can also use direction-specific properties to specify margin and padding:
		- margin-top, margin-left, etc
		- padding-bottom, padding-right, etc.
* **box width**:
	- width can be specified in px, percentage, mm, inches etc
	- when you specify an absolute width (like 250px) you are specifying the content width. this means that if you add margin, border or padding styling to an elem, that will increase the effective width of an element, so an elem with `width: 250px; margin: 10px;` will have an effective width of 270px


### POSITION
* the default value for the `position` property is **static** -- elements are stacked one on top of the other and do not share horizontal space.
* **relative**: where the element should be positioned relative to it's default position. must be used in tandem with an **offset property**
* **absolute**: all other elements on the page ignore this element and act as though it doesn't exist. the target element is positioned relative to it's closest positioned parent. also probably need **offset properties** to behave correctly
* **fixed**: like absolute but when the page is scrolled the element remains fixed on the screen
	- this is useful for navigation bars
* **offset properties** specify how much and in what direction an object should be placed
	- **top**: moves the element down
	- **bottom**: moves the element up
	- **right**: moves the element left
	- **left**: moves the element right
	- this element will sit below and to the right of where it would by default:
	```css
		.box {
			position: relative;
			top: 10px;
			left: 50px;
		}
	```
* **z-index**: when elements overlap each other, which one will be on top? the `z-index` property controls the "depth" of an element -- how far back or forward it is in the imaginary depth of the page
	- z-index accepts integers as its value -- the greater the number, the further "forward"
	- z-index does _not_ work on static-position elements

### DISPLAY
* the **display** property indicates if an element can share horizontal space with other elements
* possible values for display are block, inline or none
* some types of elements have a certain display behavior by default ( are block, for example, while are inline). the inline value is used for forcing elements away from their default behavior
	- **block** elements sit on top of each other -- they take up the entire horizontal space
		* example elements that are block by default: `div`, `h#`, `p` and `li`
		* block level elements can be modified with `height` and `width` properties but the default width is 100%
	- **inline** elements only stack vertically when there is not enough space for them horizontally
		* example elements that are inline by default: `span`, `em` and `a`
		* _inline elements cannot be altered in size with the `height` or `width` CSS properties_
	- **inline-block** combines features of block and inline: inline-block elements can appear next to each other and their height and width _can_ be modified
		* example elements that are inline-block by default: `img`
	- **none** removes an element
* using `display: block;` in conjuction with `margin: 0 auto 0;` gives us more flexibility than using the text property `center` -- better for formatting to variable screen sizes
* the **visibility** property allows elements to be hidden from (or brought into) view
	- hidden elements are not visible but reserve space
* what's the difference between `display: none;` and `visibility: hidden;`?
	- an element with `display: none;` will be completely removed from the web page
	- an element with `visibility: hidden;` will not be visible but the space reserved for it will

### FLOAT AND CLEAR
* **float** is another property which controls an elements display/position, moving as far to the left or right of the screen as possible. it's sort of another way to make elements sit next to each other (if there's horizontal space for both of them)
* as you'd imagine, float takes two possible values: **left** and **right**
* floated elements _must have a width specified_ otherwise it till assume the full width of its containing element, and changing the float won't appear to have any effect
* **clear** is the helpful companion to `float` and controls what happens when multiple floated elements "bump" into each other. If an element follows a floated element or elements, it may push up into the element in ways it's not supposed to. it must be "cleared" of the floated element(s)
* values for `clear` include:
	- **left**: the left side of the element will not touch any other element within the same containing element
	- **right**: the right side of the element will not touch any other element within the same containing element
	- **both**: neither side of the element will touch any other element within the same containing element
	- **none**: either side of the element can touch


### OVERFLOW
* the **overflow** property controls what happens when content is too large to fit inside its containing element
* possible values for `overflow` include:
	- **hidden**: the content that overflows will not be seen
	- **scroll**: a scroll bar will be added so that the rest of the element can be viewed by scrolling
	- **visible**: the overflow content will be displayed outside of the containing element. this is the default behavior

### BORDER
* the **border** property takes several arguments:
		`border: 3px solid #432;`
* you can also specify a radius to your border so it doesn't need to be square
	- `border-radius` can be in px or %
	```css
		div.container {
			border: 2px dotted #432;
			border-radius: 100%:
		}
	```
* using radius gives you a circular border on your images. WHOA!
	- using "border-radius: 100%;" gives you a perfect circle
	- using "border-radius: 20px" will give you subtly rounded corners




# old notes to add:

IMAGES:
o some devices have screens with double-pixel density and should be
  created at twice the size to show up sharply on such screens
	- not super common, but becoming moreso


FONT:
o heading fonts are automatically set to bold but can be overridden
o some basic properties of fonts;
	- font-family: <specific font>, <backup font family(sans-serif, etc)
	- margin: space around the block of text
	- font-size: given in px or em's
	- font-weight: normal, bold, italic, etc
	- line-height: space between text, given in em's usually




### LESS
* language extension for CSS
* variables:
	- instead of typing `#5B83AD` over and over again, assign it to a nice variable name.
	```css
		@nice-blue: #5B83AD;
		@light-blue: @nice-blue + #111;

		#header {
		  color: @light-blue;
		}
	```
* Mixins: way of "mixing in" a bunch of properties from one rule set to another
	```css
		.bordered {
		  border-top: dotted 1px black;
		  border-bottom: solid 2px black;
		}

		#menu a {
		  color: #111;
		  .bordered; /* contains all properties of .bordered */
		}

		.post a {
		  color: red;
		  .bordered;
		}
	```
	- you can also use `#ids` as mixins.
* nesting

SASS: SYNTACTICALLY AWESOME STYLE SHEETS
o from the dir you're working in, type:
	$ sass --watch inputFile.sass:outputFile.css
o sass will then automatically refresh the css file every time you save
  the sass file

# CSS
https://app.pluralsight.com/player?course=css-intro&author=scott-allen&name=css-box&clip=6&mode=live
***pick it up with dispoay and visibility***

* **selector**
* **property name/value**

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


### BOX MODEL
* understanding the box model is key to understanding how internet layout do
* **border**: the physical boundary of our element
* **padding**: the space between the border of an element and its children
* **margin**: the space between an element and its parent
    - **vertical margin collapse**: if one elem has a vertical margin of 5px and another below it also has a vertical margin of 5px, you'd think there would be 10px between them, right? WRONG!
        * vertical margin specifies how much space is between the borders of vertically aligned elements
        * this does not apply to horizontal margin or horizontally stacked elements
* **box width**:
    - width can be specified in px, percentage, mm, inches etc
    - when you specify an absolute width (like 250px) you are specifying the content width. this means that if you add margin, border or padding styling to an elem, that will increase the effective width of an element, so an elem with `width: 250px; margin: 10px;` will have an effective width of 270px
* display and visibility:
    * **display** is usually set to block, inline or none. some types of elements have a certain display behavior by default (divs, li's etc are block -- span are inline)
        - **block** elements sit on top of each other
        - **inline** elements only stack vertically when there is not enough space for them horizontally
        - **none** removes an element
    * **visibility**
        - hidden elements are not visible but reserve space




# old notes to add:

IMAGES:
o some devices have screens with double-pixel density and should be
  created at twice the size to show up sharply on such screens
    - not super common, but becoming moreso


UNITS:
o pixels
o ems: ems are a relative unit. for example, if our font is set to 1.75
  ems, it will be displayed at 1.75 times the size of the default font
  size.
    - one em is equivalent to the height of the default font which is set
      to 16px in most browsers by default
    - ems are better suited to the cascading style of css.

FONT:
o heading fonts are automatically set to bold but can be overridden
o some basic properties of fonts;
    - font-family: <specific font>, <backup font family(sans-serif, etc)
    - margin: space around the block of text
    - font-size: given in px or em's
    - font-weight: normal, bold, italic, etc
    - line-height: space between text, given in em's usually




CODING STYLE
o "normalize.css": in the treehouse example, normalize.css is used to
  create a baseline display so that the html will display the same in all
  browsers. we will then add our own custom css on top of this baseline.
o you may select elements multiple times within the same body of code
    - e.g.: rather than doing one selection for 'body', one for 'h1', etc
      you may choose to have a 'colorschemes' section, a 'position'
      section, a 'font' section, etc




**PROPERTIES**

MARGIN AND PADDING:
o using the line "padding: 0 auto;" sets top and bottom margins to zero
  and balances the margin right and left -- a sneaky, effective way of
  centering your work
o you can assign negative values.
    - for example: the following code moves the element up (-5px) from the
      place it normally would have sat with 0 margin, placed 0px left
      and right, and 0px on the bottom
	margin: -5px 0 0;
o what's the difference between using one, two, three and four values
  for margin/padding?
    - margin: 10px;    
	* sets all margins to 10px
    - margin: 5px 0;
	* sets top and bottom margins to 5px, left and right
	  margins to 0
    - margin: 5px 20px 10px;
	* top is 5, right and left are 20, bottom is 10
    - margin: 5px 20px 10px 12px;
	* top is 5, right is 20, bottom is 10, left is 12
o you can also use the following properties to specify margin and
  padding:
    - margin-top, margin-left, etc
    - padding-bottom, padding-right, etc.

WIDTH:
o given as a percentage or px
o 'max-width':
    - set to 100%, it will fill the volume of the parent container
o width: 45%;   
    - the element will take up 45% of the width of its parent element
    - this example allows for up to 2 such elements to be displayed
      side-by-side with 10% of the page width left over
    - this "extra space" can be used to pad the elements nicely with:
	padding: 2.5%;


FLOAT:
o allows elements to appear side-by-side, rather than being displayed
  consecutively down in a single column
o floated elements don't have an explicit width, so make sure to set it
  to 100% unless you mean something else

CLEAR:
o helpful companion to 'float'
o if an element follows a floated element or elements, it may push up into
  the element in ways it's not supposed to. it must be "cleared" of the
  floated element(s)
o allows elements to be "cleared" of other elements, both top/bottom,
  and left/right

DISPLAY
o without specifying this property, an element's display is either 'block'
  or 'inline'
o block elements seem to push other elements out of the way
    - sections, divs, etc that have a specific width and height
o inline elements
    - mostly just text -- fluid, follows lines
o display can be set to 'inline-block' which gives inline character to
  block elements
    - ex: displaying images in a row
o using "display: block;" in conjuction with "margin: 0 auto 0;" gives us
  more flexibility than using the text property "center"
    - better for formatting to variable screen sizes


BORDER:
o using radius gives you a circular border on your images. WHOA!
    - using "border-radius: 100%;" gives you a perfect circle
    - using "border-radius: 20px" will give you subtly rounded corners




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

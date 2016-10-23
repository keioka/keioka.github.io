---
published: false
layout: post
title: Welcome to Jekyll!
date: '2016-10-21 02:56:10 -0700'
categories: performance
---
# HTML Questions

### * Q: What does a doctype do?

#### The doctype declaration...
- should be the very first thing in an HTML document, before the tag.
- is not an HTML tag; it is an instruction to the web browser about what version of the markup language the page is written in.
- refers to a Document Type Definition (DTD). The DTD specifies the rules for the markup language, so that the browsers render the content correctly. 


#### doctype is used for...
- You will not be able to use a HTML (HyperText Markup Language) Validator to check the page coding. HTML validation requires the DOCTYPE declaration.
- The browser rending the web page will process the coding in Quirks Mode.
- The stylesheet may not be implemented as planned.

In HTML5, the only purpose of the DOCTYPE is to activate full standards mode. Older versions of the HTML standard gave additional meaning to the DOCTYPE, but no browser has ever used the DOCTYPE for anything other than switching between quirks mode and standards mode.



### * Q: What's the difference between full standards mode, almost standards mode and quirks mode?
There are now three modes used by the layout engines in web browsers: quirks mode, almost standards mode, and full standards mode. 

- In quirks mode, layout emulates nonstandard behavior in Navigator 4 and Internet Explorer 5. This is essential in order to support websites that were built before the widespread adoption of web standards. 

- In full standards mode, the behavior is (hopefully) the behavior described by the HTML and CSS specifications. 

- In almost standards mode, there are only a very small number of quirks implemented.

### * Q: What's the difference between HTML and XHTML?
XHTML is an application of XML, which is quite a strict angle-bracket language.
HTML is an application of SGML, which is a much less strict angle-bracket language.
XHTML will be treated an application of XML only in case where MIME type application/xhtml+xml, application/xml, or text/xml are used.

XHTML 
- must be properly nested.
- must always be closed.
- must be in lowercase.
- must have one root element.


### * Q: Explain Web workers
A web worker is a script that runs in the background (i.e., in another thread) without the page needing to wait for it to complete. The user can continue to interact with the page while the web worker runs in the background. Workers utilize thread-like message passing to achieve parallelism.

### * Q: Are there any problems with serving pages as application/xhtml+xml?
### * Q: How do you serve a page with content in multiple languages?
### * Q: What kind of things must you be wary of when design or developing for multilingual sites?

### * Q: What are data- attributes good for?

data-* attributes allow us to store extra information on standard, semantic HTML elements without other hacks such as non-standard attributes, extra properties on DOM, or setUserData.

### * Q: Consider HTML5 as an open web platform. What are the building blocks of HTML5?
### * Q: Describe the difference between a cookie, sessionStorage and localStorage.
### * Q: Describe the difference between `<script>`, `<script async>` and `<script defer>`.

### * Q: Why is it generally a good idea to position CSS `<link>`s between `<head></head>` and JS `<script>`s just before `</body>`? Do you know any exceptions?

The difference is the timing of being loaded. The higher you put code on, the earlier code will be read. When you load CSS, it shoule be read first because it is important for visualization. On the other hand, Javascript file does not have to be loaded at the beginning especially if it handles the part of interaction. Moreover, loading Javascript files causes gaining latency. So that javascript files tend to load at the last part.


### * Q: What is progressive rendering?

Progressive rendering is the name given to techniques used to render content for display as quickly as possible.

It's still useful in modern development as mobile data connections are becoming increasingly popular.

#### Lazy loading 
Lazy loading of images where (typically) some javascript will load an image when it comes into the browsers viewport instead of loading all images at page load.

#### Prioritizing visible content 
Prioritizing visible content (or above the fold rendering) where you include only the minimum css/content/scripts necessary for the amount of page that would be rendered in the users browser first to display as quickly as possible, you can then use deferred javascript (domready/load) to load in other resources and content.




### Accept (Content-Types)

	- text/plain
	



---
# CSS Questions

## General
### * Q: Describe what a “reset” CSS file does and how it’s useful. Are you familiar with normalize.css? Do you understand how they differ?

Resets are so wildly common in CSS that anyone who is a front end developer type has surely used them. Do they do so blindly or do they know why? The reason is essentially that different browsers have different default styling for elements, and if you don't deal with that at all, you risk designers looking unnecessarily different in different browsers and possibly more dramatic breaking problems.

Normalize you might call a CSS reset alternative. Instead of wiping out all styles, it delivers a set of reasonable defaults. It doesn't unset things that are already consistent across browsers and reasonable (e.g. bold headers). In that way it does some less than a reset. It also does some more than a reset in that it handles quirks you may never consider, like HTML5 audio element inconsistencies or line-height inconsistencies when you use sub and sup elements.

### * Q: The 'C' in CSS stands for Cascading. How is priority determined in assigning styles (a few examples)? How can you use this system to your advantage?

### * Q: What existing CSS frameworks have you used locally, or in production? How would you change/improve them?
### * Q: What are some of the "gotchas" for writing efficient CSS?

---
# Float
### * Q: Describe Floats and how they work.

- A floated element is "invisible" to its parent

[how-floating-works](https://bitsofco.de/how-floating-works/)

### * Q: What are the various clearing techniques and which is appropriate for what context?

- create div and set `clear: both` to style. 
- Overflow
- Psuedo element `:after`

https://css-tricks.com/all-about-floats/

Floats are still incredibly common. As this is published, still probably the most cross-browser consistent way to build layout and grids. Anyone who has worked with them is aware of float collapsing. That is, floated element do not add to the height of a parent element. So for example if a parent element contained only floated elements, it would collapse to zero height. You can deal with that like:

- Use a clearfix (bonus points for micro clearfix).
- Float the parent as well.
- Use an overflow property other than "visible" on the parent (bonus points for listing downsides like cutting off shadows).
Bonus points for "create a new block formatting context". Possibly negative points for something like <br style="clear: both;">

As a bonus question, you could ask them to compare using inline-block and floats for building a grid. Good answer: there are problems either way. With inline block you need to deal with the whitespace issue. With floats you need to deal with clearing.


---

### * Q: Describe BFC(Block Formatting Context) and how it works.
A block formatting context is a part of a visual CSS rendering of a Web page. It is the region in which the layout of block boxes occurs and in which floats interact with each other.

A block formatting context is created by one of the following:

- the root element or something that contains it
- floats (elements where float is not none)
- absolutely positioned elements (elements where position is absolute or fixed)
- inline-blocks (elements with display: inline-block)
- table cells (elements with display: table-cell, which is the default for HTML table cells)
- table captions (elements with display: table-caption, which is the default for HTML table captions)
- elements where overflow has a value other than visible
- flex boxes (elements with display: flex or inline-flex)

A block formatting context contains everything inside of the element creating it that is not also inside a descendant element that creates a new block formatting context.

Block formatting contexts are important for the positioning (see float) and clearing (see clear) of floats. The rules for positioning and clearing of floats apply only to things within the same block formatting context. Floats do not affect the layout of things in other block formatting contexts, and clear only clears past floats in the same block formatting context.

### * Q: Explain the three main ways to apply CSS styles to a Web page and advantage, disadvantage
- #### Inline
    - ##### The advantages of Inline Styles are:
        1. It is especially useful for small number of style definitions.
        2. It has the ability to override other style specification methods at the local level.

    - ##### The disadvantages of Inline Styles are:
        1. It does not separate out the style information from content.
        2. The styles for many documents can not be controlled from one source.
        3. Selector grouping methods can not be used to handle complex situations.
        4. Control classes can not be created to control multiple element types within the document.
    
    
- #### Embedded/Internal
    - ##### The advantages of Embedded Style Sheets are:

        1. It is possible to create classes for use on multiple tag types in the document
        2. Under complex situations, selector and grouping methods can be used to apply styles.
        3. No extra download is required to import the information.
    - ##### The disadvantages of Embedded Style Sheets are:
        1. It is not possible to control the styles for multiple documents from one file, using this method.


- #### Linked/External
    - ##### The advantages of External Style Sheets are:
        1. Using them, the styles of multiple documents can be controlled from one file.
        2. Classes can be created for use on multiple HTML element types in many documents.
        3. In complex situations, selector and grouping methods can be used to apply styles.

    - ##### The disadvantages of External Style Sheets are: 
        1. In order to import style information for each document, an extra download is needed. 
        2. Until the external style sheet is loaded, it may not be possible to render the document. 
        3. For small number of style definitions, it is not viable.


---

## Performance Optimization

### * Q:What are the advantages and disadvantages of External Style Sheets?

http://www.skilledup.com/articles/25-css-interview-questions-answers

### * Q: Explain CSS sprites, and how you would implement them on a page or site.

An image sprite is a collection of images put into a single image.
A web page with many images can take a long time to load and generates multiple server requests. Using image sprites will reduce the number of server requests and save bandwidth.

https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_variables

#### Generate sprites image

```
$ npm install sprity -g
```

```
$ sprity ./output-directory/ ./input-directory/*.png
```
https://css-tricks.com/css-sprites/


### Use

####

####

### * Q: What are your favorite image replacement techniques and which do you use when?

CSS image replacement is a technique of replacing a text element (usually a header tag like an `<h1>`) with an image (often a logo)
It has its origins in the time before web fonts and SVG. For years, web developers battled against browser inconsistencies to craft image replacement techniques that struck the right balance between design and accessibility.

### * Q: How would you approach fixing browser-specific styling issues?


### * Q: How to use CSS variable?
CSS Variables

```html
<div class="one">
  <div class="two">
    <div class="three">
    </div>
    <div class="four">
    </div>
  <div>
</div>
```

```css
:root {
  --main-bg-color: brown;
}

.one {
  background-color: var(--main-bg-color);
}

```


### * Q: How do you serve your pages for feature-constrained browsers?

### * Q: media queries or mobile specific layouts/CSS?

```html
<link rel="stylesheet" type="text/css" media="only screen and (max-device-width: 480px)" href="small-device.css" />
```

### * Q: Explain how a browser determines what elements match a CSS selector.



---

## Selector
### * Q: What is the difference between classes and IDs in CSS?

A class can be used several times, while an ID can only be used once, so you should use classes for items that you know you're going to use a lot. An example would be if you wanted to give all the paragraphs on your webpage the same styling, you would use classes.

* 'id' can be used for anchor link. (name property is also OK)

### * Q: Selector priority

- Inline style attribute - 1000
- ID - 100
- Class, psuedo-class, attributes - 10
- element - 1
- The universal selector (*) has no specificity value (0,0,0,0)

The negation pseudo-class :not is not considered a pseudo-class in the specificity calculation. But selectors placed into the negation pseudo-class count as normal selectors when determining the count of selector types.

The !important value appended a CSS property value is an automatic win. It overrides even inline styles from the markup. The only way an !important value can be overridden is with another !important rule declared later in the CSS and with equal or great specificity value otherwise. You could think of it as adding 1,0,0,0,0 to the specificity value.


### * Q: What are child selectors?

### * Q: What is grouping and what is it used for?

### * Q: Explain what elements will match each of the following CSS selectors?
1. div, p
2. div p
3. div > p
4. div + p
5. div ~ p

### * Q: Why we should not use css class like this?

```css
.redError {
  color: red;
}

.font-16 {
  font-size: 16px;
}
```

The color might be changed.

### * Q: Describe pseudo-elements and discuss what they are used for.

Pseudo classes 
- are similar to classes, but are not explicitly defined in the markup, and are used to add additional effects to selected HTML elements such as link colors, hover actions, etc.
- are defined by first listing the selector, followed by a colon and then pseudo-class element. E.g.,  a:link{ color: blue }, or a:visited { color: red }
describing a special state, they allow you to style certain parts of a document. For example, the ::first-line pseudo-element targets only  the first line of an element specified by the selector.

### * Q: What is the purpose of pseudo-elements and how are they made?
Pseudo elements 
- are made using a double colon (::) followed by the name of the pseudo element.
- are used to add special effects to some selectors, and can only be applied to block-level elements.
- are ::first_line, ::first_letter, ::before, ::after

#### pseudo-elements list
- ::after
- ::before
- ::first-letter
- ::first-line
- ::selection
- ::backdrop
- ::placeholder
- ::marker
- ::spelling-error
- ::grammar-error


#### pseudo class

- :active
- :any
- :checked
- :default
- :dir()
- :disabled
- :empty
- :enabled
- :first
- :first-child
- :first-of-type
- :fullscreen
- :focus
- :hover
- :indeterminate
- :in-range
- :invalid
- :lang()
- :last-child
- :last-of-type
- :left
- :link
- :not()
- :nth-child()
- :nth-last-child()
- :nth-last-of-type()
- :nth-of-type()
- :only-child
- :only-of-type
- :optional
- :out-of-range
- :read-only
- :read-write
- :required
- :right
- :root
- :scope
- :target
- :valid
- :visited

### Resource

- [Meaningful CSS: Style Like You Mean It](http://alistapart.com/article/meaningful-css-style-like-you-mean-it)
---

## Z-index

### * Q: Describe z-index and how stacking context is formed.

### * Q: z-index priority

---

## Box model

### * Q: Explain  box model and how you would tell the browser in CSS to render your layout in different box models.

- Content - The content of the box, where text and images appear
- Padding - Clears an area around the content. The padding is transparent
- Border - A border that goes around the padding and content
- Margin - Clears an area outside the border. The margin is transparent

### * Q: What does * { box-sizing: border-box; } do? What are its advantages?

---

## Display

### * Q: What are the different ways to visually hide content (and make it available only for screen readers)?
https://jsfiddle.net/keioka/Lsgu0ak9/
http://alistapart.com/article/now-you-see-me

```html
<p>This is div1</p>
<div class="div1">div1</div>
<p>This is div2</p>
<div class="div2">div2</div>
<p>This is div3</p>
<div class="div3">div3</div>
<p>This is div4</p>
<div class="div4">div4</div>
<p>This is div4</p>
<div class="div5">div5</div>
<p>end</p>
```


```css
.div1 {
  display: none;
}

.div2 {
  visibility: hidden;
}

.div3 {
  position: absolute;
  left: -9999em;
  top: auto;
  width: 0px;
  height: 0px;
  overflow: hidden;
}

.div4 {
  position: absolute; 
  overflow: hidden; 
  clip: rect(0 0 0 0); 
  height: 1px; 
  width: 1px; 
  margin: -1px; 
  padding: 0; 
  border: 0; 
}

.div5 {
  opacity: 0;
}
```
By applying that class to an element you've immediately made that content "inaccessible" by screen readers. You've probably known this forever, but still the poison apple sneaks into our code once in a while.

[What is the difference between visibility:hidden and display:none?](http://stackoverflow.com/questions/133051/what-is-the-difference-between-visibilityhidden-and-displaynone)
### * List as many values for the display property that you can remember.

|property name||
|---|---|
|inline|Default value. Displays an element as an inline element (like `<span>`)   |
|block| Displays an element as a block element (like `<p>`)|
|flex|  Displays an element as an block-level flex container.|  
|inline-block|  Displays an element as an inline-level block container. The inside of this block is formatted as block-level box, and the element itself is formatted as an inline-level box|
|inline-flex|   Displays an element as an inline-level flex container. New in CSS3  |
|inline-table|  The element is displayed as an inline-level table   | 
|list-item| Let the element behave like a `<li>` element  |
|run-in|    Displays an element as either block or inline, depending on context|     
|table| Let the element behave like a `<table>` element   |
|table-caption| Let the element behave like a `<caption>` element |
|table-column-group|    Let the element behave like a `<colgroup>` element|   
|table-header-group|    Let the element behave like a `<thead>` element   |
|table-footer-group|    Let the element behave like a `<tfoot>` element   |
|table-row-group|   Let the element behave like a `<tbody>` element   |
|table-cell|    Let the element behave like a `<td>` element  |
|table-column|  Let the element behave like a `<col>` element |
|table-row| Let the element behave like a `<tr>` element  |
|none|  The element will not be displayed at all (has no effect on layout)|
|initial|   Sets this property to its default value. Read about initial |
|inherit|   Inherits this property from its parent element.|

### * Q: What's the difference between inline and inline-block?

Elements with display:inline-block are like display:inline elements, but they can have a width and a height. That means that you can use an inline-block element as a block while flowing it within text or other elements.

### * Q: CSS Flexbox or Grid specs


### * Q: What is grid system?

---

## Position

### * Q: What's the difference between a relative, fixed, absolute and statically positioned element?
- Static
- Relative
- Absolute
- Fixed


### * Q: Is there any reason you'd want to use translate() instead of absolute positioning, or vice-versa? And why?

https://www.paulirish.com/2012/why-moving-elements-with-translate-is-better-than-posabs-topleft/



---

## font

### * Q: How would you implement a web design comp that uses non-standard fonts?

### Say you were tasked with coding a design that used non-standard web fonts, how would you go about it?
A non-leading way to get them to talk about @font-face and how it works. Talking about how it works as a core CSS technology is great, as well as talking about services that provide the fonts and can make it easier e.g. Google Fonts, Typekit, Font Deck, Cloud Typography, etc.

Bonus points for obscure knowledge like the history of @font-face syntax or Firefox's issue with cross-origin fonts.

### * Q: Why shouldn’t I use fixed sized fonts

### Font size (px, %, em, rem)
While em is relative to the font size of its direct or nearest parent, rem is only relative to the html (root) font-size. I like to think of it as a reset. If a style sheet is built in a modular fashion, then rem shouldn’t be needed very often, but it can be handy at times.

https://codemyviews.com/blog/whats-the-deal-with-em-and-rem
```css
html {
  font-size: 17px;
}
@media (max-width: 900px) {
  html { font-size: 15px; }
}
@media (max-width: 400px) {
  html { font-size: 13px; }
}

/* Type will scale with document */
h1 {
  font-size: 3rem;
}
h2 {
  font-size: 2.5rem;
}
h3 {
  font-size: 2rem;
}

```
### * Q: Which font names are available on all platforms?
Only five basic font families( serif, sans-serif, cursive, fantasy, and monsospace) are recognized across platforms, rather than specific fonts.
Specific font name recognitions will vary by browser.

---

## Property

### * Q: Why and how are shorthand properties used? Give examples.
http://www.skilledup.com/articles/25-css-interview-questions-answers


---

## Responsive

### * Q: How is responsive design different from adaptive design?

- Responsive websites respond to the size of the browser at any given point. 

- Adaptive websites adapt to the width of the browser at a specific points. In other words, the website is only concerned about the browser being a specific width, at which point it adapts the layout.

### * Q: List all Stylesheet Media Types

- all 
- aural
- braille
- embossed
- handheld
- print
- projection
- screen
- tty
- tv
---


## * Q: Cross Browser

* -moz- Mozilla
* -webkit- Safari, Chrome
* -o- Opera
https://developer.mozilla.org/en-US/docs/Web/CSS/Webkit_Extensions

---

## Practical

### * Q: In CSS3, how would you select
- Every <a> element whose href attribute value begins with “https”.
- Every <a> element whose href attribute value ends with “.pdf”.
- Every <a> element whose href attribute value contains the substring “css”.

```css
a[href^="https"]
a[href$=".pdf"]
a[href*="css"]
```

### * Q: How could you use CSS to achieve the following automatic numbering:

```html
<div id="page">
  <h1>Heading Title</h1>
  <h2>Subheading Title</h2>
  <h2>Subheading Title</h2>
  <h1>Heading Title</h1>
  <h2>Subheading Title</h2>
  <h1>Heading Title</h1>
</div>
```

Answer 
```css
#page {
  counter-reset: heading;
}

h1:before {
  content: counter(heading)") ";
  counter-increment: heading;
}

h1 {
  counter-reset: subheading;
}

h2:before {
  content: counter(heading)"." counter(subheading)") ";
  counter-increment: subheading;
}
```


### * Q: Explain to me what's going on in this CSS selector

```css
[role=navigation] > ul a:not([href^=mailto]) {

}
```

This selects anchor links that are not email links that are decedents of an unordered list that is the direct child of any element with a role attribute of 'navigation'.

Being able to verbalize a selector is proof you understand them and evidence they can communicate complex tech subjects.
https://css-tricks.com/interview-questions-css/













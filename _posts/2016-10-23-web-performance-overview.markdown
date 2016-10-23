---
published: false
---
# Why web performance?

80%-90% of the end-use response time is spent on front end

## Be sure to solve critical problem.

"premature optimization is the root of all evil." The surrounding context of this quote is important. His assertion is that optimizing on the 97% of non-critical code paths leads to uglier/harder-to-maintain code for little-to-no benefits. But, this implies that optimizing on the 3% of critical code paths is quite important. So, in reality, he’s more saying: “Optimizing non-critical code paths is the root of all evil.”

https://msdn.microsoft.com/en-us/magazine/gg622887.aspx

### Response time
The range of the response time between 100ms - 1000ms is not noticiable for user.

https://www.nngroup.com/articles/response-times-3-important-limits/

http://theixdlibrary.com/pdf/Miller1968.pdf


# Middle end

### YSlow
#### Optimization
https://developers.google.com/speed/docs/insights/rules?csw=1
Basic file optimization is these following things. There are a lot of things we can do on the backend such as SQL optimization however most of slow rendering cause from front-end and mid-end.

Minify -> Compress -> Cache

However we can do many more things and these are 14 optimization rules from Yahoo!

##### 14 rules
1. Fewer HTTP Requests
2. Use a CDN
3. Expires/Cache-Control Header
4. Gzip
5. Stylesheets at Top
6. Scripts at Bottom
7. Avoid CSS Expressions
8. External JS/CSS
    - note: inline style can not be cached
9. Fewer DNS Lookup
10. Minify JS/CSS
11. Avoid Redirects
12. Avoid Duplicate Scripts
13. Etags
14. Cacheable Ajax

### 1. Fewer HTTP Request
- image sprite
    - disadvantage: Use much CPU memory for giant image
- concat JS/CSS (minify and concating is different)
- gzip (http://www.gidnetwork.com/tools/gzip-test.php)
- Cookie Free Domain (https://www.keycdn.com/support/how-to-use-cookie-free-domains/ )
-
- avoid image empty value

##### Average page file size
https://www.soasta.com/blog/page-bloat-average-web-page-2-mb/

### 2. Use a CDN

### 3. Expires/Cache-Control Header

### 4. Gzip

Set gzip on web server such as Apache and Nginx. Content-Encoding: gzip.

### 5. Stylesheets at Top

### 6. Scripts at Bottom

### 7. Avoid CSS Expressions

### 8. External JS/CSS

### 9. Fewer DNS Lookup

### 10. Minify JS/CSS

### 11. Avoid Redirects

### 12. Avoid Duplicate Scripts

### 13. Etags

### 14. Cacheable Ajax


# Front end (Browser)

![img](https://www.html5rocks.com/ja/tutorials/internals/howbrowserswork/webkitflow.png)

## Understanding how browser works.

#### 1. Building DOM
![img](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/images/full-process.png)
Once browser receives the HTML file from server, DOM starts parsing it. It recognizes tag and nested structure and builds tokens of document.  
Characters -> Parse -> Tokens -> Nodes -> DOM tree

#### 2. CSSOM : [link](https://www.w3.org/TR/cssom-view-1/)

#####  How CSSOM works?
Likely DOM, CSSOM also generates tree structure of tree.

Characters -> Tokens -> Nodes -> CSSOM
![img](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/images/cssom-construction.png)
Cascading means it has tree hierarchy and style from parent of tree will inhere to child.

##### CSS is render blocking.

Web page is not available without CSS styling practically. The webpage without styling is called FOUC（Flash of Unstyled Content). Rendering tree waits the DOM and CSSOM to be created.
[render-blocking-css](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/render-blocking-css)




#### 3. Render Tree
Render tree create new tree which combined DOM and CSSOM except for `<head></head>`.

- `display: none` does not show render tree.
![img](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/images/render-tree-construction.png)

#### 4. Paint

Lots of things going on while the browser is painting.

## How 'Rendering' works
http://tympanus.net/codrops/css_reference


### Pixel pipeline
- Javascript
- Style
- Layout
- Paint
- Composite


|Stage|Benchmark|Explain|When?|
|-|-|-|-|
|Javascript or ( or CSS Animation or Web Animation API) | Change value of style | Even if value is changed at this moment, it does not mean all processes on pixel pipeline have to be triggered. |event triggered|
|Style | Recalculation from changes | | Any value is changed by JS or CSS |
|Layout | Reflow other elements| Check all the other elements| Relocation such as change `width`, `height`, `position`|
|Paint | | Loading data or UI | Repaint `color`, `box-shadow`|
|Composite | | Loading data or UI | ||

## RAIL
RAILS represents 'Response', 'Animate', 'Idle', and 'Load'. These are the points we have to take care of performance.

Each benchmark is below.

||Benchmark|Explain|
|-|-|-|
|Response| 100ms| The time between click and starting animation|
|Animate |10ms per frame (60fps)| Animation |
|Idle | 50ms ||
|Load | 1000ms | Loading data or UI |

### fps
What is fps? fps stands for 'Frame per second'. Believe or not, the browser and any devices refresh the screen 60 times per second. If it is below that, users will feel uncomfortable. Thus, we have to keep fps under 60 fps.

![img](http://i.imgur.com/WleUBGG.gif)

https://www.youtube.com/watch?time_continue=10&v=pfiHFqnPLZ4

- 60 fps and 16 ms per frame. However, browser has to do "housekeeping", thus benchmark is within 10ms per a frame.
If it is slower than benchmark which means frame rate is going down, it causes "Judder". It is called "Junk" and causes bad user experience.

http://jakearchibald.github.io/jank-invaders/

### Animation

What is animation for browser? It is recalculate, repaint, and re-composite.
As pixel pipeline, doing shortcut means cheaper for browser typically. Regular animation flow (pixel pipeline) is Recalculate Style, Layout, Paint, and Composite Layers. How we can avoid these process? Here is the another question. Why Recalculation and Layout, Paint, Composite happened? The answer is changing value which relates other elements. If you animate something, browser has to recalculate, layout, paint, and composite everything.   
Bigger CSSOM tree structure, bigger calculation time. Well this happens when you change and manage application state.

### The properties affects layout phase.

- width
- height
- padding
- margin
- display
- border-width
- border
- top
- left
- right
- bottom
- position
- font-size
- font-weight
- float
- text-align
- overflow-y
- overflow
- font-family
- line-height
- vertical-align
- clear
- white-space
- min-height

If you change these property for animation, it requires recalculation.


#### Property affects Paint

- color
- border-style
- visibility
- background
- text-decoration
- background-image
- background-position
- background-repeat
- outline-color
- outline
- outline-style
- border-radius
- outline-width
- box-shadow
- background-size


Repaint can be also expensive for mobile phone browser. When browser repaint, the layer on which the element animated belongs will transfer to GPU. Mobile has less GPU than laptop so that processing takes longer time than laptop. Moreover, transfer from CPU to GPU is limited so that updating texture will be slow.



#### Property which causes composition

There is the way to animate elements cheaply. They are changing `Position`, `Scale`, `Rotation`, `Opacity`.

As 'Opacity' changes, GPU between composite will handle and change alpha of element.
'transition' and 'animation'

transform and opacity property will not pass Style, Layout, Paint. The elements will be just re-composited, which means a lot of cheaper than recalculation.

You might have heard that `why you use transform rather than position: absolute for animation`. That is the one of the answer.









One great example of using transform is `First Last Invert Play`.

[CSS animated propertiesstack rank](https://www.chromestatus.com/metrics/css/animated)
[CSS properties by style operation required](https://docs.google.com/spreadsheets/d/1Hvi0nu2wG3oQ51XRHtMv-A_ZlidnwUYwgQsPQUg1R2s/pub?single=true&gid=0&output=html)

[Youtube: Position vs Transform](https://www.youtube.com/watch?v=-62uPWUxgcg)

[High-performance-animations](https://www.html5rocks.com/ja/tutorials/speed/high-performance-animations/)

[Animation Elements](https://drafts.fxtf.org/web-animations/animation-elements.html)

[animating-web-gonna-need-bigger-api](https://www.smashingmagazine.com/2013/03/animating-web-gonna-need-bigger-api/)

[Performance Tooling](https://docs.google.com/presentation/d/19R_E5B__kdE55L1bTpS6IFKbYbHq-PQKKky4do5Yc6A/edit#slide=id.g105c64d69_187)

#### Forced Synchronous Layouts

### Making frame

### Raster
Rasterizing is that vector image turns to pixel. Any images has to be turned to pixel.

### Encoded JPEG

Image Decode and resize


### Composite layer


### What is CPU and GPU?
Layer will be uploaded to GPU from CPU and GPU paint on the screen.





## Image optimization
### Resuce image size
- http://www.smushit.com/ysmush.it/
- http://imageoptimizer.net/
- http://pmt.sourceforge.net/pngcrush/

### Use image sprite
- http://spriteme.org
- http://spritegen.website-performance.org
- http://www.floweringmind.com/sprite-creator/
- http://dopiaza.org/tools/datauri/
- http://dataurl.net/#dataurlmaker
- http://dataurl.net/#cssoptimizer


### inline image

## CSS optimization
### Limit Load file by using inline media query
Loading and parsing CSS does block rendering and execute javascript. We have to reduce the number of critical css files. At least we can load only necessary files first using media query on `<link>` tag.

```
<link href="style.css"> // block rendering
<link href="style.css" media="screen"> // block rendering because all device has screen!!!
<link href="style-mobile.css">
<link href="style-tablet.css">
<link href="style-print.css" media="print">
```

These files are all read by engine however they don't block rendering while being loaded.


### Reduce recalculate time

#### 1. Reduce selector complexity
use fewer tags and class names to select elements

pseudo element is also on the render tree.

##### BEM is good solution!
Block element modular is naming convention of CSS styling in order to make selectors understandable and

- http://output.jsbin.com/gozula/1/quiet
- https://www.sitepoint.com/bem-smacss-advice-from-developers/
- https://en.bem.info/methodology/key-concepts/

#### 2. Reduce affected elements
fewer changes to render tree.

#### 3. Avoid Forced Synchronous Layout
[How (not) to trigger a layout in WebKit](http://gent.ilcore.com/2011/03/how-not-to-trigger-layout-in-webkit.html)


## Layout

Composite layer is also

https://aerotwist.com/blog/on-translate3d-and-layer-creation-hacks/

### will-change

Animation layout painting is expensive. If you will animate some object constantly, you should create layer by yourself. Browser does it automatically however sometimes it is created in wrong way. Creating too much layer by yourself is also not good practice.  

```css
div.animation {
  will-change: transform;
}

```
http://cssmojo.com/the-dark-side-of-the-will-change-property/

https://css-tricks.com/almanac/properties/w/will-change/

## JS optimization


### Avoid timer function, use requestAnimationFrame
Animation has to be run at the beginning of processing frame. We have to make sure that javascript will executed at the point. We can use the function called requestAnimationFrame. By using setTimeout and setTimeInterval, callback executed at the middle of building a frame which is bad for animation. jQuery uses setTimeout function for animation..

```javascript
/**
 * If run as a requestAnimationFrame callback, this
 * will be run at the start of the frame.
 */
function updateScreen(time) {
  // Make visual updates here.
}

requestAnimationFrame(updateScreen);
```


If you animate constantly, you need to call requestAnimationFrame inside the animation function recursively.

```javascript
/**
 * If run as a requestAnimationFrame callback, this
 * will be run at the start of the frame.
 */
function updateScreen(time) {
  // Make visual updates here.
  requestAnimationFrame(updateScreen);
}

requestAnimationFrame(updateScreen);
```

### Use Web Worker.

### Micro task

### Measure Javascript which cause bottle neck

### Heap and stack memory

### script tag blocks DOM construction.


#### Use async attribute on script tag
```
<script src="script.js" async></script>
```
1. Does not block DOM construction
2. Does not block on CSSOM


#### Minify JS/CSS

Compressor tool

• http://compressorater.thruhere.net

• http://marijnhaverbeke.nl/uglifyjs

• http://www.minifycss.com/css-compressor/

• http://jsbeautifier.org

• http://cssbeautify.org



Optimization tool

• http://zbugs.com



Article

• http://robertnyman.com/2010/01/19/tools-for-concatenating-and-minifying-css-and-javascript-files-in-different-development-environments/



## Architecture


## The Front-end: Resource Loading

### Pre Loading
Pre loading

### On-demand loading (Lazy Loading)
### Parallel Loading



# Thread
JS Engine can not be multi thread even async. It is single thread. While JavaScript has asynchronous code execution, the event loop is single threaded.
The UI Thread in the browser is single threaded. This means the CSS rendering engine, DOM, JavaScript engine, etc. are all on the same thread.

### Web Worker
Javascript is single thread and it can not execute functions in parallel. By using web worker, we can set another thread. Each thread such as main thread and web worker can talk each other and pass data.


It is good usage for web worker
- Loading process
- Search
- Sort

Not to
- manipulate DOM
-
### Garbage Collection

#### 1. Avoid global scope pollution
#### 2. Take care of huge Closure
#### 3. clear timer function


- [Writing fast memory efficient javascript]( https://www.smashingmagazine.com/2012/11/writing-fast-memory-efficient-javascript/)

- [Memory Management]( https://developer.mozilla.org/en-US/docs/Web/JavaScript/Memory_Management)

- [High-Performance, Garbage-Collector-Friendly Code](http://buildnewgames.com/garbage-collector-friendly-code/)

# resources
### http://webpagetest.org
### http://whichloadsfaster.com

[Faster HTML and CSS: Layout Engine Internals for Web Developers](https://www.youtube.com/watch?v=a2_6bGNZ7bA)

[A Reference Architecture for Web Browsers](http://grosskurth.ca/papers/browser-refarch.pdf)
# terminology
### waterfall chart
### Visual progress


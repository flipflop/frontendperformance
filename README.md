#Front End Performance Tips and Tricks

Performance Golden Rule: 80-90% of the end-user response time is spent on the frontend. Start there.
- Steve Souders

##CSS

- IDs are resolved quicker than classes (improves rendering speed)
  Warning: be aware of specifity problems is using ids for styling (note: this breaks principles of OO CSS)

- CSS Background Images are rendered quicker than Image Tags, due to how the rendering / compositing cycle handles the request

- Understand cascading and selector specificity so you can write very terse
and effective code.

- Write selectors that are optimized for speed. Where possible, avoid
expensive CSS selectors. e.g. the * wildcard selector.

- Don't 'type' ID selectors (e.g. div#myid) or class selectors (e.g.table.results) .

This can also reduce extensibility.
- Don't use a z-index over 50, Assistive Technologies (AT) place links at this
level.

- Avoid !important, it's an anti-pattern, be more specific and use the cascade
if you have to.

##CSS Shorthand
As a general rule shorthand properties are preferred because of their terseness.
Don't forget to shorten colours: #ff0000; is #f00;
￼
##Images

- Use CSS sprites judiciously. They make hover states easy, improve page load times, but at a maintenance cost. Also horizontal sprites compress slightly better.

- Typically, all images should be sliced with a transparent background (PNG8). All should be cropped tightly to the image boundaries.

- Use a PNG squeezer. PNGs contain a fair amount of comments which add to file size. See http://www.smushit.com/ysmush.it/

- Improve Jpeg compression in photographic images by selectively blurring backgrounds. This technique creates more areas of redundancy for the Discrete Cosine Transformation (DCT) to compress images.
￼
- Donʼt use Conditional Comments to load CSS for Internet Explorer Browser hacks. It will have a significant impact on Front End Performance.
￼
###Warning

- The perceptual algorithm in Jpegs prioritises Lumninance (Brightness) over Chrominance (Colour). In short low colour images compress badly.
￼
- Jpegs do and will colour shift if left unattended.
￼
- Improve gif compression by reducing the colour palette down to only the most relevant colours required for each image.
￼ Note: Dithering images or interlacing will increases file size

##File Formats

- PNG: Can be compressed up 50% (as a generalisation) with the right tools, includes Gamma (colour) correction, lossless compression and supports transparency.

- JPEG: Can be compressed, does not include Gamma information (colours shift once saved), lossy compression (artifacts appear depending on compression scheme used) and does not support transparency.

- GIF: Can be compressed, does not colour shift, can have fixed 256 or lower palette, lossless compression, supports animation and one colour transparency.

##Visual Effects (Mobile HTML5 focussed - also relevant to Desktop)

- Don't apply multiple alpha transparencies to the same visual element, this increases the load on the renderer (can be a significant problem in Mobile HTML5 development)

- Gradients and transparency should not be applied to areas that move or scroll. In Android versions below 4.0 the browser renderer has to calculate the colour values of pixels each time an area is scrolled that contains overlays with gradients or transparency. In contrast iOS uses optimisations, for example converting the scroll area to a texture (bitmap) to mitigate the need to re-calculations

Note: Android 4.0+ now incorporates rendering performance optimisations like the above.

Note: CSS3 effects like gradients, shadows, borders (strokes), although recommendation by some Mobile HTML5 practitioners slow rendering speeds on mobile device because the CPU does ʻmore workʼ when rendering. It is better to ʻbake inʼ these effects into a bitmap graphic.

##Positioned Elements
Fixed position elements, like menus or tab controls 'pinned'; to the lower section of the screen should be used with caution in Mobile Web Apps.

Typically this effect is achieved using JavaScript because Webkit only supports ʻposition: fixedʼ in iOS 5.0+ or when used in a Hybrid App, native controls are used.

Reference: http://remysharp.com/2012/05/24/issues-with-position-fixed-scrolling-on-ios/
Independent design elements should be kept to a minimum where possible. Design elements typically translate to HTML tags, which in turn become DOM Nodes. To improve rendering performance in Android OS reduce the number of DOM Nodes (independent design elements) needed.

##Image Optimisation Tools

- Image Alpha http://pornel.net/imagealpha/

- Smush.it http://www.smushit.com/ysmush.it/

- YUI Compressor


##JavaScript

- Var all variables, un Vared variables become Global and reduce Front End Performance. 
  Locally scroped variables are Garbaged Collected (GC) more efficiently.

- Make use of short namespaces. Variables are referenced by looping through the different members of the scope chain. 

- Implement Life Cycle methods in components (e.g. destroy) in order to release memory, in preparation for the GC.

- If animation timers or polling is required, make use of window.requestAnimationFrame (with vendor prefix support and a fallback)

- Use a script library to load JavaScript or the standard Non UI Blocking technique (which dynamically creates a Script tag).
  The browser will better parallelise the download in this instance, preventing the script from blocking the UI render thread.

- DOM nodes with ids are resolved quicker than classes (even when using querySelectorAll - which is not as performant as you may think)

- For processor intensive operations, consider delegating work to a SetTimeout with a 0 or short millisecond parameter (acting like a 'Pseudo Green Thread'). This will reduce blocking within the main UI thread.  HTML5 Webworkers are also useful in this regard, carefully monitor CPU load with both solutions.

Tip: IDs are implicitly made available as globals in JavaScript (no need for document.getElementById : be careful how you use it however)

- The new keyword is expensive

- jQuery Functions that trigger Reflows
  https://gist.github.com/desandro/4657744

- Don't use for in to loop over Arrays

- Optimise for loops by looping backwards (if index is not important)

var itemsLen = items.length; // cache length - prevents evaluation on each loop

for(i=itemsLen; i--;)  { // looping backwards is faster
	//note: loop works in reverse
}

Tip: If performance is critical, don't forget you can unroll loops

- Don't concatenate with + use .concat(), it's quicker and consumes less memory.  Strings are immutable, concatenation creates new references.

- create quick, performant, easy to read JS Templates via:

var tmpl = ''.concat(
  '<div>',
    '<span>foo<span>',
  '</div>'
);

or

var tmpl = [
  '<div>',
    '<span>foo<span>',
  '</div>'
].join('');

- jQuery selector caching

var foo = $(".item").bind("click", function(){
	
	foo.not(this).addClass("bar");
		.removeClass("foobar");
		.fadeOut(500);

});


##Principals of Performance Testing Heuristics (SCORN):

- SIZE
  Media Compression
  Object/Code Duplication
  Code Minification
  Script/Style Abstraction

- CACHING
  Expires
  ETags
  Other Controls

- Order
  Styles
  Critical Content (what the user wishes to see)
  Relevant Media (graphics related to critical content)
  Other assets (non critical assets)
  Scripts
 
- Response Codes*
  3xx Redirection
  4xx Client Error
  5xx Server Error
  *Incorrect requests
  *Redundant requests
  *Server errors
  *Invalid URLs
  Reference: http://en.wikipedia.org/wiki/List_of_HTTP_status_codes

- Number (ask questions related to)
  number of requests
  large assets vs small assets  
  inline vs external scripts / css

##Further Reading

###The Runtime Performance Checklist
http://calendar.perfplanet.com/2013/the-runtime-performance-checklist/

###Best Practices for Speeding Up Your Web Site
http://developer.yahoo.com/performance/rules.htm

###Repaint and Reflow
http://dev.opera.com/articles/view/efficient-javascript/?page=3#reflow
http://www.stubbornella.org/content/2009/03/27/reflows-repaints-css-performance-making-your-javascript-slow

###YSlow Episodes
http://stevesouders.com/episodes

###Google Page Speed
http://code.google.com/speed/page-speed

###Web Performance Best Practices
http://code.google.com/speed/page-speed/docs/rules_intro.htm

###Optimise for Mobile
http://code.google.com/speed/page-speed/docs/mobile.htm

###Performance Research, Part 4: Maximizing Parallel Downloads in the Carpool Lane
http://www.yuiblog.com/blog/2007/04/11/performance-research-part-4

###GPU Accelerated Compositing in Chrome
http://www.chromium.org/developers/design-documents/gpu-accelerated-compositing-in-chrome

###Best Practices for Speeding Up Your Web Site (highly recommend)
http://developer.yahoo.com/performance/rules.html

###10 Javascript Performance Boosting Tips from Nicholas Zakas
http://jonraasch.com/blog/10-javascript-performance-boosting-tips-from-nicholas-zakas

###The case against script loaders
http://www.website-performance.co.uk/performance-metrics/the-case-against-script-loaders

###Beginners guide to AMD and RequireJS
http://www.integralist.co.uk/posts/beginners-guide-to-amd-and-requirejs/

###Frontend SPOF 
http://www.stevesouders.com/blog/2010/06/01/frontend-spof/

###Foreward to O’Reilly’s High Performance Web Sites Book by Steve Souders
http://nate.koechley.com/blog/2008/03/19/foreward-to-oreillys-high-performance-web-sites-book-by-steve-souders/

###Five Steps to Establish Software Performance Engineering in Your Organization
http://www.perfeng.com/papers/establish5.pdf

What is a non-blocking script?
http://www.nczonline.net/blog/2010/08/10/what-is-a-non-blocking-script/

Google Analytics (Asynchronous example)
http://googlecode.blogspot.com.au/2009/12/google-analytics-launches-asynchronous.html

###Jpeg Optimisations
http://www.smashingmagazine.com/2009/07/01/clever-jpeg-optimization-techniques/

###Png Optimisations
http://www.smashingmagazine.com/2009/07/15/clever-png-optimization-techniques/

###Jpeg 2000
http://www.codinghorror.com/blog/2007/02/beyond-jpeg.html

Critical Render Path 


Browsers make requests to a server, and receives an HTML file.

Once it receives the HTML file, it starts constructing the DOM. 

As the browser parses the HTML, it incrementally generates a tree model of HTML 
tags.

Parsing pauses when CSS links are encountered; the resource is downloaded, and  then 
parsing resumes.

Once all CSS is downloaded, the CSS Object Model is created.

Parsing is paused when JavaScript is encountered; the JavaScript file is read 
by the browser, and executes any changes that it may want onto the DOM and the 
CSSOM.

Afterwards, the browser combines the DOM and the CSSOM into a render tree.

The browser now knows exactly what should be rendered on a page.

Now, we have a rendered page!

** Images are not part of this process; they are just downloaded in the background, and 
appear on the screen once loaded.


The above process describes the Critical Render Path. It can be optimized!!


Optimizing the Critical Render Path 


- load CSS files as soon as possible (in the <head>)

- load JavaScript files as late as possible (since DOM and CSSOM are re-written
by JavaScript, and since JavaScript requires HTML and CSS parsing to be done)


For example, putting a JavaScript link in the head tag of an HTML document blocks 
page rendering.

By placing them at the bottom, style content and media could be loaded more quickly,
giving the perception of increased performance.


Optimizing CSS in the Critical Render Path 

- the render tree waits for the CSSOM to complete, since it needs to be combined with the 
DOM 

- CSS is render blocking, so it should be as lightweight as possible 


Above the Fold Loading 

- only load content that needs to be visible to the user 

- in the case of a scrollable page, only load content (including CSS) as the page is 
scrolled


Above the Fold Loading Example 


Two HTML elements:

<h2 class="important"

    Important 

...  

lower in the page:

<h2 class="second">

    Not important // We can have a condition to load this later

...


Two CSS Links:

<link rel="stylesheet" href="style.css"...
<link rel="stylesheet" href="style3.css"...


With the following code, the 2 above imports are no longer needed:

<script type="text/javascript>

    const loadStyleSheet = src => {

        if (document.createStyleSheet) {

            document.createStyleSheet(src)
        } else {

            const stylesheet = document.createElement('link')
            stylesheet.href = src; 
            stylesheet.type = 'text/css'
            stylesheet.rel = 'stylesheet'

            // Insert the stylesheet into the <head>
            document.getElementsByTagName('head')[0].appendChild(stylesheet)

        }
    }

    /* Load style3.css once all of the objects in the document are in the DOM,
    and all the images, scripts, links and sub-frames have finished loading. */
    window.onload = function() {

        loadStyleSheet('./style3.css')
    }

</script>


Media Attributes

media="all" is default.

We can specify media in stylesheet links:

<link rel="stylesheet" href="./style2.css" media="only screen and (min-width:500px)>


Less Specificity 

Don't be overly specific with CSS (it makes the browser do more calculations).

Bad:

.header .nav .item .link a.important {

    color: pink;
}

Good: 

a.important {

    color: pink;
}


Internal and inline CSS are good for single page applications. It isn't good for multi 
page applications, since pages can't share CSS (to share, we'd have to use the same 
imported stylesheet).


JavaScript Optimizations

When a script tag is found during HTML parsing, DOM construction is paused, 
and the script is requested from the server.

After the CSSOM is constructed, JavaScript is executed, and can alter the DOM and 
CSSOM.

Thus, JavaScript is parser blocking. There are ways to optimize scripts.


Loading Scripts Asynchronously 

2 tags can alter the pausing of parsing to retrieve a script.


<script async> 

- tells the browser to download the script on another thread (doesn't block the 
rest of the page from loading)

- as soon as it finishes downloading, it executes 

- executing still blocks parsing 

- ony use for scripts which don't affect the DOM of CSSOM (eg. use it for 
Google Analytics scripts or tracking scripts)


<script defer>

- similar to async in that it will not block the loading of the page 

- however, it will wait to execute until after all has been parsed, and will 
execute in order of appearance 


** Generally, if core functionality requires JavaScript, use async. If not, it should be deferred outside
of the critical render path.


<script>

- critical scripts 

<script async>

- third party scripts which don't affect the DOM 

<script defer>

- unimportant, below the fold third party scripts 


Avoid render blocking and parser blocking JavaScript. Setting style through JavaScript like this makes the 
browser do extra work to render:

<script>

    var btn = document.querySelector("button")
    btn.style.color = "red"


Critical Render Path Demonstration


1) document.addEventListener("DOMContentLoaded", function(event)) {

    console.log("DOM loaded and parsed")
})


2) window.addEventListener("load", function(event) {

    console.log("All resources finished loading")
})


3) console.log("script1")


1) runs first, 2) runs second, and 3) runs third

-> this is because the DOM build tree is finished after 1), and resources such as images are loaded after 2).


Avoid Long Running JavaScript 

const button = docuement.querySelector("button");

button.addEventListener("click", function() {

    alert("Click Here"); // This will produce an annoying alert that prevents users from doing anything else
})


The Critical Render Path 


                        DOMContentLoaded                                        Load 
                            |                                                     |
                            v                                                     v
        DOM   ->   CSSOM   ->   Render Tree (5)   ->   Layout (6)   ->   Paint (7)
                     ^  -> JS (4)       ^
                     |                    \
                     |                     \
                     |                      \
HTML (1) -> CSS (2) JS (3)                  JS (8) (JavaScript updates the render tree as the user uses the app, so
                                                DOM manipulation should be minimized)


- it's best to have scripts just before </body> (the end of the body tag)

- use PageSpeed Insights (now LightHouse) and WebPageTest


Resource Prefetching

- can be used to tell the browser which assets the user might need in the future, before they need 
them 

- it's a way of hinting to the browser about resources that are definitely going to or might be used 
in the future

- the practice of guessing what users need before they need it is prebrowsing, which breaks into:

1) dns-prefetch 

2) prefetch(standard)

3) preconnect 

4) prerender 

You can read more about them here: https://css-tricks.com/prefetching-preloading-prebrowsing/


DNS Prefetching 

- notifies the client that there are assets we'll need later from a specific URL so the browser 
can resolve the DNS as quickly as possible 

Example

- if we need a resource, like an image or an audio file from example.com, we'd write the following in the 
head of the document 

<link rel="dns-prefetch" href="https://example.com"

- then, when we request a file from it, we'll no longer have to wait for the DNS lookup

- this is useful if we're using code from third parties or resources from social networks where we might 
be loading a widget from a <script>


PreConnect 

- will resolve the DNS, but will also make the TCP handshake, and an optional TLS negotiation

<link rel="preconnect" href="https://css-tricks.com"

- ships in Firefox 39 and Chrome 46


Prefetching 

- if we're certain a specific resource will be requried in the future, we can ask the browser to request 
that item and store it in the cache 

- for example, an image or script or anything cacheable by the browser 

<link rel+"prefetch" href="image.png"

- unlike DNS prefetching, we're actually requesting and downloading that asset and storing it in 
the cache 

- usually, font files have to wait for the DOM and the CSSOM to be constructed before they download, but 
if we prefetch them, that bottleneck can be circumvented


Subresources

- helps identify the resources that are the highest priority and should be requested before prefetched items 

- for example, in Chrome and Opera, the following can be added to the head of the document 

<link rel="subresource" href="styles.css">

- this enables the early loading of resources within the current page 


Prerendering 

- gives us the ability to preemptively load all assets of a certain document 

<link rel="prerender" href="https://css-tricks.com"

- all resources will be downloaded, the DOM is created, JavaScript is executed etc, so it's best to 
be sure that the user will actually click the link!


HTTP/2 

https://developers.google.com/web/fundamentals/performance/http2

- a protocol update 

- goal is to improve netowrk latency

- uses multiplexing, eg. can have parallelism for a single connection

- due to optimizations, it may not be substantially beneficial to combine files


Sample Node Server using HTTP/2 


const http2 = require('http2')
const fs = require('fs')

const options = {

    key: fs.readFileSync('./selfSigned.key'),
    cert: fs.readFileSync('./selfSigned.crt'),
    allowHTTP1: true
}

const server = http2.createSequenceServer(options, (req, res) => {

    res.setHeader('Content-Type', 'text/html');
    res.send('ok')
})


This enables full request and response multiplexing, minimizes protocol overhead via efficient compression
of HTTP header fields, and adds support for request prioritization and server push.

** In telecommunications and computer networks, multiplexing is a method by which multiple analog
or digital signals are combined into one signal over a shared medium. The aim is to share a scarce resource.
For example, in telecommunications, several telephone calls may be carried using one wire.


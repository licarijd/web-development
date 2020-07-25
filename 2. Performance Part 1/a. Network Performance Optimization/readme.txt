Network Performance Optimization


Minify JavaScript (to discuss in later sections)

UglifyJS 

- removes whitespace and minifies JavaScript

- similar tools can be used for CSS and HTML 


Minimize Text Files 

- HTML 

- CSS 

- Images 


Minimize Images 

Image File Formats


JPG 

- usually used for photos and images with many colours 

- don't allow for transparency 

- large in file size 


GIFS 

- limited colous, usually 2-256

- good for animations 


PNG

- limited colours 

- much smaller in size than jpg 

- often used for logos 

- supports transparency


SVG

- XML based, very flexible 

- uses vector graphics 

- can rescale without any quality loss 

- very small 

- great for 4K or retina displays 

- can be customized with CSS 

- typically simple, with few colours 


New image types, such as .webp are faster than all of these, but lack browser support (2019).


If you want transparency:

- use PNG 

If you want animations:

- use GIF 

If you want colourful images:

- use a JPG 

If you want simple icons, images, logos, and illustrations:

- use SVGs


Image Optimizations and Tools

JPGs can be reduced with tools like http://jpeg-optimizer.com/

On websites, always lower JPEG image qulaity (30 - 60%).

Resize images based on the size they will be dislayed (if CSS specifies an image will be 50 pixels by 50 
pixels, make sure the actual image is this size so you're not needlessly using large images).

Display different sized images for different backgrounds (use media queries, like mixins to detect 
breakpoints on Listen).

Use CDNs (Content Delivery Networks) like imigx.

Remove image metadata


** Media Queries


Media Queries are useful when you want to apply CSS styles depending on a 
device general type, such as screen size.

Example:

@media screen and (min-width: 900px) {

    body {

        background: url(...)
    }
}

@media screen and (max-width: 500px) {

    body {

        background: url(...)
    }
}


** ImageX 

- allows you to upload image files 

- optimizes images, and makes sure they're as small as possible

- gives a URL you can use in your CSS 

- uses CDNs, which allows faster access through their servers, rather than just uploading
it ourselves


Image tags and metadata can be removed with http://www.verexif.com/


Delivery Optimizations 

- less resources on a page means less HTTP requests and a faster site

- if you're using Bootstrap or Foundation, or anything else to help with your UI
and adds new CSS files, make sure it is necessary (Flexbox and CSS grids
are both suerb native alternatives)

- JavaScript libraries, such as jQuery, are not always necessary, and often very large 

- "querySelector", "querySelectorAll" are alternatives to using jQuery 

- event binding is simple with "addEventListener", "classList", "setAttributes", etc
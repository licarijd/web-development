Single Page Applications vs. Server Side Rendering 


Single Page Application

- update only the parts of the page that you need 

- don't have to request each page like in Server Side Rendering 

- the downside is that a lot of JavaScript is downloaded in the first request 

- another downside is SEO, and web crawlers can't find content 


Server Side Rendering 

- faster initial load 


Server Side Rendering with React 

- see AllKenora repo

- requires React on the client and server 


ReactDOM.hydrate() 

- like calling render on components

- when this is called on a component which has server rendered markup, the event handlers will be attached 
so that the component will be usable and interactive

- renderToNodeStream is faster than renderToString 

** NextJS makes SSR in React much easier!!


Next.js 

- every page in "pages directly corresponds with a route (see allKenora repo)

https://nextjs.org/


Client Side Routing 

- when a route is handled internally with JavaScript that's loaded on the page (no server rendering)

- don't have to refresh the entire page (unlike SSR and page links with <a>)


GetInitialProps API 

- allows for dynamic apps with NextJS 

https://nextjs.org/docs/api-reference/data-fetching/getInitialProps


Fetching Server Side 

- NextJS page code is server side, and thus needs an isomorphic fetch (a fetch that also works server side as well  
as client side)

https://www.npmjs.com/package/isomorphic-fetch


Now for Next.js 

Realtime global deployments (on Next.js site)


Progressive Randering 

- send out only what you need for the initial render 

- then, progessively load features/pages in the background as more things arrive 


Prerender.io 

- allows JavaScript websites to be crawled by search engine 

https://prerender.io/
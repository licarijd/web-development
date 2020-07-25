Performance Part 2


Code Splitting 

- orignally, JavaScript files bundled into one file, and then sent to the browser 

- a downside is that certain parts of the code are only relevant to certain pages 

- code splitting involves splitting JavaScript into chunks that don't slow the main thread;
we load the resources for one page at a time (only loading the resources we need for the page)


Route Based Chunking

- code split by route (page)


Component Based Chunking 

- code split by component 


Read React Code Splitting docs! There are better alternatives like React Router and Loadable Components:
https://reactjs.org/docs/code-splitting.html


Performance in React 

Use React dev tools in Chrome to use the profiler and monitor components; you can see cool stuff like re-renders 
to help debug components.

Re-rendering a component will affect all of it's children.

- try to avoid large trees/heriarchies 

- move nested components that don't need re-rendering out of nested positions


Track Unecessary Re-Renders (see why and when a component updates)

https://github.com/welldone-software/why-did-you-render


Tree Shaking 

- removes unused code 

- done with Webpack automatically


Load Testing 

- simulates client requests to a server to see how your server handles a large number of 
requests 

https://locust.io/
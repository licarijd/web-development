React, Redux, and Module Bundling 


React Notes 

npm run build 

- creates an optimized build 

- minified files 

- everything needed to deploy a site 


Improving the Critical Render Path 


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


React helps with the cycle of 6, 7, and 8 (DOM manipulation after the site loads).

Webpack helps with 3 (JavaScript delivery).

Redux helps improve React for 6, 7, and 8.


State Management 

- React components uses state 

    state = { ... }

- state gets complicated with larger apps

- can be improved with Redux 


Redux 

- remove this.state from all Components 

- just use props 

- keep state in an application store 

- pass the state as props to whichever components need it 


3 Concepts 

- single source of truth (one large object for state)

- state is read-only (create a new state after each action is fired)

- changes using pure functions (a pure function always takes an input and returns a predictable 
output)


Redux Concepts 


Action 

- something a user does 


Reducer 

- receives an action, produces an output

- output is the store (state)

** reducers implicitly receive fired actions, we don't call reducers explicitly


All actions go to one Reducer. Redux uses the Flux architectural pattern:

Action -> Dispatcher -> Store -> View (one way data flow)


Sophisticated Logging Middleware

- Redux DevTools


Popular Tools for React and Redux 


React Router 

- for routing (changing pages and URLs)


Utility Libraries (Extra JavaScript tools)


Ramda

- extra functional programming capabilities: https://ramdajs.com/

Lodash

- A modern JavaScript utility library delivering modularity, performance, & extras: https://lodash.com/


Styling 

Glamorous: https://glamorous.rocks/

Styled Components: https://styled-components.com/

CSS Modules: https://github.com/css-modules/css-modules


Gatsby.js 

- good for static React sites 


Next.js 

- for server rendered (statically pre-rendered) React apps


React Components 

Material UI: https://material-ui.com/

Semantic UI: https://semantic-ui.com/


Redux Improvements

Reselect: https://github.com/reduxjs/reselect

Redux-Saga (for async actions): https://redux-saga.js.org/

Immutable (makes sure state remains immutable) - https://redux.js.org/recipes/using-immutablejs-with-redux


Webpack 

- used to bundle files into static assets 

- takes input files 

- outputs to a folder (eg. 'build')

- has an entry point (eg. index.js)


  js ------------
  |               \
  v                \
  hbs                ----> loaders, plugins ----> .js, .css, .jpg, .png 
  |                 /
  v                /
  cjs --> sass ---
  ...


loaders 

- transpiles code (eg. ES6 to ES5 using Babel)


Parcel 

- new bundler 

- less complicated configuration than Webpack 

"scripts": {

    "start" : "parcel index.html"
}





Testing 


Test Driven Development 

- write tests before code (start with failing tests)

- code is written based on tests 


Unit Tests 

- tests individual functions and classes 


Integration Tests 

- tests how different pieces of code interact with each other 


Automation Tests 

- real life UI based tests 

- can be humans or machines 


Top 3 Testing Libraries 

- Jest 

- Jasmine 

- Mocha


Assertion Libraries 

- Jasmine 

- Jest

- Chai (usually paired with Mocha)

- karma.js 


Mocks, Spies and Stubs (provide information about functions)

- Jasmine 

- Jest 

- Sinon.js 

Spies provide info about a function, like calling info.

Stubbing replaces selected functions with a function to ensure the expected behaviour 
happens.

Mocks fake a behaviour to test different parts of a process (good for integration tests).


Stub Example with Sinon.js 

var callback = sinon.stub() 

callback.withArgs(42).returns(1)

- this fakes a function and it's expected output with a given input 


Code Coverage Tools 

- SonarQube (also tests code complexity, bugs, and code smells!)

- Istanbul


** Create React App contains Jest out of the box!! 


Ava - good for parallelism 


** Unit Testing is better with Pure Functions (no side effects, and determintic - 
same input yields the same output every time).

** Use mocks, spies, and stubs for integration tests.

** It's easy to run tests in JSDOM: https://corinneling.com/post/frontend-testing-with-jsdom/
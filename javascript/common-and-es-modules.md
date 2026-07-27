# Common JS Modules VS ES6 Modules for Node.js

## Why Modules were introduced?
- Early websites need just one simple file of javaScript code to enable interactivity wherever needed in the website but as websites evolve to web application they created a need for reusable code when javaScript is written in multiple files. 
- So, for to reduce code duplication and increase the reusability of code, modules are introduced.

## Common JS Modules
- It was introduced to use modules in the Server Side Environments like Node.js and it doesn't support in browser or client side.
- To import modules the ``require()`` is used and for to export the modules ``module.exports`` is used.
- It is flexible, we can import a module any where in the code like in a variable, function, loop, or condition using ``require()`.
- It loads the modules synchronously like one by one and it blocks loading a file next to a file the flow of loading modules can be easily found. It may cause performance issues while loading modules which contains I/O operations, long computaions, etc and the system may become freeze or unresponsive.
- As earlier Node.js versions supports only Common JS modules some Node.js libraries and modules are used Common JS modules.

## ES 6 Modules
- It is introduced to solve the perfomance issues in Common JS modules and also to support the modules usage in browsers.
- To import modules the ``import add from './add'`` statement is used and for to export module ``export add`` statement is used.
- We can import a module in top of the file only we cannot use ``import`` like Common JS modules.
- It loads the modules asynchronously it is non-blocking but the it is harder to debug because the flow of loading modules cannot be determined easily. It provides a better user experience compared to Common JS modules.

## When to use which one?
- Use Common JS modules if you are building a small scale server side web applications those doesn't need high performance or have unpredictable execution of modules.
- Use ES6 modules if you are building a large scale application that need higher perfomance and also having unpredicatable execution of modules.

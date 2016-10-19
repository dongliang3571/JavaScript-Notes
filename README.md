# JavaScript-Notes

## Strict mode
Strict mode makes several changes to normal JavaScript semantics.

  - It eliminates some JavaScript silent errors by changing them to throw errors.
  - It fixes mistakes that make it difficult for JavaScript engines to perform optimizations: strict mode code can sometimes be made to run faster than identical code that's not strict mode.
  - It prohibits some syntax likely to be defined in future versions of ECMAScript.

  ```javascript
  // To invoke strict mode for an entire script, put the exact statement "use strict";
  // (or 'use strict';) before any other statements.

  "use strict";
  var v = "Hi!  I'm a strict mode script!"; // Now this line of code is in strict mode
  ```

## Import and Export
- Export
  ```javascript
  ////////////////// Method 1 //////////////////////
  // File name newModule.js
  function myPrint() {
    console.log("myPrint");
  }
  
  function anotherPrint() {
    console.log("anotherPrint");
  }
  
  module.exports.my = myPrint;  // Just function name, not calling the funciton(no parentheses)
  module.exports.another = anotherPrint;  // Just function name, not calling the funciton(no parentheses)
  
  ////////////////// Method 2 //////////////////////
  // Better way to do just function exporting
  // Write function definition inside module.exports(it's an object after all)
  
  // File name newModule.js
  module.exports = {
    myPrint: function() {
      console.log("myPrint");
    },
    anotherPrint: function() {
      console.log("anotherPrint");
    }
  };
  ```
  
  
- Import
  ```javascript
  ////////////////////////// import custom module ////////////////////////
  
  ////////////////// Method 1 //////////////////////
  var newModule = require('./newModule');  // File name without suffix .js
  newModule.my(); // prints "myPrint"
  newModule.anohter();  // prints "anotherPrint"
  
  ////////////////// Method 2 //////////////////////
  var newModule = require('./newModule');  // File name without suffix .js
  newModule.myPrint(); // prints "myPrint"
  newModule.anohterPrint();  // prints "anotherPrint"
  
  
  ///////////////// import core module(built-in module) ///////////////////
  var coreModule = require('express');  // No suffix .js
  ```
  
  
# JavaScript Template Engine

## Handlebars

### Variables
  ```javascript 
  // HTML
  <h1>{{title}}</h1>
  <p>{{body}}</p>
  
  // JavaScript
  {
    title: "Express.js Guide",
    body: "The Comprehensive Book on Express.js"
  }
  
  // renders:
  <h1>Express.js Guide</h1>
  <p>The Comprehensive Book on Express.js</p>
  ```
 
### Iteration(each)

Inside the block, we can use @key for the former (objects), and @index for the later (arrays).

  ```javascript 
  // HTML
  <div>
    {{#each languages}}
      <p>{{@index}}. {{this}}</p>
    {{/each}}
  </div>
  
  // JavaScript
  {languages: ['php', 'node', 'ruby']}
  
  // renders:
  <div>
    <p>0. php</p>
    <p>1. node</p>
    <p>2. ruby</p>
  </div>
  
  ```
  
 ### Unescaped Output
 
 By default, Handlebars escapes values. If you don’t want Handlebars to escape a value, use triple curly braces: {{{ and }}}.

  

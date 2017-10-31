# JavaScript-Notes

THis guy is good http://ryanmorr.com/

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
  
  ```javascript
  "use strict";
  x = 3.14;       // This will cause an error because x is not declared
  ```
  
  ```javascript
  "use strict";
  myFunction();

  function myFunction() {
      y = 3.14;   // This will also cause an error because y is not declared
  }
  ```
  
## Context vs. Scope

[Understanding Scope and Context in JavaScript](http://ryanmorr.com/understanding-scope-and-context-in-javascript/)

The first important thing to clear up is that context and scope are not the same. I have noticed many developers over the years often confuse the two terms (myself included), incorrectly describing one for the other. To be fair, the terminology has become quite muddled over the years.

Every function invocation has both a scope and a context associated with it. Fundamentally, scope is function-based while context is object-based. In other words, scope pertains to the variable access of a function when it is invoked and is unique to each invocation. Context is always the value of the `this` keyword which is a reference to the object that "owns" the currently executing code.

```javascript
// define a function
var f = function(){console.log(this)};

f(); //calling the function, it prints window(global) object
new f(); // create a new object, it prints f object
```

### Closures

```javascript
function foo() {
    var localVariable = 'private variable';
    return function() {
        return localVariable;
    }
}

var getLocalVariable = foo();
getLocalVariable() // "private variable"
```

Accessing variables outside of the immediate lexical scope creates a closure. In other words, a closure is formed when a nested function is defined inside of another function, allowing access to the outer functions variables. Returning the nested function allows you to maintain access to the local variables, arguments, and inner function declarations of its outer function. This encapsulation allows us to hide and preserve the execution context from outside scopes while exposing a public interface and thus is subject to further manipulation. A simple example of this looks like the following:

## Promises and callbacks
callback function can be either asynchronous or synchronous
  - synchronous
  
  For example, the .forEach function takes a callback but is synchronous.
  
  ```javascript
  var available = false;
  [1,2,3].forEach( function(){
    available = true;
  });
  //code here runs after the whole .forEach has run,
  //so available === true here
  ```

  - asynchronous
  
  The setTimeout takes a callback too and is asynchronous.
  
  ```javascript
  function myFunction( fn ) {
    setTimeout( function() {
        fn(1,2,3);
    }, 0 );
  }

  var available = false;
  myFunction( function() {
    available = true;
  });
  console.log(available); // print false because it won't wait setTimeout to complete, avaiable was false initially
  ```

### Usage of callbacks

  ```javascript
  Something.save(function(err) {  
    if (err)  {
      //error handling
      return;
    }
    console.log('success');
  });
  ```
  
### Write functions with callback

  ```javascript
  function save(callback) {
    var success = doSomethingToSaveData();
    if (success) {
      var err = new Error();
    } else {
      err = null;
    }
    callback(err);
  }
  ```

### Usage of promise

  ```javacript
  // Promises Example
  saveSomething()  
    .then(updateOtherthing)
    .then(deleteStuff)  
    .then(logResults);
  ```

### Write promise
What you need to do is wrapping the callback the original function call with a Promise, like this:

  ```javacript
  function saveToTheDb(value) {  
    return new Promise(function(resolve, reject) {
      db.values.insert(value, function(err, user) { // remember error first ;)
        if (err) {
          return reject(err); // don't forget to return here
        }
        resolve(user);
      })
    }
  }
  ```

 
### Support both callback and promise

  ```javascript
  function foo(cb) {  
    if (cb) {
      return cb();
    }
    return new Promise(function (resolve, reject) {
    });
  }
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
 
## Objects

Access object attributes by dot(.) notation

```javascript
var myCar = new Object();
myCar.make = "Ford";
myCar.model = "Mustang";
myCar.year = 1969;
```

Access object attributes using a bracket notation

```javascript
myCar["make"] = "Ford";
myCar["model"] = "Mustang";
myCar["year"] = 1969;
```

You can add any attribute to a object on the fly

```javascript
function Person(first, last, age, eyecolor) {
    this.firstName = first;
    this.lastName = last;
    this.age = age;
    this.eyeColor = eyecolor;
}

var myFather = new Person("John", "Doe", 50, "blue");
myFather.nationality = "English"; //The property will be added to myFather object

////// This won't work //////
Person.nationality = "English";

// either you can do:
function Person(first, last, age, eyecolor) {
    this.firstName = first;
    this.lastName = last;
    this.age = age;
    this.eyeColor = eyecolor;
    this.nationality = "English"
}

// or you can do:
Person.prototype.nationality = "English";

```

An object property name can be any valid JavaScript string, or anything that can be converted to a string, including the empty string. However, any property name that is not a valid JavaScript identifier (for example, a property name that has a space or a hyphen, or that starts with a number) can only be accessed using the square bracket notation.

```javascript
var myObj = new Object(),
    str = "myString",
    rand = Math.random(),
    obj = new Object();

myObj.type              = "Dot syntax";
myObj["date created"]   = "String with space";
myObj[str]              = "String value";
myObj[rand]             = "Random Number";
myObj[obj]              = "Object";
myObj[""]               = "Even an empty string";

```

### List all property for an Object

```javascript
var arr = [1, 2, 3];
Object.getOwnPropertyNames(arr)
//  ["0", "1", "2", "length"]
```

### Get type of the object

```javascript

Object.prototype.toString.call([1, 2])
//  [object Array]
```

Use object method shorthand. eslint: `object-shorthand` jscs: `requireEnhancedObjectLiterals`

```javascript
// bad
const atom = {
  value: 1,

  addValue: function (value) {
    return atom.value + value;
  },
};

// good
const atom = {
  value: 1,

  addValue(value) {
    return atom.value + value;
  },
};
 
```

Use property value shorthand. eslint: `object-shorthand` jscs: `requireEnhancedObjectLiterals`

```javascript
//  Why? It is shorter to write and descriptive

const lukeSkywalker = 'Luke Skywalker';

// bad
const obj = {
  lukeSkywalker: lukeSkywalker,
};

// good
const obj = {
  lukeSkywalker,
};
```

## References
Use const for all of your references; avoid using var. eslint: `prefer-const`, `no-const-assign`

```javascript
//  Why? This ensures that you can't reassign your references, which can lead to bugs and difficult to comprehend code.

// bad
var a = 1;
var b = 2;

// good
const a = 1;
const b = 2;
```

If you must reassign references, use let instead of var. eslint: `no-var` jscs: `disallowVar`

```javascript
//  Why? let is block-scoped rather than function-scoped like var.

// bad
var count = 1;
if (true) {
  count += 1;
}

// good, use the let.
let count = 1;
if (true) {
  count += 1;
}
```

Note that both let and const are block-scoped.

```javascript
// const and let only exist in the blocks they are defined in.
{
  let a = 1;
  const b = 1;
}
console.log(a); // ReferenceError
console.log(b); // ReferenceError
```

## Array

### Some bult-in method

```javascript

// Slicing array
var arr = [1, 2, 3]
arr.slice();  // [1, 2, 3]
arr.slice(1);  // [2, 3]
arr.slice(1, 3);  // [2, 3]

// Determines whether the passed value is an Array
Array.isArray([1, 2, 3]); // true
```

### Some syntax

```javascript

// bad
const items = new Array();

// good
const items = [];

```

```javascript
const someStack = [];

// bad
someStack[someStack.length] = 'abracadabra';

// good
someStack.push('abracadabra');
```

Closure can omit return if body consists of a single statment

```javascript
// good
[1, 2, 3].map((x) => {
  const y = x + 1;
  return x * y;
});

// good
[1, 2, 3].map(x => x + 1);
```

# NodeJS

The main event loop is single-threaded by nature. But most of the i/o (network, disk, etc) is run on separate threads, because the i/o APIs in Node.js are asynchronous/non-blocking by design, in order to accommodate the event loop.

However, in some ways, calling Node asynchronous is a misnomer IMO, because almost all the code that the developer sees is in fact synchronous, not asynchronous. In other words, for the average program a Node.js developer writes, all the asynchrony is abstracted away inside the i/o related APIs.

Just because you don’t see it, doesn’t mean it’s not there or that it’s not important.

One of the biggest pluses about Node is the very fact that more often than not all the code the developer writes is running on one thread in an event loop – that’s the rub – you don’t have to deal with a multi-threaded environment because it’s abstracted away.

As long as you don’t do computationally heavy work on the main Javascript thread, then you are golden, because the slow I/O stuff is taken care of for you by the asynchronous non-blocking I/O libraries. If you do need to do computationally heavy work (CPU bound work), don’t do it on the main thread, instead fork a child_process and run it on a separate thread than the main Javascript event loop thread.

So yes, you can’t get away from multithreading, even in Node.js. However, with the Node.js event loop, you can truly avoid many of the pitfalls of more traditional multithreaded servers and programs.

Let me give you an analogy. Clientside Javascript has no traditional I/O. Node.js was created in the first place because Javascript had no existing i/o libraries so they could start clean with non-blocking i/o. For the client, I/O takes the form of AJAX requests. AJAX is by default asynchronous. If AJAX were synchronous then it would lock up the front-end. By extension, the same thing is true with Node.js. If I/O in Node was synchronous/blocking then it would lock up the event-loop. Instead asynchronous/non-blocking i/o APIs in Node allow Node to utilize background threads that do the I/O work behind the scenes, thus allowing the event loop to continue ticking around and around freely, about once every 20 milliseconds.

```javascript

// next() with no arguments says "just kidding, I don't actual want 
// to handle this". It goes back in and tries to find the next route 
// that would match.

// Example: request is taken by middleware first, if you called next() 
// in middleware, it will send the request to controller.

module.exports = function(app) {

    app.get('/hello', function(req, res) {
        res.send('look at me!');
    });

};

function isAuthenticated(req, res, next) {

    // do any checks you want to in here

    // CHECK THE USER STORED IN SESSION FOR A CUSTOM VARIABLE
    // you can do this however you want with whatever variables you set up
    if (req.user.authenticated)
        return next();

    // IF A USER ISN'T LOGGED IN, THEN REDIRECT THEM SOMEWHERE
    res.redirect('/');
}
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
 
 By default, Handlebars escapes values. Use triple curly braces: `{{{ and }}}` if you don’t want Handlebars to escape a value, 

  ```javascript
  // HTML
  <ul>
    {{#each arr}}
    <li>
      <span>{{@index}}</span>
      <span>unescaped: {{{this}}} vs. </span>
      <span>escaped: {{this}}</span>
    </li>
    {{/each}}
  </ul>
  
  // JavaScript
  {
    arr: [
      '<a>a</a>',
      '<i>italic</i>',
      '<strong>bold</strong>'
    ]
  }
  
  // render:
  <ul>
    <li>
      <span>0</span>
      <span>unescaped: <a>a</a> vs. </span> // This is unescaped, so that original value is shown
      <span>escaped: &lt;a&gt;a&lt;/a&gt;</span>  // This is escaped, so escaped value is shown
    </li>
    <li>
      <span>1</span>
      <span>unescaped: <i>italic</i> vs. </span>
      <span>escaped: &lt;i&gt;italic&lt;/i&gt;</span>
    </li>
    <li>
      <span>2</span>
      <span>unescaped: <strong>bold</strong> vs. </span>
      <span>escaped: &lt;strong&gt;bold&lt;/strong&gt;</span>
    </li>
    </ul>
  ```

### Conditions(if)

if is another built-in helper invoked via #. For example, this Handlebars code:

  ```javascript
  // HTML
  {{#if user.admin}}
    <button class="launch">Launch Spacecraft</button>
  {{else}}
    <button class="login"> Log in</button>
  {{/if}}
  
  // JavaScript
  {
    user: {
      admin: true
    }
  }
  
  // render:
  <button class="launch">Launch Spacecraft</button>
  ```
  
### Unless
To inverse an if not ... (if ! ...)statement (convert negative to positive), we can harness the unless built-in helper block. For example, the previous code snippet can be rewritten with unless.

  ```javascript
  // HTML
  {{#unless user.admin}}
    <button class="login"> Log in</button>
  {{else}}
    <button class="launch">Launch Spacecraft</button>
  {{/unless}}
  
  // JavaScript
  {
    user: {
      admin: true
    }
  }
  
  // render:
  <button class="launch">Launch Spacecraft</button>
  ```
  
### With
In case there’s an object with nested properties, and there are a lot of them, it’s possible to use `with` to pass the context.

  ```javascript
  // HTML
  {{#with user}}
  <p>{{name}}</p>
  {{#with contact}}
  <span>Twitter: @{{twitter}}</span>
  {{/with}}
  <span>Address: {{address.city}},
  {{/with}}
  {{user.address.state}}</span>
  
  // JavaScript
  {user: {
    contact: {
      email: 'hi@azat.co',
      twitter: 'azat_co'
    },
    address: {
      city: 'San Francisco',
      state: 'California'
    },
    name: 'Azat'
  }}
  
  // render:
  <p>Azat</p>
  <span>Twitter: @azat_co</span>
  <span>Address: San Francisco, California
  </span>
  ```
  
### Comments
To output comments, use regular HTML <!-- and -->. To hide comments in the final output, use {{! and }} or {{!-- and --}}. For example:

  ```javascript
  <!-- content goes here -->
  <p>Node.js is a non-blocking I/O for scalable apps.</p>
  {{! @todo change this to a class}}
  {{!-- add the example on {{#if}} --}}
  <p id="footer">Copyright 2014 Azat</p>
  
  // Output
  <!-- content goes here -->
  <p>Node.js is a non-blocking I/O for scalable apps.</p>
  <p id="footer">Copyright 2014 Azat</p>
  ```

### Custom Helpers

Custom Handlebars helpers are similar to built-in helper blocks and Jade mixins. To use custom helpers, we need to create them as a JavaScript function and register them with the Handlebars instance.
This Handlebars template uses our custom helper table which we’ll register (i.e., define) later in the JavaScript/Node.js code:

`{{table node}}`

Here goes the JavaScript/Node.js that tells the Handlebars compiler what to do when it encounters the custom table function (i.e., print an HTML table out of the provided array):

  ```javascript
  Handlebars.registerHelper('table', function(data) {
  var str = '<table>';
  for (var i = 0; i < data.length; i++ ) {
    str += '<tr>';
    for (var key in data[i]) {
      str += '<td>' + data[i][key] + '</td>';
    };
    str += '</tr>';
   };
   str += '</table>';

   return new Handlebars.SafeString (str);
  });
  
  
  // data
  
  {
    node:[
      {name: 'express', url: 'http://expressjs.com/'},
      {name: 'hapi', url: 'http://spumko.github.io/'},
      {name: 'compound', url: 'http://compoundjs.com/'},
      {name: 'derby', url: 'http://derbyjs.com/'}
    ]
  }
  
  // Output
  <table>
    <tr>
        <td>express</td>
        <td>http://expressjs.com/
    </td>
    </tr>
        <tr><td>hapi</td>
    <td>http://spumko.github.io/
    </td>
    </tr>
    <tr>
        <td>compound</td>
        <td>http://compoundjs.com/
    </td>
    </tr>
    <tr>
        <td>derby</td>
        <td>http://derbyjs.com/</td>
    </tr>
  </table>

  ```
  
### Includes (Partials)

Includes or partials templates in Handlebars are interpreted by the `{{>partial_name}}` expression. Partials are akin to helpers and are registered with `Handlebars.registerPartial(name, source)`, where name is a string and source is a Handlebars template code for the partial.


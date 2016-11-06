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


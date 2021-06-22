# Javscript

## Theory

### Q1: Can you name two programming paradigms important for JavaScript app developer?

```
JavaScript is a multi-paradigm language, supporting procedural, function & object oriented programming.

A multi-paradigm programming language is one that is equally well-suited in more than one programming paradigm. A programming paradigm is a style of programming like functional, OOP & procedural. Not many languages support all at a time.

Java, Python, C++, C# also claims to be multi-paradigm
```

### Q2: What is asynchronous programming, and why is it important in JavaScript?

```
Synchronous programming means that, barring conditionals and function calls, code is executed sequentially from top-to-bottom, blocking on long-running tasks such as network requests and disk I/O.

Asynchronous programming means that the engine runs in an event loop. When a blocking operation is needed, the request is started, and the code keeps running without blocking for the result. When the response is ready, an interrupt is fired, which causes an event handler to be run, where the control flow continues. In this way, a single program thread can handle many concurrent operations.

User interfaces are asynchronous by nature, and spend most of their time waiting for user input to interrupt the event loop and trigger event handlers.
```

### Q3: What are the pros and cons of monolithic vs microservice architectures?

```
A monolithic architecture means that your app is written as one cohesive unit of code whose components are designed to work together, sharing the same memory space and resources.

A microservice architecture means that your app is made up of lots of smaller, independent applications capable of running in their own memory space and scaling independently from each other across potentially many separate machines.

Monolithic Pros: The major advantage of the monolithic architecture is that most apps typically have a large number of cross-cutting concerns, such as logging, rate limiting, and security features such audit trails and DOS protection.
When everything is running through the same app, it’s easy to hook up components to those cross-cutting concerns.

There can also be performance advantages, since shared-memory access is faster than inter-process communication (IPC).

Monolithic cons: Monolithic app services tend to get tightly coupled and entangled as the application evolves, making it difficult to isolate services for purposes such as independent scaling or code maintainability.

Monolithic architectures are also much harder to understand, because there may be dependencies, side-effects, and magic which are not obvious when you’re looking at a particular service or controller.

Microservice pros: Microservice architectures are typically better organized, since each microservice has a very specific job, and is not concerned with the jobs of other components. Decoupled services are also easier to recompose and reconfigure to serve the purposes of different apps (for example, serving both the web clients and public API).
They can also have performance advantages depending on how they’re organized because it’s possible to isolate hot services and scale them independent of the rest of the app.

Microservice cons: As you’re building a new microservice architecture, you’re likely to discover lots of cross-cutting concerns that you did not anticipate at design time. A monolithic app could establish shared magic helpers or middleware to handle such cross-cutting concerns without much effort.

In a microservice architecture, you’ll either need to incur the overhead of separate modules for each cross-cutting concern, or encapsulate cross-cutting concerns in another service layer that all traffic gets routed through.
Eventually, even monolthic architectures tend to route traffic through an outer service layer for cross-cutting concerns, but with a monolithic architecture, it’s possible to delay the cost of that work until the project is much more mature.

Microservices are frequently deployed on their own virtual machines or containers, causing a proliferation of VM wrangling work. These tasks are frequently automated with container fleet management tools.
```

## Practical

### Q1: What is a potential pitfall with using `typeof bar === "object"` to determine if `bar` is an object? How can this pitfall be avoided?

### Ans:

```
This is a reliable way of checking however null is also considered object in Javascript so
(bar && typeof bar === "object)
this is correct way
```

### Q2: What will the code below output to the console and why?

```javascript
(function () {
  var a = (b = 3);
})();
console.log("a defined? " + (typeof a !== "undefined"));
console.log("b defined? " + (typeof b !== "undefined"));
```

### Ans

```
var a = b = 3; is a shorthand for
b = 3;  // not var b = 3;
var a = b;

Now, if strict mode is on, then it will give a runtime error that b is not defined (since b is outside function) if strict mode is off then a is not defined but b is defined.
```

### Q3: What will the code below output to the console and why?

```javascript
var myObject = {
  foo: "bar",
  func: function () {
    var self = this;
    console.log("outer func:  this.foo = " + this.foo);
    console.log("outer func:  self.foo = " + self.foo);
    (function () {
      console.log("inner func:  this.foo = " + this.foo);
      console.log("inner func:  self.foo = " + self.foo);
    })();
  },
};
myObject.func();
```

### Ans

```
outer func:  this.foo = bar
outer func:  self.foo = bar
inner func:  this.foo = undefined
inner func:  self.foo = bar
```

### Q4: What is the significance of, and reason for, wrapping the entire content of a JavaScript source file in a function block?

### Ans

```
This is an increasingly common practice, employed by many popular JavaScript libraries (jQuery, Node.js, etc.). This technique creates a closure around the entire contents of the file which, perhaps most importantly, creates a private namespace and thereby helps avoid potential name clashes between different JavaScript modules and libraries.
```

### Q5: What is the significance, and what are the benefits, of including `use strict` at the beginning of a javascript source file?

### Ans

```
it enforces stricter parsing and error handling. Code errors that would otherwise have been ignored or failed silently will now give error.
- It makes debugging easier
- Prevents accidental globals: Without strict mode, assigning a value to an undeclared variable automatically creates a global variable with that name. This is one of the most common errors in JavaScript. In strict mode, attempting to do so throws an error.
- Disallows duplicate parameter values
- Makes eval() safer
```

### Q6: Consider the two functions below. Will they both return the same thing? Why or why not?

```javascript
function foo1() {
  return {
    bar: "hello",
  };
}

function foo2() {
  return;
  {
    bar: "hello";
  }
}
```

### Ans

```
Surprisingly, they both are different. foo1 returns {bar: "hello"} and foo2 returns undefined.

This is because in javascript ; are optional (still we should write it for good practice) in foo2 return has no semicolon so parser puts one hence ignoring braces.
```

### Q7: What is NaN? What is its type? How can you reliably test if a value is equal to NaN?

### Ans

```
NaN = Not a number like "abc" / 4 will give NaN. Although NaN is not a number but it's type is Number. isNaN() function is used to check.

typeof NaN === 'number'     (this is true)
NaN === NaN                 (this is false XD)
```

### Q8: What is output of `console.log(0.1 + 0.2 === 0.3);`

### Ans: false

### Q9: Describe possible ways to write isNumber function

### Ans

```
function isInteger(x) { return (typeof x === 'number) && (x % 1) === 0; }

function isInteger(x) { return parseInt(x, 10) === x; }

function isInteger(x) { return Math.round(x) === x; }

xor number with 0, in javascript int float everything is stored as floting point.
function isInteger(x) { return (x^0) === x; }
```

### Q10: Write output of

```javascript
(function () {
  console.log(1);
  setTimeout(function () {
    console.log(2);
  }, 1000);
  setTimeout(function () {
    console.log(3);
  }, 0);
  console.log(4);
})();
```

### Ans

```
1
4
3
2
```

### Q11: What gets logged to the console when the user clicks Button4 and why? Provide one or more alternate implementations that will work as expected.

```javascript
for (var i = 0; i < 5; i++) {
  var btn = document.createElement("button");
  btn.appendChild(document.createTextNode("Button " + i));
  btn.addEventListener("click", function () {
    console.log(i);
  });
  document.body.appendChild(btn);
}
```

### Ans

```
No matter what button user clicks 5 will always be logged because at the time of event i is 5

for (var i = 0; i < 5; i++) {
  var btn = document.createElement('button');
  btn.appendChild(document.createTextNode('Button ' + i));
  btn.addEventListener('click', (function(i) {
    return function() { console.log(i); };
  })(i));
  document.body.appendChild(btn);
}

// or simplest solution just use let instead of var
for (let i = 0; i < 5; i++) {
  var btn = document.createElement('button');
  btn.appendChild(document.createTextNode('Button ' + i));
  btn.addEventListener('click', function(){ console.log(i); });
  document.body.appendChild(btn);
}
// https://stackoverflow.com/questions/762011/whats-the-difference-between-using-let-and-var
```

### Q12: What will be the output of `console.log(typeof typeof 1);`

### Ans: string because typeof "number" is string

### Q13: How do you add an element at the beggining and end of array?

### Ans: myArr = ['start', ...myArr, 'end'];

### Q14: What is output of console.log(3 > 2 > 1);

### Ans: False XD because javascript reads from left to right it will become true > 1, true has value 1 so 1 > 1 is false therefore it gives false

### Q15: How to clone an object?

### Ans

```
var obj = {a: 1, b: 2};
var objClone = Object.assign({}, obj);

this only creates a shallow copy here it's fine but if obj contains nested object it will fail, for that need to itterate and re call assign
```

### Q16: What will be the output of this code?

```javascript
var x = 21;
var girl = function () {
  console.log(x);
  var x = 20;
};
girl();
```

### Ans:

```
undefined

It's because x gets hoisted inside the function because of which global x value gets masked (if local x was not there then 21 must be output)
```

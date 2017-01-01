---
published: true
title: Scope and Closure
layout: post
categories: [Javascript]
---
# Scope

![scope](../img/scope.png)

Scope is range or space to where variable and function will be stored.
Scope is created when function is invoked. Local variable will be stored on Scope Object (LexicalEnvironment). It is not allowed to access inner scope object from the outter scope. On the other hand, it is possible to access outter scope from inner scope. Outter scope is parent scope from inner scope which allows to access outter scope from the inner scope. When the program accesses the variable, the JS interpreter searches the current scope object. If the interpreter can not find them, it goes to parent scope and return it. This is called scope chain. It seems like prototype chain but it is slightly different. When interpreter can not find variable, it returns not an error but `undefined` on the prototype chain. However, on the scope chain, it returns `ReferenceError`.

Global scope is always on the end of scope chain. If we declared variable and function on the top level, they will be on the global scope object.

Scope object lasts as long as the scope is referred. When referrence to the scope object is no longer exist, garbage collection works and scope object is released. In javascript, invoking function is referring scope object.

### Scope

You can see scope on this code.

```javascript
var message = "Hello"

function sayHello(name){
  console.log(message + ", " + name);
}

sayHello("Kei"); // Hello, Kei

```

The variable and function delcared on top level is on global scope. There are `message`, `sayHello()` on the global scope.
What is like accessibilty between scope.

```javascript
var message = "Hello" // Global Scope

function sayHello(name){
  console.log(message + ", " + name);
}

sayHello("Kei"); // Hello, Kei

```
`sayHello` is function object and create its own function scope. It is always referers outer scope. The ability to check outer scope is called scope chain. Deep nested function may cause deep search for scope which affects performance.

```javascript
var message = "Hello"

function sayHello(name){
  var message = "Good bye"
  console.log(message + ", " + name);
}

sayHello("Kei"); // Hello, Kei

```

In this case, this refers own `message` variable, so that JS engine does not look for outer scope.

Callback can also look for outer scope.

```javascript
var message = "Hello"

var foo = function(){
  console.log(message);
}

function bar(fn){
  fn();
}

bar(foo); // Hello
```

Despite Inner scope can access outer scope, we can not access inner scope from outer one.

```javascript
var message = "Hello"

var foo = function(){
  var userName = "Kei"
  console.log(message);
}


console.log(message); // Hello
foo(); // Hello
console.log(userName); // ReferenceError: userName is not defined
```

This is kind of trixy but we can use this feature for create private scope. 

```javascript
;(function foo(message){
  var userName = "Kei"
  function sayHello(message){
    console.log(message + " " + userName);
  }

  sayHello(message);
})("Hello"); // Hello Kei

sayHello("Hello") // ReferenceError: sayHello is not defined
```
This IIFE creates it own scope and `sayHello` function on out of IIFE can not be invoked because it is on outer scope of IIFE. 
Being wrapped by IIFE is common technic to create scope which is good for privacy and avoiding global scope pollution.

These are basic scope and scope chain.

### Hoisting


The concept of hoisting is simple. Declartion of variable and function goes to top of scope even you assign certain value to variable. 

##### Sample code

**code-1**

```javascript
foo() // foo is not a function

var foo = function(){  
}
```

Why this is not function? It is because `var foo` contains `undefined` at this moment.

**code-2**

```javascript
foo() // foo is not defined 
```


In this case, foo is not clearly defined because we did not declare any variable or function. Back to first code, only variable declartion is "hoisted"

**code-1**
```javascript
foo() // foo is not a function

var foo = function(){  
}
```

code-1 is equivalant to code-3.

**code-3**
```javascript
var foo; //undefined

foo(); // foo is not a function

foo = function(){  
};
```


To make this code work, we have to call method after assign function into variable foo. 

**code-4**
```javascript
var foo; //undefined

foo = function(){  
  console.log("foo")
};

foo(); // "foo"
```

Or Function Declaration


**code-5**
```javascript
foo();

function foo(){
  console.log("foo")
}
```
Function Declaration is also hoisted which means you can call function anywhere inside scope



### In the case of variable name collision

This is valid code.
```javascript
var x = 0;

function hoge(){
  console.log(x); // 0
}

hoge();
```


However, this does not work because of hoisting and name collision.

```javascript
var x = 0;

function hoge(){
  console.log(x); // undefined
  var x = 1; // var x declaration is hoisted
}

hoge();
```

Inside function scope, `var` declartion will be done at the begining. 
This code is same as code below.  

```javascript
var x = 0;

function hoge(){
  var x;
  console.log(x); // undefined
  x = 1; // var x declaration is hoisted
}

hoge();
```


#### Function is hoisted

Hoisting is the reason why 'function declaration' is called first over 'function expression'.

```javascript
foo(); // 1

var foo; // undefined

foo = function() {
    console.log( 2 );
};

// This function is hoisted to the top of scope.
function foo() {
    console.log( 1 );
}
```


http://wp-p.info/tpl_rep.php?cat=js-intermediate&fl=r9

---


### Another Example
```javascript
var salary = "1000$";

(function () {
  console.log("Original salary was " + salary);

  var salary = "5000$";

  console.log("My New Salary " + salary);
})();
```

```javascript
var salary = "1000$";

(function () {
  var salary = undefined;
  console.log("Original salary was " + salary);

  salary = "5000$";

  console.log("My New Salary " + salary);
})();
```



### What will the code below output to the console and why?
```javascript
(function(){
  var a = b = 3;
})();

console.log("a defined? " + (typeof a !== 'undefined')); // true
console.log("b defined? " + (typeof b !== 'undefined')); // false (ES6 ReferenceError: b is not defined)
```

```javascript
var a = b = 3;
// Wrong
var b = 3;
var a = b;

// Correct
b = 3;
var a = b;
```

### Why is it, in general, a good idea to leave the global scope of a website as-is and never touch it?

As variables lose scope, they will be eligible for garbage collection. If they are scoped globally, then they will not be eligible for collection until the global namespace loses scope.


### ES6
the lexical scope of a function is statically defined by the function’s physical placement within the written source code.

ES6 is block scope.


```javascript
function(){
  if(true){
     let a = 0
  }
  console.log(a)
}
```

# Closure

### What is a closure, and how/why would you use one?

Closure is when a function is able to remember and access its lexical scope even when that function is executing outside its lexical scope.

When you refer a function, you will also refer the scope of the function at the same time.

Let's see the code below.

```javascript

var outter, inner;

function outterFunction(){
  
  var message = "Hello";
  
  function innerFunction(){
    console.log(message);
  }
  
  inner = innerFunction;
  
}

outter = outterFunction;
outter(); // undefined

inner(); // "Hello"

```


This code explains what "remember and access its lexical scope even when that function is executing outside its lexical scope" means.

`function outterFunction()` is declared and invoked. Then `function innerFunction()` is assined into `var inner` which is on the top level. In Javascript, `outterFunction` is called then scope will not be gone. Closure remembers and access its lexical scope.

How about multiple nested situation like below?

```javascript
var a, b, c;

var name = "Kei";

function functionA(){
  
  var message = "Hello";
  
  function functionB(){

    console.log(message + " from function B");

    function functionC(){
      console.log(message + " from function C");
    }

    c = functionC;
  }
  
  b = functionB;
  
}

a = functionA;

a();
b();
c();
```

Despite multiple nested function, function remembers and is allowed to access to scope.



### Use Memory?

* Closures use memory, but they don't cause memory leaks since JavaScript by itself cleans up its own circular structures that are not referenced. Internet Explorer memory leaks involving closures are created when it fails to disconnect DOM attribute values that reference closures, thus maintaining references to possibly circular structures.

http://postd.cc/how-do-javascript-closures-work-under-the-hood/

```javascript
var x = 10;
 
function foo() {
  alert(x);
}
 
(function (funArg) {
 
  var x = 20;
 
  // variable "x" for funArg is saved statically
  // from the (lexical) context, in which it was created
  // therefore:
 
  funArg(); // 10, not 20
 
})(foo);
```
This is also closure. The first variable x remember

### Closure works in bad way.

Closure works wrong way.

```html
<p id="help">Form</p>
<p>Email: <input type="text" id="email" name="email"></p>
<p>Name: <input type="text" id="name" name="name"></p>
<p>Age: <input type="text" id="age" name="age"></p>
```

```javascript

function showHelp(help) {
  document.getElementById('help').innerHTML = help;
}

function setupHelp() {
  var helpText = [
    {'id': 'email', 'help': 'Your Email'},
    {'id': 'name', 'help': 'Your Fullname'},
    {'id': 'age', 'help': 'Your age'}
  ];

  for (var i = 0; i < helpText.length; i++) {
    var item = helpText[i];
    document.getElementById(item.id).onfocus = function() {
      showHelp(item.help); // Closure works
    }
  }
}

setupHelp();
```

In this case, closure works in bad way. Inside loop and `onfocus` function bind closure. Inside `onfocus` function item is first element by closure even for loop going on.
Using `let` keyword of the ES6 syntax, we can avoid this.



So what is closure used for?

1. Private / Public accessor 
2. Module
3. Callback
4. Practical closure (Curry)

---
### 1. To create private/public function

In some programming languages, if you keep privacy on some functions, you just write private or public explictly.

```javascript

var module = function(name){
  
  var name = name;
  
  function getName(){
    console.log(name);
  }
  
  
  return {
    getName: getName
  };
}


var m = module("kei");

m.getName();
```

`getName()` function remembers the scope which is closure. `var name` can not be and retrive by public pethod `getName()`. 
---

### 2. Module

Well, this code is almost similar to the code on private/public function section. We can apply closure to module.


```javascript

var module = (function(option){
  
  var option = option;
  
  function getVersion(){
    console.log(option.version);
  }

  function changeRootPath(path){
    option.appPath = path
  }
  
  return {
    getVersion: getVersion,
    changeRootPath: changeRootPath
  };

}(window.app || {}));


module.changeRootPath("/public/javascript/")

```
---
### 3. Callback

Closure works for callback very powerfully. Program can not access to outer scope without closure. Callback will be executed after outer function is invoked such as callback in ajax, setInterval or setTimeOut. 


- setInterval

```javascript
var message = "Hello"

setInterval(function(){ 
  console.log(message) 
}, 1000)
```

- ajax

ajax callback is invoked after sending request which means the callback function seems separated to parent function. However, the function inside ajax has closure and allows to access to outer scope.

```javascript
var userName = "Kei"
$.ajax({
  type: "GET",
  url: "http://localhost:3000",
  success: function(){
    console.log(userName);
  }
})
```
---

### 4. Practical closures / Curry

Practical closure is implemented with curry methology. The closure take a practical value of whole functionality but return function to implement rest of parts of functionality.

```javascript
function makeSizer(size) {
  return function() {
    document.body.style.fontSize = size + 'px';
  };
}

var size12 = makeSizer(12);
var size14 = makeSizer(14);
var size16 = makeSizer(16);
```
---

- http://dmitrysoshnikov.com/ecmascript/chapter-6-closures/
- http://stackoverflow.com/questions/26061856/javascript-cant-access-private-properties/26063201#26063201
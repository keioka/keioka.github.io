---
published: true
layout: post
title: prototype
categories:
  - Javascript
---
# Prototype

Prototype is an object from which other objects inherit propertiesand and the way we can inherent property to the object. You can not define Class in JavaScript. However, we can do a similar thing. How we can create class-ish something and inherent. The answer is Object. Any object has an internal connection between other objects internally, which is called Prototype. Prototype object has own prototype and search property through Prototype. This is called prototype chain. If prototype chain ends, it returns null.


The syntax `someObject.[[Prototype]]` referes prototype of someObject which is equal to `someObject.__proto__`

![img](http://i.stack.imgur.com/KFzI3.png)

### How Prototypes Affect In-Memory Model?



### Superclass and Subclass

### How prototypal inheritance works

Here is parent object.

```javascript
var animal = {
  bark: function(bark){  
    return bark;
  },
  stop: function(){
    return this.bark("Baw");
  }
};
```

It is kind of obsolete technic to make an inherent relationship.

```javascript
var dog = {};
// Set the object prototype
dog.__proto__ = animal;

console.log(dog.__proto__ === animal); // true
console.log(dog.bark("wao")) //wao
console.log(dog.stop()) //"Baw"
```

since ES5, it is better syntax.
```javascript
var dog = Object.create(Animal);

console.log(dog.__proto__ === animal); // true
console.log(dog.bark("wao")) //wao
console.log(dog.stop()) //"Baw"
```

`Object.create` create empty object and add `__proto__` on it.

[javascript-inheritance-patterns](http://postd.cc/javascript-inheritance-patterns/)

---

### Inheritance gives reference to child

Variable is pass by value. When you refer the property of an object, it is not passed by value which means not copy of the value. It refers to parent property through prototype chain. Deep prototype chain can make program slower so that we have to make sure you don't create too deep to refer unnecessarily.

Ex1:) Pass by Reference 

```javascript
var animal = {
  name: "kei" 
}

var dog = {}

dog.__proto__ = animal

dog.name // "kei"

animal.name = "John" 

dog.name // "John"
```

Ex2:) Pass by Reference 

```javascript
var e = {
  message: "Hello"
}


var f = Object.create(e);
console.log(f.__proto__);
console.log(f.message); // "Hello"

e.message = "Error"
console.log(f.__proto__);
console.log(f.message); // Error

```

Ex3:) Pass by Value

```javascript
var a = [1,2,3];
var b = a; // Copied value

console.log(a); //[1,2,3]
console.log(b); //[1,2,3]

a = [4, 5, 6];
console.log(a); //[4,5,6]
console.log(b); //[1,2,3]
```

Ex4:) Pass by value

```javascript

var c = {
  a: [7,8,9]
}

var d = c.a //copied value [7,8,9]

c.a = [10, 11, 12];

console.log(c.a); //[10, 11, 12]
console.log(d); //[7, 8, 9]

```




### __proto__ VS. prototype in JavaScript

__proto__ is the actual object that is used in the lookup chain to resolve methods, etc.  prototype is the object that is used to build __proto__ when you create an object with new



When creating a function, a property object called prototype is being created automatically and is being attached to the function object. This new prototype object also points to or has an internal-private link to, the native JavaScript Object. If you will create a new object out of Foo using the new keyword, you basically creating (among other things) a new object that has an internal or private link to the function's prototype Foo we discussed earlier



### `this` in prototype

```javascript
function Animal(name){
  this.animalName = name;
}


function Plant(name){
  var plantName = name;
}


var animal = new Animal("Kei");

var plant = new Plant("John");
```

![img](../img/prototype/Screen-Shot3.png)
![img](../img/prototype/Screen-Shot4.png)

When we don't bind variable on `this` context for Plant, it will be not accessed from an object.
We attached animalName by using `this` keyword.

What happened in this case?

```javascript
function Animal(name){
  this.animalName = name;
  this.printName = function(){
    console.log(this.animalName);
  }
}


function Plant(name){
  var plantName = name;
  this.printName = function(){
    console.log(plantName);
  }
}


var animal = new Animal("Kei");

var plant = new Plant("John");

console.log(animal.printName());

console.log(plant.printName());
```

![img](../img/prototype/Screen-Shot7.png)
![img](../img/prototype/Screen-Shot8.png)

Closure works on Plant constructor function. `animal.printName()` access own property `animalName`. Plant does not have property `plantName` but access value from closure.

```javascript
function plant(name){
  var plantName = name;
  this.printName = function(){
    console.log(plantName);
  }
}

plant("John");
```

What if we call this plant function as not a constructor but function.
Unless object created from a constructor, this context is global scope in this case so that this context will be the `window`.


```javascript
function Dog(){};

Dog.prototype = {
  legs: function(){
  },
  height: function(){
    return this.height;
  }
};

var dog = new Dog(); 

dog.__proto__ 
// Object {
//   legs: function(){
//   },
//   height: function(){
//     return this.height;
//   }
// }

dog.__proto__ == Animal.prototype // true
```

`dog` refers to `Dog` prototype. The __proto__ getter function exposes the value of the internal [[Prototype]] of an object. 

```javascript
var o = {}

Object.prototype === o.__proto__ //true

o.constructor === Object;

var f = function(){}
Function.prototype === f.__proto__ 
```

```javascript
var Cat = function(name){ this.name = name; }
var myCat = new Cat("Kei"); 
myCat.prototype

myCat.__proto__ //

Cat.prototype = { getName: function(){ return this.name }}

myCat.__proto__
```

How instance refers to prototype? It is not whole copy of constructor function. Prototype object of constructor is always referred from instance object dynamically through prototype chain.

```javascript
function Cat(){}

var myCat = Cat("Kei");
myCat.__proto__

// Object >
//   constructor:　Cat(name)
//   __proto__: Object

Cat.prototype.getName = function(){
  return this.name;
}

myCat.__proto__
//  constructor:　Cat(name)
//  getName: function()
//  __proto__: Object
```

#### What is constructor?

Instance objects does not have constructor property. Prototype of constructor function has constructor.



#### What is function constructor and instance object

```javascript
Cat.__proto__ === Function.prototype
Cat.prototype.__proto__ === Object.prototype

Object.__proto__  === Function.prototype
Function.__proto__  === Function.prototype
Function.prototype.__proto__ === Object.prototype

Object.prototype.__proto__ // null => this is end of prototype chain

myCat.__proto__ === Cat.prototype // true
myCat.__proto__.__proto__ === Object.prototype // true
Cat.prototype.__proto__ === Object.prototype // true
```





### ES6 class

ES6 provide class syntax. However, it is just syntax sugar and still Prototype.





### What is `Function.prototype.bind`.

bind is the way to bind `this` object.
`this` is determined where the function is called. When the function is called in the global scope, `this` would be `window` object.  
Use .bind() when you want that function to later be called with a certain context, useful in events

```javascript
var x = 0;

module = {
  x: 9,
  get: function(){ 
    return this.x
  }
}

console.log(module.get())

var m = module.get //=> 9

//  This is equal to the code below
//  var m = function(){ 
//    return this.x //=> this refers to window 
//  }

console.log(m()) //=> 0
console.log(m.bind({x: 10})()) //=> 10
console.log(m.bind({y: 10})()) //=> undefined
```
[feature-detection-vs-inference](http://lucybain.com/blog/2014/feature-detection-vs-inference/)

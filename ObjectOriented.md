# Object Oriented Design in Javascript

## Lesson Objectives

1. Use JavaScript objects to model the real world
1. Assign functions to variables and properties of objects
1. Use methods to access properties of the object
1. Use constructor functions to make creating multiple similar objects easy
1. Create a base class which can be used to create child classes

## Use JavaScript objects to model the real world

Objects in javascript are just key/value pairs

```javascript
var matt = {
	age: 36,
	eyes: 'blue',
	name: 'Matt'
}
```

Can retrieve values using dot notation

```javascript
matt.age //returns 36
```

Can modify an object's properties like so:

```javascript
matt.age = 40;
matt.eyes = 'green';
```

## Assign functions to variables and properties of objects

Functions are objects and can be assigned to variables

```javascript
var myFunc = function(){
	console.log('hi!');
}

myFunc();
```

Functions can be assigned to properties of objects.  These functions are referred to as methods of the object.  It's like having the object perform an action.

```javascript
var matt = {
	sayHello: function(){
		console.log('Hello, my name is Matt!');
	}
}

matt.sayHello();
```

## Use methods to access properties of the object

When invoking methods, you can reference other properties of the object with the keyword `this`

```javascript
var matt = {
	name: 'Matt',
	greet: function(){
		console.log('Hello!  My name is' + this.name);
	}
}

matt.greet(); //logs 'Hello!  My name is Matt'
```

## Use constructor functions to make creating multiple similar objects easy

If you need to create multiple objects of the same type, object literals (`{ property:'value'}`) can be inefficient.  We can create constructor functions, which act like a blueprint (or class) for creating objects.

```javascript
var Person = function(){
	this.numArms = 2; //use the this keyword to create properties and methods
	this.numLegs = 2;
}

var me = new Person(); //use the new keyword to instantiate a new object
var someoneElse = new Person();
console.log(me);
console.log(someoneElse);
```

We can pass parameters into constructor functions to make instances unique

```javascript
var Person = function(name){
	this.name = name;
	this.numArms = 2; //use the this keyword to create properties and methods
	this.numLegs = 2;
}

var me = new Person('Matt'); //use the new keyword to instantiate a new object
var someoneElse = new Person('Joey Jo-Jo Junior Shabadoo');
console.log(me);
console.log(someoneElse);
```

Methods are added just like adding properties

```javascript
var Person = function(name){
	this.name = name;
	this.numArms = 2; //use the this keyword to create properties and methods
	this.numLegs = 2;
	this.sayHello = function(){
		console.log("Hello, my name is " + this.name);
	}
}

var me = new Person('Matt'); //use the new keyword to instantiate a new object
me.sayHello();
```

<!-- Since functions are objects, we can create static properties/methods (properties and methods that relate to the class, not the instances of the class) by using dot notation

```javascript
var Person = function(){
	/*
	usual stuff here...
	*/
}

Person.genders = ['male', 'female'];
console.log(Person.genders);
``` -->

## Create a base class which can be used to create child classes

We can have a "class" inherit from another class, using the `.call` static method of a function

```javascript
var Car = function(){
	this.wheels = 4;
}

var Humvee = function(){
	Car.call(this);
	this.numAmericanFlags = 4;
}

var murica = new Humvee();
console.log(murica);
```

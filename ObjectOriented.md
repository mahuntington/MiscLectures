# Object Oriented Design in Javascript

## Lesson Objectives

1. Use JavaScript objects to model the real world
1. Assign functions to variables and properties of objects
1. Use methods to access properties of the object
1. Use constructor functions to make creating multiple similar objects easy
1. Create a base class which can be used to create child classes
1. Use prototype to assign properties to a class
1. Assign the correct prototype to a sub class 
1. Correctly assign the constructor of a child class

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

## Use prototype to assign properties to a class

The problem with using `this.wheels = 4` inside a constructor function is that the code runs each time you create a new object.  This is redundant.

Instead we can assign the property to the function's `.prototype` property instead.

```javascript
var Car = function(){
}
Car.prototype.wheels = 4;
var myCar = new Car();
console.log(myCar.wheels);
```

- `Car.prototype.wheels = 4;` runs only once, but all instances of the Car class have the wheels property already set.  You can do this with any property you want
- If you do `console.log(myCar)` you'll notice that you can inspect the prototype of the Class of the object by looking at the `.__proto__` property.  There you'll find the .wheels property you added to `Car.prototype`
- When you look for a property of an object, it will first look on the object itself, then on its prototype, then on the prototype's prototype, etc...

## Assign the correct prototype to a sub class 

If we are using prototype to add methods and properties to our class, our inheritance process gets a little tricky

```javascript
var Person = function(){
}

Person.prototype.legs = 2;
Person.prototype.arms = 2;

var Student = function(){
    Person.call(this);
}

var matt = new Student();

console.log(matt.legs); //returns undefined?!
```

To fix this, we need to assign a person object to the `Student.prototype` property

```javascript
var Person = function(){
}

Person.prototype.legs = 2;
Person.prototype.arms = 2;

var Student = function(){
    Person.call(this);
}

Student.prototype = Object.create(Person.prototype); //create a Person object, then assign it to the Student.prototype

var matt = new Student();

console.log(matt.legs); //returns 2
```

Now we've created a Person object and assigned it to the to Student's prototype property.  Now when a Student object looks for `.legs` it will go to the Student's `__proto__` (Person object) and then the `__proto__` of that object.

## Correctly assign the constructor of a child class

A good way to check a constructor is to log the `.__proto__.constructor` property:

```javascript
var Person = function(){
    console.log('constructing person');
}

Person.prototype.legs = 2;
Person.prototype.arms = 2;

var matt = new Person();
console.log(matt.__proto__.constructor);
```

This becomes a problem when doing inheritance:

```javascript
var Person = function(){
    this.foo = 'bar';
}

Person.prototype.legs = 2;
Person.prototype.arms = 2;

var Student = function(){
    Person.call(this);
}

Student.prototype = Object.create(Person.prototype);

var matt = new Student();

console.log(matt.__proto__.constructor); //logs Person?!
```

This is because we are wiping out the normal constructor function when we reassign the prototype.  To fix this, we need to add the constructor back in:

```javascript
var Person = function(){
    this.foo = 'bar';
}

Person.prototype.legs = 2;
Person.prototype.arms = 2;

var Student = function(){
    Person.call(this);
}

Student.prototype = Object.create(Person.prototype);
Student.prototype.constructor = Student; //add the constructor back in to the prototype

var matt = new Student();

console.log(matt.__proto__.constructor);
````

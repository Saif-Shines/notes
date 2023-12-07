# JS: Deep Foundations v3

# Introduction

[ECMAScript® 2018 Language Specification](https://www.ecma-international.org/ecma-262/9.0/index.html#Title)

[https://static.frontendmasters.com/resources/2019-03-07-deep-javascript-v2/deep-js-foundations-v2.pdf](https://static.frontendmasters.com/resources/2019-03-07-deep-javascript-v2/deep-js-foundations-v2.pdf)

## Pillars

### Types

1. Primitive types
2. Abstract Operations
3. Coercion
4. Equality
5. TypeScript, Flow etc.

### Scope

1. Nested Scope
2. Hoisting
3. Closure
4. Modules

## Objects(Oriented)

1. this
2. class {}
3. Prototypes
4. OO vs. OLOO

# Types

### Primitive types

1. undefined
2. string
3. number
4. boolean
5. object
6. symbol

### Others those have behavior similar to types

1. undeclared
2. null
3. function
4. array
5. bigint

![JS%20Deep%20Foundations%20v3%20e69058f130fe4a2aade461c4b2ffda15/Untitled.png](JS%20Deep%20Foundations%20v3%20e69058f130fe4a2aade461c4b2ffda15/Untitled.png)

> In javascript, variables don't have types but values do. Because it's a dynamic language
> 
- `typeof` can only return **strings**
- If `var v = null` and `typeof v;` is performed,
    - It will return string `"object"` even it's a one of the value types.
    - Historically, it was recommended that, if you are are unsetting a primitive value in a variable, use **undefined** and if you are unsetting a variable that has  a reference to **object, use null.** This is part of story of this bug.
- `typeof` is the only operator that can reference to a variable that doesn't exist and not throw an error.

### Ways to define emptiness

1. undefined — an initialized but is undefined
2. undeclared — A variable that is not even created and is undeclared
3. uninitialized(aka TDZ) — A variable that is never been initialized
    1. TDZ is just a type of error. More about it when learning scope.
    

NaN = invalid number ; ~~Not A Number~~

```jsx
var myAge = Number('0o46'); // Hex so converts into number
var myNextAge = Number('39');
var myCatsAge = Number('n/a');
// output
myAge - "my dad's age"; // NaN

myCatsAge === myCatsAge; // false because two NaN are different because of IEEE spec defined for numbers

isNaN(myAge); // false
isNaN(myCatsAge); // true
isNaN("my dad's age"); // true - in isNaN definition the string passed is coerced into Number that turns out to be NaN and isNaN of NaN is true.

Number.isNaN(myCatsAge); // In ES6 - it is fixed
Number.isNaN("my dad's age");
```

```jsx
NaN
false
false
true
true
true
false
```

- typeof NaN is 'number' . Just that it is Invalid Number.

```jsx
var trendRate = -0; // An example of using -0 can be a game that indicates car moving in opposite direction
trendRate === -0; // true . Fine.

trendRate.toString(); // '0'. Some how JS founders assumed '-0' can be developers mistake. That itself went very wrong.
trendRate === 0; // true
trendRate < 0; // false
trendRate > 0; // false

// The below is new and recommended way to do this equality check
Object.is(trendRate, -0); // true
Object.is(trendRate, 0); // false
```

The Math.sign() function returns either a positive or negative +/- 1, indicating the sign of a number passed into the argument. If the number passed into Math.sign() is 0, it will return a +/- 0. Note that if the number is positive, an explicit (+) will not be returned.

- something

```jsx
Math.sign(-3); // -1
Math.sign(3); // 1
Math.sign(-0); // -0 Bug
Math.sign(0); // 0 Bug

// Own Fix.
function sign(v) {
  return v !== 0 ? Math.sign(v) : Object.is(v, -0) ? -1 : 1;
}

sign(-0);
sign(0);
```

```jsx
true
'0'
true
false
false
true
false
```

```jsx
-1
1
-0
0
-1
1
```

## Fundamental Objects

### aka Built-in Functions

### Use new keyword

1. Object()
2. Array()
3. Date()
4. Function()
5. RegExp()
6. Error()

### Don't use new keyword

1. String()
2. Number()
3. Boolean()

# Coercion

### Abstract Operations

Abstract Operations are the operations that occur within JavaScript, to which we refer to them as function. For Examples **ToPrimitive(**hint**) is** one such Abstract Operation. 

- In reality it may be or may not a function inside a JS Engine. But surely is a Algorithm.
- An Algorithm which we verbally refer to it.
- Yet, based on the "hint", this operation will return a value.

For example ToPrimitive will do ValueOf() and toString() if the hint is a number. Hint comes from the kind of Math operation developer is trying to perform in the code. It does until, it gets and primitive value.

## ToPrimitive(hint)

1. It is a abstract operation that takes an hint and tries to convert(indirectly) for example, Object, function, Array to Primitive values
2. Primitive values can only be either String or Number.
3. If the Hint is String
    1. It will perform toString() operation first. If the returned value is a primitive, that will be also the result of ToPrimitive
    2. If toString()'s return value is not a primitive, then it will apply valueOf() property. The return value of valueOf() is the return result of ToPrimitive(hint)
    3. If both of them fail to convert into primitive value, it will throw an error.

<aside>
💡 Abstract operation are just abstract set of rules. They are not actual funcitons

</aside>

## ToString

As told, ToPrimitive(hint) operation is performed indirectly.

The way this works is

1. When ToString operation is performed
2. It calls ToPrimitive(hint of String)
3. Then
    1. It is a abstract operation that takes an hint and tries to convert(indirectly) for example, Object, function, Array to Primitive values
    2. Primitive values can only be either String or Number.
    3. If the Hint is String
        1. It will perform toString() operation first. If the returned value is a primitive, that will be also the result of ToPrimitive
        2. If toString()'s return value is not a primitive, then it will apply valueOf() property. The return value of valueOf() is the return result of ToPrimitive(hint)
        3. If both of them fail to convert into primitive value, it will throw an error.

### When applied on primitives

![JS%20Deep%20Foundations%20v3%20e69058f130fe4a2aade461c4b2ffda15/Untitled%201.png](JS%20Deep%20Foundations%20v3%20e69058f130fe4a2aade461c4b2ffda15/Untitled%201.png)

### When applied on Arrays

![JS%20Deep%20Foundations%20v3%20e69058f130fe4a2aade461c4b2ffda15/Untitled%202.png](JS%20Deep%20Foundations%20v3%20e69058f130fe4a2aade461c4b2ffda15/Untitled%202.png)

### When applied on Objects

![JS%20Deep%20Foundations%20v3%20e69058f130fe4a2aade461c4b2ffda15/Untitled%203.png](JS%20Deep%20Foundations%20v3%20e69058f130fe4a2aade461c4b2ffda15/Untitled%203.png)

<aside>
💡 Note that you can basically return different than the traditional one by simply adding `toString(){return "Your definition"}`

</aside>

## ToNumber

### When applied on primitives

![JS%20Deep%20Foundations%20v3%20e69058f130fe4a2aade461c4b2ffda15/Untitled%204.png](JS%20Deep%20Foundations%20v3%20e69058f130fe4a2aade461c4b2ffda15/Untitled%204.png)

![JS%20Deep%20Foundations%20v3%20e69058f130fe4a2aade461c4b2ffda15/Untitled%205.png](JS%20Deep%20Foundations%20v3%20e69058f130fe4a2aade461c4b2ffda15/Untitled%205.png)

### When applied on Arrays

![JS%20Deep%20Foundations%20v3%20e69058f130fe4a2aade461c4b2ffda15/Untitled%206.png](JS%20Deep%20Foundations%20v3%20e69058f130fe4a2aade461c4b2ffda15/Untitled%206.png)

![JS%20Deep%20Foundations%20v3%20e69058f130fe4a2aade461c4b2ffda15/Untitled%207.png](JS%20Deep%20Foundations%20v3%20e69058f130fe4a2aade461c4b2ffda15/Untitled%207.png)

### When applied on Objects

![JS%20Deep%20Foundations%20v3%20e69058f130fe4a2aade461c4b2ffda15/Untitled%208.png](JS%20Deep%20Foundations%20v3%20e69058f130fe4a2aade461c4b2ffda15/Untitled%208.png)

## ToBoolean

It is not much of operation, it more like a lookup.

![JS%20Deep%20Foundations%20v3%20e69058f130fe4a2aade461c4b2ffda15/Untitled%209.png](JS%20Deep%20Foundations%20v3%20e69058f130fe4a2aade461c4b2ffda15/Untitled%209.png)

## Boxing

How does we get a value on primitive value?

```jsx
if(studentNameElem.value.length > 50){
	console.log("Student's name too long.");
}

var ele = 'Hello';
ele.length; // 5
```

1. When ever developer tries to access a property on a primitive value that is stored in an identifier, Javascript says "Since you are trying to access a property on a primitive value, So I will try to implicitly coerce that for you. 
2. Which otherwise, thread of execution needs to throw an error saying property is not defined. "

![Some of the corner cases on javascript](JS%20Deep%20Foundations%20v3%20e69058f130fe4a2aade461c4b2ffda15/Untitled%2010.png)

Some of the corner cases on javascript

## The Root of All (coercion) Evil

1. If you look at `Number([]), Number("") , Number([null]) , Number([undefined]) , String([null]) , String ([undefined])` all of them basically route due to empty string or string with white spaces.
2. For example, **Refer how ToString and ToNumber abstract operations work?**
    1. `Number([])`→  tries ToNumber and fails → turns `[]` ToString → gets `""` → then does ToNumber and passes → returns 0
    2. `Number("")` →  tries ToNumber and passes → returns `0`
    3. `Number([null])` →  tries ToNumber and fails → so tries ToString and gets `""` → so does ToNumber and passes → with return `0` 
    4. `Number([undefined])` →  tries ToNumber and fails → so tries ToString and gets `""` → so does ToNumber and passes → with return 0 
    5. String([null]) → tries ToString and passes → with return of `""`
    6. String([undefined]) → tries ToString and passes → with return of `""`
3. Note that `Number("   /n /t")` will also evaluate to `0`
    1. It's becase ToNumber abstract operation will pass on ToPrimitive with hint of number and engine will invoke valueOf() first removing all the white spaces and remaining `""` will be passed to `toString()`

![Boolean corner cases on JS.](JS%20Deep%20Foundations%20v3%20e69058f130fe4a2aade461c4b2ffda15/Untitled%2011.png)

Boolean corner cases on JS.

> Instead, you have to adopt a coding style that makes value types plain and obvious.
> 

> You code comments should explain why. Not How.
> 

# Equality

1. The `==` and `===` are exactly same when both the types are exactly same. Whatsoever. Look at Spec section 7.11.12 you will see that == will perform === when types on both the sides are same.

## Summary of `==`

1. If the types are same: do`===`
2. If null or undefined: equal
3. If non-primitives: ToPrimitive
4. Prefer: ToNumber 

### Corner Cases

```jsx
[] == ![]; // true  WAT!?

```

## Avoid

1. `==` with `0 ""` (or event " ")
2. `==` with non-primitives
3. `==` true or == false : allow ToBoolean implicitly or use ===

# Static Typing

## TypeScript, Flow and type-aware linting

### Benefits:

1. Catch type-related mistakes
2. Communicate type intent
3. Provide IDE feedback

### Caveats:

1. Inferencing is best-guess, not a guarntee
2. Annotations are optional
3. Any part of the application that isn't typed introduces uncertainty

# Scope

> In lexically scoped languages like JavaScript, the scopes are determined at compile time and not Runtime. They are used at Runtime. This allows the JS Engine to efficiently optimize, because everything is known and it's fixed. Takeaway is the scopes to which variables are assigned at author time are going to stay the same way.
> 

# Scope and Functions Expressions

## Function Declaration

If the keyword `function` is literally the first keyword in the scope, that you can basically call it Function Declaration.

```jsx
function aFunc(parameter){  
// Function Definition
}
```

## Function Expression

Function Expressions usually are oriented to pass functions here and there. 

One of the key difference between the both is, In function expression, their definition get their own scope while in Function declaration they attach themselves to the scope in which they are defined.

```jsx
var JoblessFunction = function keyHandler(){
// some function definition
}
```

## Why you should prefer named function than anonymous function

3 Reasons

1. Reliable function self-reference (recursion, etc)
2. More debuggable stack traces
3. More self-documenting code

> Don't use arrow functions because they are short and concise. They are anonymous as well.
> 

Rule of thumb: When to use Function Declaration vs Function Expressions

If your code is more than 3 lines of code, use Function Declaration. If it's 1,2 or 3 lines of code the use inline function expression unless it will be used heavily in which case fall back to Function Declaration.

# Advanced Scope

## Lexical Scope vs Dynamic Scope

1. When you think of Lexical Scope think about identifier's scope being determined at author time or compile time before the execution. This where thread of compilation talks to scope manager and asks "Hey scope manager, is this identifier formally declared before? and if no. gives it a marble."
2. JavaScript is 100% lexically scoped.
3. Dynamic Scope however is redundant now a days but Bash Scripting is good example for that.

> A block is not a scope until there's a let or a const in it.
> 

### var vs let

```jsx
function lookupRecord(searchStr){
	try{
		var id = getRecord(searchStr);
	}
	catch (err){
		var id = -1;
	}
	return id;
}

```

In this case, we generally think of try and catch as syntactic brackets. They are not scoped by default. They only become when we use var, let or const. Any way, if you try to change all var's to let's in above code. The code will break. Because id is not available outside.

### Const means

> A variable that can't re-assigned. As opposed to a lot of developers thinking it is not going to change.
> 

```jsx
var teacher = 'Suzy';
teacher = 'Kyle'; // OK

const myTeacher = teacher;
myTeacher = 'Suzy'; // TypeError

const teachers = ['Kyle', 'Suzy'];
teachers[1]; // Allowed. WoW
```

## Hoisting

JS engine doesn't Hoist at all. It is english language convention to discuss idea of lexical scope. But in reality, during the compilation time, they are determined by thread of compilation with scope manager and during runtime they are executed by either target or source reference.

Example:

```jsx
student;
teacher;
var student = "you";
var teacher = "Kyle";
```

1. Lexical scope is nothing but author time scope.
2. The thread of compilation goes through all the formal declarations and determines if they are of source reference or of target reference.
    1. If source reference, thread of execution can extract data.
    2. If target reference, thread of execution can store data to those identifiers.
3. In above case identifiers — `students` and `teacher` are declared as `var` and they are of target reference during thread of compilation kicks in.

```jsx
getTeacher();

function getTeacher() {
  return teacher;
}
teacher = 'Kyle';
var teacher;
```

**Important points**

1. Thumb rule — Just don't change all var's to let's
2. `let` is used to signify there's block scope. It means that particular variable is only needed there and no where else.
    1. For example.
    
    ```jsx
    function useLet(){
    	if(x<y){
    		let temp = x;
    			y = x;
    			x = y;
    	}
    	return y-x;
    }
    ```
    

Only use `const` with things like strings, Booleans or numbers

```jsx
teacher(); // 4

function teacher() {
  var a = 4;
  return a;
}
```

1. In the above case, during the thread of compilation happens, teacher's function definition is formally declared.
2. However,
3. In the below case, thread of compilation the otherTeacher is declared with undefined during the compilation. So when thread of execution tries to executes from the start again, all it has at time is undefined and throws TypeError

```jsx
function teacher(){
	return "Kyle"
}
var otherTeacher;

teacher(); // Kyle
otherTeacher(); // TypeError

otherTeacher = function(){
	return "Suzy";
}
```

## TDZ error: let doesn't hoist? It does but..

TDZ error generally occurs when value that you are trying to access is basically uninitialized. It is very different to **undefined**. **undefined** is a value that is assigned after initialization. But uninitialized means, the thread of compilation has blocked the area in the memory and is inaccessible. 

```jsx
{
	teacher = "Kyle";
	let teacher; // TDZ - Temporary Dead Zone
}
```

1. In the above code, the lines are in block scope.
2. Thread of compilation doesn't actually do anything with `teacher = "Kyle"` . But Thread of compilation only cares about declarations like variable and function declarations.
3. After the Thread of compilation determines the teacher variable being declared as target reference, it's the job of thread of execution to do execute these lines.
    1. When reached `teacher="Kyle"` ..
        1. Thread of execution needs to access the teacher `identifier` which then thread of execution finds it is uninitialized and throws a ReferenceError.
        2. But if that is `var teacher` the thread of compilation initializes it with undefined value.

```jsx
var teacher = "Kyle";
{
	console.log(teacher); // TDZ errpr
	let teacher = "Suzy";
}
```

1. Read the L.H.S example first before coming to this.
2. In this case, you may be under the assumption that if thread of execution doesn't find the value of teacher when logging `teacher` it will go look in it's outer scope.
3. But the `let` 's will only determined in it's own block scope during thier phase in thread of compilation. 
4. So when thread of execution tries to access teacher first, it will uninitialized and not able to access it!!!

# Closure

> Closure is when a function "remembers" it's lexical scope even when the function is executed outside that lexical scope.
> 

*At this moment please try to go through the slides of Kyle's and think.*

## Module Pattern

### Namespaces

```jsx
var workshop = {
	teacher: "Kyle"
	ask(question){
		console.log(this.teacher,question);
	}
}

workshop.ask("Is this a module?");
```

## Over Engineered Namespaces

```jsx
var workshop = (function Module(teacher) {
  var publicAPI = { ask };
  return publicAPI;
  function ask(question) {
    console.log(teacher, question);
  }
})('Kyle');

workshop.ask("It's a over-engineered namespace"); // 'Kyle' "It's a over-engineered namespace"
```

1. Here is a variable **workshop** defined and has been assigned with an IIFE.
2. IIFE is singleton, so it takes up one argument `'Kyle'`
3. When run, as I know it happens in two passes
    1. First pass is by thread of compilation, that goes through declarations and determines the variables and their access to scopes. var, let, const.
    2. In the 2nd pass is by thread of execution, that goes through and actually executes values to the memory and run the executable lines.
4. The reference of entire IIFE is labelled to workshop. That also means teacher variable is always 'Kyle'.
5. When workshop.ask is executed it prints //'Kyle' "It's a over-engineered namespace"
6. Here, data and behaviour is encapsulated. But.. state is not held by it's methods. state is "Kyle" forever.

A common pattern the developer assume that packaging variables and functions together into a object is not Module pattern. The idea behind this phrase is called **Namespace pattern**. It's an idiom.

Module pattern requires some form of encapsulation. That means having control over the visibility.

> Modules encapsulate data and behaviour(methods) together. The state(data) of a module is held by its methods via closure.
> 

### Over Engineered Namespace Factory

```jsx
function WorkshopModule(teacher) {
  var publicAPI = { ask };
  return publicAPI;
  //********************
  function ask(question) {
    console.log(teacher, question);
  }
}

var workshop = WorkshopModule('Kyle');
var workshop = WorkshopModule('Saif');

workshop.ask("It's a Namespace creation factory");
```

1. Ctd.. Over Eng Namespaces.
2. Do you see state being created again and again?
    1. It's called creating  Namespace Factory.

# Objects (Oriented)

"Object**s**" because it is not strictly a class oriented system.

### this

> A function's this references the execution context for that call, determined entirely by how the functions are called. A this aware function can thus have a different context each time it's called, makes it more flexible and reusable.
> 

### Second way of invoking a function

**Explicit Binding:** Use .call and .bind or .apply for explicit binding

```jsx
function ask(question) {
  console.log(this.teacher, question);
}

var workshop1 = {
  teacher: 'Kyle',
};

var workshop2 = {
  teacher: 'Suzy',
};

ask.call(workshop1, 'Can I explicitly set context?');

ask.call(workshop2, 'Can I explicitly set context?');
```

### First way of Invoking a Function

**Implicit Binding:** direct usage on namespace object for implicit binding

```jsx
var workshop = {
	teacher: "kyle",
	ask(question){
		console.log(this.teacher,question);
	}
};

workshop.ask("What is implicit binding?");

// kyle What is implicit binding
```

### Loosing you this binding?

```jsx
var workshop = {
  teacher: 'Kyle',
  ask(question) {
    console.log(this.teacher, question);
  },
};

setTimeout(workshop.ask, 10, 'Lost this?'); // undefined Lost this?

setTimeout(workshop.ask.bind(workshop), 10, 'Hard bound this?'); // Kyle Hard bound this?
```

### 3rd way of invoking a function

```jsx
function ask(question) {
  console.log(this.teacher, question);
}

var newEmptyObject = new ask("Whats is 'new' doing here?");
//undefined "Whats is 'new' doing here?"
```

1. The `new` keyword is 3rd way of invoking a function.
2. When ever you use new keyword, JS engine invokes the this-aware function and passes an empty object as an context. That is similar to `ask.call({},'Can I exlicitly set context?')` in the above example. So new keyword has nothing to do with class and is just syntactic sugar over explicit binding of the function.

### 4 steps that new keyword does

1. Create a brand new empty object
2. * Link that object to another object 
3. Call/Invoke function with this; set the new object
4. If function does not return an object, assume return of this

### 4th way of invoking a function — Default Binding

```jsx
var teacher = "Kyle";

function ask(question){
  console.log(this.teacher, question);
}

function askAgain(question){
  "use strict";
  console.log(this.teacher, question);
}

ask("What's the non-strict-mode default?");
// Kyle What's the non-strict-mode default?

askAgain("What's the strict mode default?")
// TypeError
```

1. When you do a normal function call to a  this-aware function, without and implicit and explicit binding, the thread of execution defaults the context to **global**. 
2. Especially when you try to do it in **strict mode**, JS Engine defaults it to **undefined** and if you are trying to access any property on this undefined will throw a TypeError.
3. If for any reason in the code has all the 3-4 ways of calling a this-aware function. The order of precedence is determined by asking following questions
    1. Is the function called by new ?
    2. Is the function called call() or apply()?
        1. NOte: bind() effectively uses apply()
    3. Is the function called on a context object?
    4. DEFAULT: Go back to pointing at global (expect in strict mode — in that case point to undefined)

### What about Arrow function?

```jsx
var workshop = {
  teacher: 'Kyle',
  ask(question) {
    setTimeout(() => {
      console.log(this.teacher, question);
    }, 1000);
  },
};

workshop.ask("Is this lexical 'this'?"); // 'Kyle' "Is this lexical 'this'?"
```

```jsx
var workshop = {
  teacher: 'Kyle',
  ask: (question) => {
    console.log(this.teacher, question);
  },
};

workshop.ask("Is this lexical 'this'?");

workshop.ask.call(workshop, "Still no 'this'?");
```

- An arrow function doesn't have this keyword at all.
- Arrow functions have lexical this. Which means the this keyword inside arrow function is resolved by lexically to it's parents or parents of parent's scope. That is why the example on the left, the ask function's call site has workship.ask to it. So this inside of workshop object is 'Kyle'
- But interestingly example on the right, doesn't do the same because in it itself is a arrow funciton and this will resovle to undefined because it points to the global scope.

### class{}

### Prototypes

### "Inheritance" vs "Behavior Delegation" (OO vs OLOO)

# Prototypes

> Objects are built by "constructor calls" (via new)
> 

![JS%20Deep%20Foundations%20v3%20e69058f130fe4a2aade461c4b2ffda15/Untitled%2012.png](JS%20Deep%20Foundations%20v3%20e69058f130fe4a2aade461c4b2ffda15/Untitled%2012.png)

> A "constructor call" makes an object linked to it's own prototype
> 
1. Before Line 0 in the code:
    1. Look at the what's about the horizontal line.
        1. Circles represents *functions with properties* and Squares represents an unnamed object. 
        2. That circle is **Object(fuction)** and the unnamed box above the horizantal line is reference to an object that can be accessed by Object.prototype
            1. Similarly, this reference from Object.prototype is this unnamed object that holds some utilitiy methods(toString, valueOf etc) usually also has reference back to original **Object**.
            2. That reference back to original Object can be accessed by **Object.prototype.constructor**
    2. When reached Line 1:
        1. Workshop function is declared which is also a object.
        2. So this essentially Workshop object also has link to it's own unnamed object via it's prototype property. Thread of execution can reach to this object via **Workshop.prototype** and yet similarly also has *constructor* property that links back to original Workshop object (function)
        3. There is a hidden relationship formed between Workshop.prototype and Object.prototype. What is this relationshiop called? We will come back to that later.
    3. Skip to Line 4:
        1. Just like how we add a funtion to a regular object property, similarly we add ask function as a method to Workshop.prototype
    4. Skip to Line 8:
        1. The new keyword
            1. It creates a brand new empty object — empty box
            2. This empty object links to another object — Workshop.prototype
            3. this keyword points at that empty object
                1. It adds a teacher member to that object
            4. Returns this object which now has teacher property in it holds value of "Kyle" — That now holds the reference from deepJS identifier.
                1. It returns that this object when function doesn't return specifically.
                2. If function does have sepcific return, then't isn't it what we use it for regular function calls?
    5. Skip to Line 9:
        1. Same thing happens as in Line 8:
    6. Line 11:
        1. deepJS.ask(); when executes it doesn't have ask method on it.
        2. If JS engine doesn't find ask() method, it goes back to it's link — Workshop.prototype.
            1. Inside Workshop.prototype.ask() when you hit this.teacher this will point to this object (pointing deepJS) beacuse of it's call site.

### Dunder Prototypes

```jsx
function Workshop(teacher) {
  this.teacher = teacher;
}

Workshop.prototype.ask = function (question) {
  console.log(this.teacher, question);
};

var deepJS = new Workshop('Kyle');

deepJS.constructor === Workshop;
deepJS.__proto__ === Workshop.prototype;
Object.getPrototypeOf(deepJS) === Workshop.prototype; // https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/setPrototypeOf
```

1. As we already have discussed earlier, `deepJS` .. since it is invoked with new keyword upfront of Workshop(); call.
    1. It does all that is mentioned [above](JS%20Deep%20Foundations%20v3%20e69058f130fe4a2aade461c4b2ffda15.md).
    2. Now when thread of execution comes across the accessing of deepJS.constructor
        1. deepJS doesn't have construtor property on it.
        2. So JS engines searhces in deepJS.prototype
        3. In deepJS.prototype there's constructor property that is pointing back to Workshop
    3. So, Workshop is returned and when do === turns out true.
2. Now the nextline.. `deepJS.__proto__`
    1. This simply called dunder proto
    2. Same applies. Thread of execution doesn't find `__proto__` on deepJS object.
    3. It so looks for __proto__ on deepJS.prototype. Still there's nothing up there.
    4. Now using it's links, it will go to Object.prototype which will have __proto__ up there which is simply a getter function.
    5. That gets deepJS.__proto__ a `this` object
        1. this's call site is deepJS and context of this object is acquired from deepJS itself.
        2. That is equal ot Workshop.protoype
        

 

### Shadowing prototypes

```jsx
function Workshop(teacher) {
  this.teacher = teacher;
}

Workshop.prototype.ask = function (question) {
  console.log(this.teacher, question);
};

var deepJS = new Workshop('Kyle');

deepJS.ask = function (question) {
  this.__proto__.ask.call(this, question.toUpperCase());
};

deepJS.ask('Is this fake polymorphipsm?');
```

What if we can reap the benefits of classes or prototypal nature in completely different way?

### Class

### Prototypal

### Completely new

![JS%20Deep%20Foundations%20v3%20e69058f130fe4a2aade461c4b2ffda15/Untitled%2013.png](JS%20Deep%20Foundations%20v3%20e69058f130fe4a2aade461c4b2ffda15/Untitled%2013.png)

![JS%20Deep%20Foundations%20v3%20e69058f130fe4a2aade461c4b2ffda15/Untitled%2014.png](JS%20Deep%20Foundations%20v3%20e69058f130fe4a2aade461c4b2ffda15/Untitled%2014.png)

![JS%20Deep%20Foundations%20v3%20e69058f130fe4a2aade461c4b2ffda15/Untitled%2015.png](JS%20Deep%20Foundations%20v3%20e69058f130fe4a2aade461c4b2ffda15/Untitled%2015.png)

### Delegation: Design Pattern

Have to see Delegation: Design Pattern...

# Wrapping Up

Try out typl
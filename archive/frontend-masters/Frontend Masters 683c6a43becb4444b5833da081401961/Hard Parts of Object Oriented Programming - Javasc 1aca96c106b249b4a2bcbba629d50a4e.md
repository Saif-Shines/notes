# Hard Parts of Object Oriented Programming - Javascript

# Slides

[](https://static.frontendmasters.com/resources/2018-10-03-javascript-hard-parts-oop/javascript-hard-parts-oop.pdf)

# Object Oriented Paradigm

We do we even need a paradigm? 

A paradigm usually lays down an approach to organise our code in ways that more helpful and matainable to ourselves. Especially, in OOP, we bundle the data and functionality together to reach this goal.

# Object Creation

## Object Literal

```jsx
let user1 = {
  name: 'saif',
  score: 4,
  increment: function () {
    this.score++;
  },
};

user1.increment();
user1.score;
```

When list the properties under an object directly, so that when `user1` is declared, it has that bundle of data & functionality.

## Dot Notation

## `Object.create`

```jsx
let user2 = {};

user2.name = 'Jordan';
user2.score = 3;
user2.increment = function () {
  user2.score++;
};

user2;
user2.increment();
user2.score;
```

```jsx
let user2 = Object.create(null);

user2.name = 'Jordan';
user2.score = 3;
user2.increment = function () {
  user2.score++;
};

user2;
user2.increment();
```

## Creating Objects with Functions

```tsx
const user1 = createUser("Chris", 0);
const user2 = createUser("Sophia", 0);

function createUser(name, score) {
  let newUser: any = {};
  newUser.name = name;
  newUser.score = score;
  newUser.increment = function () {
    newUser.score++;
  };
  return newUser;
}

user1.increment(); // user1's own increment()
user2.increment();// user2's own increment(); why not only once?
console.table(user1, user2);
```

# Prototypes & new

## Prototype Walkthrough

```tsx
import { userInfo, storeInfo } from "./playground/types/utilities";

const userFnStore: storeInfo = {
  increment: function () {
    this.score++;
  },
  login: function () {
    console.info("You are logged in!");
  },
};

function createUser(name, score): userInfo {
  let newUser = Object.create(userFnStore);
  newUser.name = name;
  newUser.score = score;
  return newUser;
}

const user1 = createUser("Chris", 0);
const user2 = createUser("Sophia", 0);

user1.increment();
user2.increment();

console.table(user1);
console.table(user2);
```

<aside>
💡 **Do you see any problem with this approach?**

Perhaps, the most important thing that you must notice as a problem is, `increment()` is a functionality that being repeatedly stored. It demands a more memory.

Why not only once?

</aside>

<aside>
💡 Let’s understand `__proto__`

When you add `userFnStore` using `Object.create()` , the assigned identifier `newUser` will contain a link to to `userFnStore`. The `__proto__` , has a reference stored to it.

</aside>

## `this` & `new` keywords

These two keywords combined are trying to eliminate the manual apporach of using Object.create() to **proto** to manually link reusable function stores. At this point, we only know two things

1. `this` is going to helpful for us to refer to the reusable function store.
2. while `new` is will do the work of creating such a store and creating that link.

## Functions are both Objects & Functions

```jsx
function multiplyBy2(num) {
  return num * 2;
}

console.log((multiplyBy2.stored = 5));
console.log(multiplyBy2(3));
```

- The concept is very simple, in javascript when you declare a function, it’s not just a function everytime, but it’s a ********************************function & Object******************************** combo.
- The ****************************************************Function and Object Combo**************************************************** has a property like any object called `prototype`. It exists as a property on the Object part of this combo.
- Going forward, `new` keyword will actually return function object combo, where it’s `__proto__` will hold the reference (like with Object.create) to `prototype` that uses Function Store.

## new Keyword & Share Functions with prototype

```jsx
function UserCreator(name, score) {
  this.name = name;
  this.score = score;
}

UserCreator.prototype.increment = function () {
	this.score++;
};

UserCreator.prototype.login = function () {
  console.log("logged in!");
};

const user1 = new UserCreator("Saif", "6");

user1.increment(); // 7
```

1. Declare a function `UserCreator(..)`. This automatically is a function object combo, anyway.
2. The object part ships with a `prototype` object as property by default. We simply add `increment` and `login` functionality to it.
3. Declare `user1`
4. The `user1` will get a object in which `name` and `score` are assinged with `Saif` and `6` + `__proto__` object that points reference to `UserCreator.prototype`
    1. It is possible because `new` keyword automates few things.
    2. It invokes `UserCreator()` as a any other function. In the execution context, it creates a `this` object with `__proto__` in it. 
    3. We add data/functionality to `this` . For example, `[this.name](http://this.name)` and `this.score`
    4. The automatic return value is the this object that finally created. 

![Untitled](Hard%20Parts%20of%20Object%20Oriented%20Programming%20-%20Javasc%201aca96c106b249b4a2bcbba629d50a4e/Untitled.png)

<aside>
💡 There is a good reason why developers use Pascal Case to name functions. It helps other developers understand, “Oh!, I need to invoke this function with `new` keyword”.

Otherwise, not using `new` to invoke, would mean, `this` will refer to `window` or `global` in javascript. That’s a mess.

</aside>

# Scope & this

When `user1.increment()` is invoked, the `this.score++` is executed. One quick understanding might be to see `this` is always equal to `user1` at the time of invocation. 

- In functions that do not get invoked with `new` , the this points at global, which is pretty useless.
- But there’s profound use in functions that do use `new` keyword. This time you can maybe do `user20.increment()` and yet `this.score++` nicely only increments the data scope of user20.

## Common Scoping Issues

```jsx
function UserCreator(name, score) {
  this.name = name;
  this.score = score;
}

UserCreator.prototype.increment = function () {
	function add1(){
		this.score++;
	}
	add1();
};

UserCreator.prototype.login = function () {
  console.log("logged in!");
};

const user1 = new UserCreator("Saif", "6");

user1.increment(); // 7
```

Look at this example, within the `increment` definition, we have another function declaration `add1()`. Consider a case when it’s invoked, 

1. `user1.increment()` invoked
2. `add()` is invoked
3. `this.score++;` needs to be executed.
4. The `this` is now confused because this statement is part of `add()` but not `increment()`. If it were `increment()`, the `this` value would have `score` value as returned by `new UserCreator`
5. But currently `this.score++` comes as part of `add()` ! Javascript by design points at `window`
6. The `score` must be `undefined` there and doing a math `++` operation on it will return `NaN`. 

A better way to solve the above problem might by using arrow fuctions

```jsx
UserCreator.prototype.increment = function () {
    const add1 = () => {
      this.score++;
    };

    add1();
  };
```

The arrow function binds the `this` context to it’s parent callee `increment(..)`. That turns out to be `user1` .

## ES6 Class Keyword

```jsx
class UserCreator {
  constructor(name, score) {
    this.name = name;
    this.score = score;
  }

  increment() {
    this.score++;
  }
  login() {
    console.log("logged in!");
  }
}
```

# Default Prototype Chain

## Objects default `__proto__`

```jsx
const obj = {
  num: 3,
};

obj.num;
obj.hasOwnProperty("num");

Object.prototype; // hasOwnProperty : FUNCTION
```

![Untitled](Hard%20Parts%20of%20Object%20Oriented%20Programming%20-%20Javasc%201aca96c106b249b4a2bcbba629d50a4e/Untitled%201.png)

All the objects in Javascript have both function and object nature them as their identity. We often come across, extra functionality like `hasOwnProperty` or `isArray` . Where are these coming from?

The thread of execution tries to start looking up in `Object.prototype` object through `__proto__` reference. They happen to have these functions, and as developers they become useful to us. 

Even when we instatiate using `new` , the functionality doesn’t go away, every object there would have `__proto__` reference, so that JS looks up through a chain of them — Prototypal Chain.

# Subclassing with Factory Functions

![Untitled](Hard%20Parts%20of%20Object%20Oriented%20Programming%20-%20Javasc%201aca96c106b249b4a2bcbba629d50a4e/Untitled%202.png)

![Untitled](Hard%20Parts%20of%20Object%20Oriented%20Programming%20-%20Javasc%201aca96c106b249b4a2bcbba629d50a4e/Untitled%203.png)

## call & apply

```jsx
const obj = {
  name: 'saif',
  score: 6,
  increment: function(){
    this.score++;
    console.log(this.score)
  }
};

const otherObj = {
  name: 'rev',
  score: 10
}

obj.increment(); // 7 -> this = obj -> obj.increment()

obj.increment.call(otherObj); // 11 -> this = otherObj -> otherObj.increment()
```

```jsx
const obj = {
  name: 'saif',
  score: 6,
  increment: function(x){
    this.score++;
    console.log(this.score)
  }
};

const otherObj = {
  name: 'rev',
  score: 10
}

obj.increment(); // 7 -> this = obj -> obj.increment()

obj.increment.call(otherObj, 3); // 11 -> this = otherObj -> otherObj.increment(3)
```

<aside>
💡 Ths `call` and `apply` functions will help us control the `this` context within a function’s execution.

</aside>

The `call` method, replaces the `this` reference from `obj` at `obj.increment()` to `otherObj` at `obj.increment.call(otherObj)`. 

# Subclassing with new & call

![Untitled](Hard%20Parts%20of%20Object%20Oriented%20Programming%20-%20Javasc%201aca96c106b249b4a2bcbba629d50a4e/Untitled%204.png)

![Untitled](Hard%20Parts%20of%20Object%20Oriented%20Programming%20-%20Javasc%201aca96c106b249b4a2bcbba629d50a4e/Untitled%205.png)

# Subclassing with class, extends, & super

![Untitled](Hard%20Parts%20of%20Object%20Oriented%20Programming%20-%20Javasc%201aca96c106b249b4a2bcbba629d50a4e/Untitled%206.png)

![Untitled](Hard%20Parts%20of%20Object%20Oriented%20Programming%20-%20Javasc%201aca96c106b249b4a2bcbba629d50a4e/Untitled%207.png)
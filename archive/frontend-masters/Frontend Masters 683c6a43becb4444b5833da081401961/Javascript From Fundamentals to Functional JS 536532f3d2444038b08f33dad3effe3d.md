# Javascript: From Fundamentals to Functional JS

# Introduction

> Learn to measure your learning curve.
> 

### What is functional programming?

In simple words where you write functions as verbs. One good analogy that teacher brings in is that

1. In Object Oriented Programming, if you are looking to build a house, you look doors, windows, people, stuff like that.
2. In Function Programming, you look at opening, closing, watching kind of stuff. 

We use a lot of funtions and software design pattern in the same way.

[Fundamentals to Functional Day 1 v2](https://slides.com/bgando/f2f-final-day-1#/3)

[Fundamentals to Functional Day 2 v2](https://slides.com/bgando/f2f-final-day-2)

# Objects

1. Property Access
2. Dot vs Bracket
3. Destructuring
4. Nesting + Loops
5. Nesting + Destructuring

### Assignment with Dots

```jsx
var person = {};

person.name = "Mrs. White";

var person = {
	name: "Mrs. White" // "name" : "Mrs. White" is also valid.
};
```

Anything that uses a dot is an Object.

1. Primitive Values passed by Values
    1. undefined
    2. string
    3. number
    4. null
    5. boolean
    6. symbol
2. Non Primitive Values are passed by Reference
    1. functions
    2. Objects
    3. Arrays

See this piece of code,

```jsx
var person = [];

person.name = "Mrs. White";

var who = person.name;

person[0] = "I was not in the room";
```

1. We know person is assigned with `[]` . This is an Array. But of of Type `object`.
2. So it can be assigned with `"Mrs. White"` . But at adds a property to `person` .
    - `{ "name":"Mrs. White" }`
    - Now when you are assigning who with [`person.name`](http://person.name) then person.name evaluates to `"Mrs. White"` .
3. Now when you try to access `"I was not in the room"` via
    - `person.0` because, `0` is a `number` here. That is why dot operator doesn't work. Parser tries to parse it and throws an error. But JS will make coerce it into string. Yet Parser will through Syntax errors.
    - `person[0]` works.

### Aha! moments

1. The dot notation when assigning a value to property, the property name is coerced into a string.
    - In [`person.name`](http://person.name) the name is coerced into string then statement evaluates. but parser will thro
    - That means, if you want to add another property to the object using bracket operator, you shouldn't use `person[name]` but actuaally `person["name"] = "saif`";
2. When do you decide to use Array or Objects to store data?
    1. When data is similar then use Arrays.

### Code Work till now

```bash
'??'
[
  { type: 'lasers', location: 'lab' },
  { type: 'angry cats', location: 'balcony' },
  { type: 'dish soap', location: 'kitchen' }
]
[]
'Miss Johnson'
2
'^^^^^^^^^ Total Object ^^^^^^^^^ '
{
  murderer: '??',
  weapons: [
    { type: 'lasers', location: 'lab' },
    { type: 'angry cats', location: 'balcony' },
    { type: 'dish soap', location: 'kitchen' }
  ],
  name: [ 'Miss Johnson', 'Miss Scharlet' ]
}
```

**output**

```jsx
var game = {};

game.murderer = '??';

game['weapons'] = [
  { type: 'lasers', location: 'lab' },
  { type: 'angry cats', location: 'balcony' },
  { type: 'dish soap', location: 'kitchen' },
];

// Ways to pushing into an Array. When one of the properties is array itself.
game.name = [];
game.name[0] = 'Miss Johnson';
game.name.push('Miss Scharlet');

('^^^^^^^^^ Total Object ^^^^^^^^^ ');
game;
```

## The Rules

Dot Notation will work for:

1. strings

Bracket Notation will work for:

1. strings
2. numbers
3. variables
4. weird characters
5. expressions

❓If dot notation doesn't exactly support everything and brack notation supports everything to assign values to the property keys, then why would we ever use it?

👨‍🍳Because, to save characters. As simple reason as that.

## ES6 Destructuring

### Destructuring declaration using Array literal

```jsx
var
let [a,b] = [true, false];
const [a, b] = [true, false];
a; // true
b; // false
```

What is the difference between the normal way of assigning values to the variable and the Desstructured way of doing it?

👨‍🍳Looks like they reduce the number of characters and improve readability. That's all.

### Destructuring Declaration using Object literal

```jsx
var { first, second } = { first: 1, second: 2 };
first; // 1
second; // 2

/*Real time example*/
var obj = { first: 1, second: 2 };
var { first, second } = obj;
first; // 1
second; // 2
```

In case of Objects, they should have same property names.

Some more examples,

```jsx
('Omni certain values');
var [a, , b] = [1, 2, 3, 4];
a;
b;

('Combine with spread/rest operator');
var [c, ...d] = [5, 6, 7, 8, 9, 20];
c;
d;

('// Swap variables easily without temp');
var [e, f] = [1, 2];
e;
f;
[e, f] = [f, e];
e;
f;

'// Advance Deep arrays';
var [a, [b, [c, d]]] = [1, [2, [[[3, 4], 5], 6]]];
a;
b;
c;
c;
```

### Destructuring the special way

```jsx
 
// Task: Destructure this Object into 2 variables holding one color each from the above object.

// Traditionally
var [color1, color2] = [suspects[0].color, suspects[1].color];

// More intuitive
var [{color: color1},{color: color2}] = suspects;

```

# List Transformations

### Differences of adding methods to an object

```jsx
function CreateSuspectObjects(name){
  return{
    name: name;
    color: name.split(' ')[2],
    speak: function(){ console.log("My name is ", name)};
  };
};

var suspects = ['Miss Scarlet', 'Colonel Mustard', 'Mr. Whilte'];

// ES6
function CreateSuspectObjects(name){
  return{
    name: name;
    color: name.split(' ')[2],
    speak(){ console.log("My name is ", name)};
  };
};

var suspects = ['Miss Scarlet', 'Colonel Mustard', 'Mr. Whilte'];
```

### Hydrating

It is a way of converting the some data as in primitive types into objects.

```jsx
function createSuspectObjects(name) {
  return {
    name: name,
    color: name.split(' ')[1],
    speak() {
      console.log('My name is ', name);
    },
  };
}

var suspects = ['Miss Scarlet', 'Colonel Mustard', 'Mr. White'];

var suspectObjects = [];

for (suspect of suspects) {
  suspectObjects.push(createSuspectObjects(suspect));
}

suspectObjects;
```

# .forEach() function

### Looping with _.each / .forEach

1. Traditional for loop is prone to errors and it is hard to write or even read.
2. To solve this problem we have two alternatives,

```jsx
// Here are two types of looping instead of traditional for

// Number #1

_.each(['observatory','ballroom','library'], function(value,index,list){
	// some logic
};
```

1. The array inside is called **iteratee.**
2. The function is called the **iterable**

Another Alternative #2

```jsx
// This is something that I have seen before

['observatory','ballrooom','library'].forEach(function(value, index, list){...});

var value, index, list;

['observatory', 'ballrooom', 'library'].forEach(function (
  value,
  index,
  list
) {
  console.log(`value:${value}`);
  console.log(`index:${index}`);
  console.log(`list:${list}`);
});

// output
'value:observatory'
'index:0'
'list:observatory,ballrooom,library'
'value:ballrooom'
'index:1'
'list:observatory,ballrooom,library'
'value:library'
'index:2'
'list:observatory,ballrooom,library'
```

Example

```jsx
function CreateSuspectObjects(name) {
  return {
    name: name,
    color: name.split(' ')[1],
    speak() {
      log('my name is ');
    },
  };
}

var suspects = ['Miss Scarlet', 'Colonel Mustard', 'Mr. Whilte'];

var suspectList = [];

_.each(suspects, function (name) {
  let suspectObj = CreatSuspectObjects(name);
  suspectList.push(suspectObj);
});
```

ℹ️ The underscore is is from underscore library.

1. Generally in the libraries like for example loadash. The underscore is not a special thing. It's just somewere in the code: `var _ = function() { some logic }`

### Let's create _.each()

```jsx
var _ = {};

_.each = function(list, callback){
	if(isArray(list) == "array"){
		for(item of list){
				callback(item, list); // We are passing the list to make method more generic
			}
	}else{
		for(item in list){
			callback(list[item]);
		}
	}
}
```

# .map() Function.

- The key difference between .each() and .map() is that, .each() doesn't return anything .map() returns an array.
- The callback method inside _.map() will return only an object. If there is not return statement inside of callback then an undefined is returned.

```jsx
_.map([1,2,3], function(v,i,list){console.log(v)})
```

Exercise

```jsx
var _ = {};

_.map = function (list, interatee) {
  let result = [];
  if (Array.isArray(list)) {
    for (let item of list) {
      result.push(interatee(item));
    }
  } else {
    for (let key in list) {
      result.push(interatee(list[item]));
    }
  }
  return result;
};

_.map([1, 2, 3], function (num) {
  return num * 3;
});
```

# .filter() Function

In this case, _.map() and _.filter() are a bit similar, the main important difference here is , in _`.map()` , it takes a list and a callback and map function returns an array of all the returnees by callback. However, in `_.filter()` , it returns an array of the items but on which returnees are *true* by callback.

```jsx
const videoData = [
  {
    name: 'Miss Scarlet',
    present: true,
    rooms: [
      { kitchen: false },
      { ballroom: false },
      { conservatory: false },
      { 'dining room': false },
      { 'billiard room': false },
      { library: false },
    ],
  },
  {
    name: 'Mrs. White',
    present: false,
    rooms: [
      { kitchen: false },
      { ballroom: false },
      { conservatory: false },
      { 'dining room': false },
      { 'billiard room': false },
      { library: false },
    ],
  },
  {
    name: 'Reverend Green',
    present: true,
    rooms: [
      { kitchen: false },
      { ballroom: false },
      { conservatory: false },
      { 'dining room': false },
      { 'billiard room': false },
      { library: false },
    ],
  },
  {
    name: 'Rusty',
    present: false,
    rooms: [
      { kitchen: false },
      { ballroom: false },
      { conservatory: false },
      { 'dining room': false },
      { 'billiard room': false },
      { library: false },
    ],
  },
  {
    name: 'Colonel Mustard',
    present: true,
    rooms: [
      { kitchen: false },
      { ballroom: false },
      { conservatory: false },
      { 'dining room': false },
      { 'billiard room': false },
      { library: false },
    ],
  },
  {
    name: 'Professor Plum',
    present: true,
    rooms: [
      { kitchen: false },
      { ballroom: false },
      { conservatory: false },
      { 'dining room': false },
      { 'billiard room': false },
      { library: false },
    ],
  },
];

var _ = {};

_.each = function (list, callback) {
  if (Array.isArray(list) == true) {
    for (item of list) {
      callback(item, list); // We are passing the list to make method more generic
    }
  } else {
    for (item in list) {
      callback(list[item]);
    }
  }
};

_.filter = function (list, callback) {
  var result = [];
  _.each(list, function (item) {
    if (callback(item)) result.push(item);
  });
  return result;
};

_.filter(videoData, function (obj) {
  return obj.present;
});
```

Output

```bash
[
  {
    name: 'Miss Scarlet',
    present: true,
    rooms: [
      { kitchen: false },
      { ballroom: false },
      { conservatory: false },
      { 'dining room': false },
      { 'billiard room': false },
      { library: false }
    ]
  },
  {
    name: 'Reverend Green',
    present: true,
    rooms: [
      { kitchen: false },
      { ballroom: false },
      { conservatory: false },
      { 'dining room': false },
      { 'billiard room': false },
      { library: false }
    ]
  },
  {
    name: 'Colonel Mustard',
    present: true,
    rooms: [
      { kitchen: false },
      { ballroom: false },
      { conservatory: false },
      { 'dining room': false },
      { 'billiard room': false },
      { library: false }
    ]
  },
  {
    name: 'Professor Plum',
    present: true,
    rooms: [
      { kitchen: false },
      { ballroom: false },
      { conservatory: false },
      { 'dining room': false },
      { 'billiard room': false },
      { library: false }
    ]
  }
]
```

# Functions In-Depth

### Projecting

It is nothing but a fancy keyword for taking data from one data structure and converting into another. One of the Exercises, that I went through projecting was

1. Get an *Array of Objects*. Where each object has it's on property.
2. `_.filter()` this array, so that you'd get only objects in an array were callback returns `true`.
3. `_.map()` it. That means all the results of a map will be stored in an array. I wrote logic that callback in this map() will only return value of passed object's property.
4. That means it will give you Array of those values.
5. Array of Objects at the beginning(#1) was a kind of Data Structure, and Array of Values was another. Hence I projected it.

### The Spread Operator

Looks like this operator is a unary operator.

```jsx
function createTuple(a,b,c,d){
	console.log(arguments);
	return [[a,c],[b,d]];
}

createTuple('It','be','could','anyone','no one')
```

```bash
[Arguments] {
  '0': 'It',
  '1': 'be',
  '2': 'could',
  '3': 'anyone',
  '4': 'no one'
}
[ [ 'It', 'could' ], [ 'be', 'anyone' ] ]
```

- The `arguments` was used put all the values passed to parameters into a **pseudo array.**
- It is called pseudo array mainly because the array that arguments returns will not have any array related properties to it.
- In the above code:
    - `arguments` would be `['It','be','could','anyone','no one']` with inability to having array properties on it.
    - `[[a,c],[b,d]]; // [['It','could'],['be','anyone']]`
    - Notice that `'no one'` is totally omitted and access to it can only be gained by `arguments`

```jsx
function createTuple(a,b,c,...d){
 return [[a,c],[b,d]];
}

createTuple('It','be','could','anyone','no one'); 
// [['It','could'],['be',['anyone','no one']]]
```

- That `...` is called **spread** operator.
- It will pick up the remaining elements of the array and create a subarray as show in output(in comment).

### On the flip side

```jsx
var createTuple = (a, b, c, d) => {
  console.log(arguments);
  return [
    [a, c],
    [b, d],
  ];
};

createTuple('It', 'be', 'could', 'anyone', 'no one');
```

**Output:** `ReferenceError: arguments is not defined`

1. First thing you notice is it is not a regular function. It's a arrow function.
2. The behaviour is changed. The `arguments` keyword is no longer accessible inside a arrow function.

### Default Parameters

```jsx
const add = function(a, b=2){
	console.log(arguments);
	return a+b;
};

add(3); // 5 ??
```

- Output
    
    ```bash
    [Arguments] { '0': 3 }
    5
    ```
    
- The same code in ES5
    
    ```jsx
    const add = function(a, b){
    	b = b || 2;
    // Teh OR operator just sees the firstone, if b evaluates to true then it retuns. ES5 devs used this as a hackaround to pass default values.
    	return a+b;
    };
    
    add(3); // 5 ??
    ```
    
1. I probably understand default parameters. If there is no argument that is passed explicitly, it defaults it value that is assigned to the parameter.
2. But there are other interesting thing to note:
    - In output, you will see `arguments` didn't capture the default parameter.
    - It is because, `arguments` only cares about the as it says **arguments** passed explicitly. But not the parameters.
    - Paramenters and Arguments are different.
        
        ➡️Arguments are part of local scope and it's execution context. That's all.
        

### ES5 way converting Pseudo Array to Normal Array

```jsx
const constructArr = function(){
const arr = Array.prototype.slice.call(arguments);
// ['was','it','in']
arr.push('the billbords room');
// ['was','it','in','the billbords room']
return arr.join(' '); // returns a string "was it in the billbords room"
};
constructArr('was','it','in');
```

- Now that it is told that `arguments` only returns a pseudo array it doesn't have any array properties on this object.
- Then we need to explicitly add them to it. So we take it from **Array** object that available for us by JS.
- The way we should do  it is `const arr = Array.prototype.slice.call(arguments);`

### ES6 way converting Pseudo Array to Normal Array

```jsx
const constructArr = function(){
const arr = Array.from(arguments);
// ['was','it','in']
arr.push('the billbords room');
// ['was','it','in','the billbords room']
return arr.join(' '); // returns a string "was it in the billbords room"
};
constructArr('was','it','in');
```

# Scope

```jsx
var ACTUAL = null;
var fn = function () {
  var where = 'outer';
  {
    let where = 'inner';
  }
  ACTUAL = where;
  return 1;
};
fn();
ACTUAL; // 'outer'
```

- The `ACTUAL` will return `'outer'`
- If `let` is changed to `var` or even just where, then `ACTUAL` will return `'inner'` .

```jsx
ACTUAL = null;

{
  var sameName = 'outer';
  var fn = function () {
    let sameName = 'inner';
    ACTUAL = sameName;
  };
  fn();
  ACTUAL; // 'inner'
}
```

- The above snippet is even more interesting. Firstly before you really read and expect the value of **ACTUAL** see the differences in the above both snippets.
    1. The block scope is present in the both the places.
    2. One has scope created within the function execution context and first snippet has just the block scope with *where* declared.
- See where the execution is happened that will cause the value expected to be assigned to variable *ACTUAL*
    1. In the first case, no execution happens because `let where = 'inner';` is under plain block within **{}**
    2. In the 2nd case, same block is inside of `function(){}` . Then value is reassigned. It's called function scope.

```jsx
ACTUAL = null;

{
  var sameName = 'outer';
  var fn = function () {
    var sameName = 'inner';
  };
  fn();
  ACTUAL = sameName;
  ACTUAL; // outer
}
```

- The `var sameName = 'inner';` is a declaration statement. it means in the function scope it creates a new memory space and assigns it `'inner'` .
- But when `ACTUAL = sameName` happens, the memory space labelled `ACTUAL` is assigned by value of sameName. Here, the `sameName` in it's own scope is `'outer'`
- So ACTUAL returns 'outer'.

```jsx
ACTUAL = null;

{
  var sameName = 'outer';
  var fn = function () {
     sameName = 'inner';
  };
  fn();
  ACTUAL = sameName;
  ACTUAL; // inner
}
```

- This is conjuction to above snippet but I removed the declaration keyword `var`.
- In that case, inside the function scope, an re-assignment is happening. It is re-assignment because, JS looks up for memory name labelled `sameName` and assigns it '`inner`'
- So ACTUAL lookup gives `'inner'`

```jsx
var ACTUAL = null;
{
var fn = function(){
	var innerCounter = innerCounter || 10;
	innerCounter = innerCounter + 1;
	ACTUAL = innerCounter;
};
fn();
ACTUAL; // 11
fn();
ACTUAL; // 11
}
```

🏄‍♂️Here the very important learning point it is, when fn() is called the second time, the function completely for all of it's previous values stared in the memory labels.

```jsx
var ACTUAL = null;
{
var outerCounter = 10;

var fn = function(){
	outerCounter = outerCounter + 1;
	ACTUAL = outerCounter;
};

fn();
ACTUAL; // 11
fn();
ACTUAL; // 12

}
```

🏄Now, in this case `outerCounter` is not declared. It is referenced to upper scope and evaluated to 11. But when `fn()` is invoked another time, still there's not new declaration. So `outerCounter` is referenced again. This time this reference has 11. That will be evaluated to 12.

# Callbacks

- I already know how to pass functions around. Mostly they don't have arguments when they need be passed.
- But what if I need pass arguments to the functions? or in other words, callbacks?

```jsx
const ifElse = (condition, isTrue, isFalse, param) =>{
	return condition ? isTrue(p) : isFalse(p);
}

ifElse(true, fn1,fn2, 'Hi');

// 'condition' is Boolean
// 'isTrue' or 'isFalse' are functions passed by reference. Those simply console log.

// The params is the part where you'd tend to pass arguments.
```

### Mulitple Arguments

```jsx
const ifElse = (condition, isTrue, isFalse, ...args) =>{
	console.log(args);// ['Hi','BYE','HOLA']
	return condition ? isTrue(args) : isFalse(args);
}

ifElse(true, fn1,fn2, 'Hi','BYE','HOLA');
```

- In ES5, developers used to use `arguments` .
    
    ```jsx
    const ifElse = (condition, isTrue, isFalse) =>{
    	const args = [].slice.call(arguments,3);
    	return condition ? isTrue.apply(this,args) : isFalse.apply(this,args);
    };
    
    const logTrue = (msgs) => {console.log(msgs);};
    const logFalse = (msgs) => {console.log(msgs);};
    
    ifElse(true,logTrue,logFalse);
    ```
    

# Functional Utilities

1. Currying
2. Composition

## Currying

This gives developer ability to call the a function multiple times with different arguments. Or A function is said to be curried when it's given capabilites to call function multiple times with different arguments

- The curry method should return  a function.

```jsx
var abc = function(a, b, c) {
  return [a, b, c];
};
 
var curried = _.curry(abc);
 
curried(1)(2)(3);
// => [1, 2, 3]
 
curried(1, 2)(3);
// => [1, 2, 3]
 
curried(1, 2, 3);
// => [1, 2, 3]
 
// Curried with placeholders.
curried(1)(_, 3)(2);
// => [1, 2, 3]
```

## Composition

A function is said to be composed if when given in seperate arguments with conjuctive function calls, it takes return value from other function call and passed as an argument to next function call.

```jsx
const consider = (name) => {
	return `I think it could be... ${name}`;
};

const exclaim = (statement) => {
	return `${statement.toUpperCase()}!`;
}

const blame = _.compose(consider,exclaim);

blame('you'); // I think it could be... YOU!
```

# Advanced Scope: Closure

Here is a very tricky example,

```jsx
const myAlert = () => {
  const x = 'Help! I found a clue!';
  let count = 0;
  const alerter = () => {
    console.log(`${x} ${++count}`);
  };
  console.log(count);
  return alerter;
};

const funcAlert = myAlert();
const funcAlert2 = myAlert();
funcAlert();
funcAlert();
funcAlert2();
funcAlert2();
myAlert();
```

1. I don't even know how to articulate this. But you can see that after **myAlert** is defined futher lines indicate function calls.
2. The line, `const funcAlert = myAlert();` is nothing but, firing **myAlert()** function. And it returns just the **alerter** definition. At this point **x** is *'Help! I think I found a clue!'*
3. Next, `const funcAlert2 = myAlert();` firing **myAlert()** function. And it returns just the **alerter** definition. At this point **x** is *'Help! I think I found a clue!'*
4. `funcAlert()` is invoked ➡️ *'Help! I found a clue! 1'*
5. `funcAlert()` is invoked ➡️ *'Help! I found a clue! 2'*
6. `funcAlert2()` is invoked ➡️ *'Help! I found a clue! 1'*
7. `funcAlert2()` is invoked ➡️ *'Help! I found a clue! 2'*

📌 The important point to note here is funcAlert() and funcAlert2() are completely two different spaces in the memory. That is why count is incremented separately for each consequent invocations.

### Example 1

```jsx
const newClue = (name) => {
  const length = name.length;
  return (weapon) => {
    let clue = length + weapon.length;
    return !!(clue % 1);
  };
};

const didGreenDoItWithA = newClue('Green');
didGreenDoItWithA('candlestick');
didGreenDoItWithA('lead pipe');
```

### Example 2

```jsx
function countClues() {
  var n = 0;
  return {
    count: function () {
      return ++n;
    },
    reset: function () {
      return (n = 0);
    },
  };
}

counter = countClues();
counter.count();
counter.count();
```

## The Recipe of Closure

![Bianca's Deck: 4.8](Javascript%20From%20Fundamentals%20to%20Functional%20JS%20536532f3d2444038b08f33dad3effe3d/Untitled.png)

Bianca's Deck: 4.8

![Javascript%20From%20Fundamentals%20to%20Functional%20JS%20536532f3d2444038b08f33dad3effe3d/Untitled%201.png](Javascript%20From%20Fundamentals%20to%20Functional%20JS%20536532f3d2444038b08f33dad3effe3d/Untitled%201.png)

### Famous Example

```jsx
const makeTimer = () => {
  let elapsed = 0;
  const stopwatch = () => { return elapsed; };
  const increase = () => elapsed++;
  setInterval(increase, 1000);
  return stopwatch;
}

let timer = makeTimer();

timer(); // will not return 0
```

- When `makeTimer()` is invoked and function is assigned to timer, observe that `setInterval()` is also invoked
- So, `timer();` // 22* seconds

### Example Currying

Expectation

```jsx
var abc = function(a,b){
	return [a,b];
};

var curried = _.curry(abc);

curried(1)(2);
// [1,2]
```

Implementation Just Basics

```jsx
const curry = (fn) => {
  return (arg) => {
    return (arg2) => {
      return fn(arg, arg2);
    };
  };
};
```

### Example Compose

Expectation

```jsx
const consider = (name) => {
	return `I think it could be... ${name}`;
};

const exclaim = (statement) => {
	return `${statement.toUpperCase()}!`;
}

const blame = _.compose(consider,exclaim);

blame('you'); // I think it could be... YOU!
```

Implementation: Basic Level

```jsx
const compose = (fn, fn2) =>{
  return (arg) => {
    const result = fn2(arg);
    return fn(result);
  }
}
```

# Wrapping Up

You are the killer.
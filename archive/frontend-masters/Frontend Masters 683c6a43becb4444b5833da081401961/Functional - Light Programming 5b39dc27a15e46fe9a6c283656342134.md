# Functional - Light Programming

[](http://static.frontendmasters.com/resources/2019-05-06-functional-light-v3/functional-light-v3.pdf)

# Introduction

- Most of the resources out there seemed to kyle like top to buttom approach assuming reader already know all the processes.
- This course from Kyle is actually a bottom up approach to learn and under functional programming.

[](http://static.frontendmasters.com/resources/2019-05-06-functional-light-v3/functional-light-v3.pdf)

## Imperative vs Declarative

- Your code comment should not duplicate the narrative of what code is doing. Generally focuses on "Why"
- Imperative programming is a paradigm of what programmers focus is  on "How" within the code.
- Declarative programming is a pradigm which instead focuses on "What" and "Why"
- Imperative and Declarative's are nnot absolutes. They are always relative to programmers existing experience. It's like a specturm.

## Provable

It's just Math. You don't know math? That's okay.

> Best code is, that Less to Read.
> 

# Function Purity

## Functions vs Procedures

### Procedure

```jsx
function addNumbers(x = 0, y = 0, z = 0, w = 0) {
  var total = x + y + z + w;
  console.log(x, y, z, w);
  console.log(total);
}

function extraNumbers(x = 2, ...args) {
  console.log(x, args);
  return addNumbers(x, 40, ...args);
}

extraNumbers(); // 42

extraNumbers(3, 8, 11, 10, 34); // 62 -> 10 & 34 are ignored
```

1. Functional programming doesn't mean just having `function` keyword in it.
2. In the above examples do you think `addNumbers` or the `extraNumbers` are functions? 
    1. `addNumbers` returns `undefined` . So it is not a function. It is otherwise called a procedure.
    2. `extraNumbers` returns `addNumbers` procedure call and yet doesn't return a value. So `extraNumbers` is still a procedure.

- I always didn't understand how spread operator works in arguments. I think I got more clarity now.
- Look at two function calls of `extraNumbers`
    - One has no arguments and he other has 4 arguments.
        - When thread of executions weaves in `extraNumbers` , x parameter will identify 2 as default value. Yet `args` would be empty array.
        - When thread of execution weaves in `addNumbers`
            - x parameter identifies as 2 and y paramber will identify to 40 as per the arguments are passed.
            - Notice that **z** and **w** are remaining. This is what essentially that happens
                - `[z,w]=[]` empty array is `...args` . Essentially destructuring happens.
                - At this same time when `extraNumbers(3,8,11,10,34)` is called..
                    - `[z,w]=[8,11,10,34]` destructured assignment happens so that `...args` will have z = 8 and w = 11 and remaining are just left out.

### Function

```jsx
function tuple(x, y) {
  console.log(x + 1, y);
  return [x + 1, y - 1];
}

var [a, b] = tuple(...[5, 6]);

a; // 6
b; // 5
```

### Spirit of a function

- If there were a function defined with `function` keyword and just returns 40. Does that mean anything?
- Do you remember the math? $f(x) = 2x^2+3$
    - x is an input and f(x) is output.
    - If you plot the graph,
        - It gives a parabola. That means something. Something semantic.
        
        ![Functional%20-%20Light%20Programming%205b39dc27a15e46fe9a6c283656342134/Untitled.png](Functional%20-%20Light%20Programming%205b39dc27a15e46fe9a6c283656342134/Untitled.png)
        

- Infact, x and y have a relationship.
- functions take in x and give out y. They share a relationship.

> Function: the semantic relationship between input and computed output
> 

```jsx
function shippingRate(size, weight, speed){
	return ((size+1) * weight) + speed;
}
```

Ask yourself. *How much of you write function or procedure?*

1. Having no input is also a input. Still a procedure.
2. Returning undefined is also an valid output. Still a procedure.
3. Having semantic relationship completes a procedure to a function.

## Side Effects

1. I / O - console, files, etc
2. Database Storage
3. Network Calls
4. DOM
5. Timestamps
6. Random Numbers
7. CPU Heat
8. CPU Time Delay
9. etc

 

![Functional%20-%20Light%20Programming%205b39dc27a15e46fe9a6c283656342134/Untitled%201.png](Functional%20-%20Light%20Programming%205b39dc27a15e46fe9a6c283656342134/Untitled%201.png)

> In JavaScript, there is not such thing as function. It is the function call that matters. It is the characteristics of function call that matters. Are there direct inputs and outputs with sematic relationship?
> 

> No such thing as "no side effects" Avoid them where possible, make them obvious otherwise.
> 

In a absolute sense

1. This looks like a regular code that we write.
2. The `shippingRate()` function takes in no arguments, it doesn't explicitly return anything it does a valide computation and updates value identified to `rate` , it has a sematic meaning like `shippingRate` . But it is not a true function.
3. Let's evolve our definition.
    1. A function takes in valid arguments and return value outputs.
    2. Doesn't produce any sideeffects as in changing the variable outside of it's execution context.
    3. Simple does what it does inside of it's context explicitly. Is **close to** to function.

> Simple reason. We won't see the true value of functional programming if keep on encouraging the side effects.
> 

Below is true function.

```jsx
function shippingRate(size,weight,speed){
	return ((size+1)* weight)+speed;
}

var rate;

rate = shippingRate(12,4,5); // 57

rate = shippingRate(8,4,6) // 42
```

## Pure Functions

![Functional%20-%20Light%20Programming%205b39dc27a15e46fe9a6c283656342134/Untitled%202.png](Functional%20-%20Light%20Programming%205b39dc27a15e46fe9a6c283656342134/Untitled%202.png)

![Functional%20-%20Light%20Programming%205b39dc27a15e46fe9a6c283656342134/Untitled%203.png](Functional%20-%20Light%20Programming%205b39dc27a15e46fe9a6c283656342134/Untitled%203.png)

Such a wonderful state of art is pure functions!

Very very hard to put down but let me try out. Pure functions are those when function calls are made to it, we are maximum certain about he variables involved not producing too many side effects.

### Impure

```jsx
var SomeAPI = {
  threshold: 13,
  isBelowThreshold(x) {
    return x <= SomeAPI.threshold;
  },
};

var numbers = [];

function insertSortedDesc(v) {
  SomeAPI.threshold = v;
  var idx = numbers.findIndex(SomeAPI.isBelowThreshold);
  if (idx == -1) {
    idx = numbers.length;
  }
  numbers.splice(idx, 0, v);
}

insertSortedDesc(3);
numbers; // [3]
```

Other way where you can make impurity obvious

![Functional%20-%20Light%20Programming%205b39dc27a15e46fe9a6c283656342134/Untitled%204.png](Functional%20-%20Light%20Programming%205b39dc27a15e46fe9a6c283656342134/Untitled%204.png)

### Containing Impurity

```jsx
var SomeAPI = {
  threshold: 13,
  isBelowThreshold(x) {
    return x <= SomeAPI.threshold;
  },
};

var numbers = [];

function getSortedNums(nums, v) {
  var numbers = nums.slice();
  insertSortedDesc(v);
  return numbers;

  function insertSortedDesc(v) {
    SomeAPI.threshold = v;
    var idx = numbers.findIndex(SomeAPI.isBelowThreshold);
    if (idx == -1) {
      idx = numbers.length;
    }
    numbers.splice(idx, 0, v);
  }
}

numbers = getSortedNums(numbers, 3);
numbers = getSortedNums(numbers, 5);
numbers;
```

6 Steps to Contain Impurity when it is impossible to contain impurity.

![Functional%20-%20Light%20Programming%205b39dc27a15e46fe9a6c283656342134/Untitled%205.png](Functional%20-%20Light%20Programming%205b39dc27a15e46fe9a6c283656342134/Untitled%205.png)

# Argument Adapters

## Shapes

- Kyle talks about unary, binary ane enary functions
- The functions those take in single argument are unary, two arguments are binary and more thant hat re enary. In all the cases they return single output.
- Most of the functional programmers are obsessed with the arguments because they are legos shape. If the shape doesn't fit together, developer can't use them together.

> A higher order functions, simply put it either receives one or more functions or either returns one or more functions.
> 

```jsx
function unary(fn) {
  return function one(arg) {
    return fn(arg);
  };
}

function binary(fn) {
  return function two(arg1, arg2) {
    return fn(arg1, arg2);
  };
}

function f(...args) {
  console.log(args);
  return args;
}

var g = unary(f);
var h = binary(f);

g(1, 2, 3, 4); // [1]
h(1, 2, 3, 4); //
```

# Point Free

You need to refer to the slides. Inshort these are funtions which have same shapes so that you can call the function without passing any arguments

# Closure

> If you are using closure in functional programminng, you have to make sure that you are nnot mutating the variables that you are closing over.
> 

### Pure Funtion call

A function call is pure, if it has referential transperancy. Referntial transperannce is when a function call is replaced by return value, the entirity of program runs as it is.

# Composition

# Immutability

[Immutable.js](https://immutable-js.github.io/immutable-js/)

[mori](https://swannodette.github.io/mori/)

# Recursion

# List Operations

# Transduction

# Data Structure Operations

# Async

# Functional JS Utils

# Wrapping Up
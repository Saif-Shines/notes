# A Practical Introduction to Algorithms

[Introduction to Algorithms](http://slides.com/bgando/intro-to-algorithms)

![A%20Practical%20Introduction%20to%20Algorithms%206565611634d44f76b5f74b529f620a07/Untitled.png](A%20Practical%20Introduction%20to%20Algorithms%206565611634d44f76b5f74b529f620a07/Untitled.png)

# The Two complexities

## Space Complexity

It mostly tries to answer How much memory is used?

## Time Complexity

It is answer to the question How many primitive operations are executed

We generally try to measure both of these to our algorithms with respect to input size and assuming worst case scenarios. Another piece of what I want to take notes that made a lot of sense to me is why Time is not measure in seconds or micro seconds, it is because time would depend about a lot of external factors like memory, ssd, hdd, etc etc; which makes it nearly impossible to measure via just general time units like seconds and milli seconds.

![A%20Practical%20Introduction%20to%20Algorithms%206565611634d44f76b5f74b529f620a07/Untitled%201.png](A%20Practical%20Introduction%20to%20Algorithms%206565611634d44f76b5f74b529f620a07/Untitled%201.png)

### constant

1. Saving to a variable
2. math operations

<aside>
💡 Something like .shift() operation is not constant because if one of the element added to array means others to be shifted to the right.

</aside>

- What do we do if we have mulitple expressions/looops etc
    - Most of the times we just multiply them if they are loops. For example a loop inside loop will cost O(n*n) time complexity.
    - If it's it's couple of simple direct operations, we just add them together: 1+1 =2 O(1); if there are n oprations then O(n)
    - Note that O(nlogn) and O(logn) are different. That means logarithmic time is faster than linear only in the case of $log n$. The logarithmic base is determined by the number of times data input set is made into half. Most of the time it would be $logn$ with the base 2. Some times the base is 10.

![A%20Practical%20Introduction%20to%20Algorithms%206565611634d44f76b5f74b529f620a07/Untitled%202.png](A%20Practical%20Introduction%20to%20Algorithms%206565611634d44f76b5f74b529f620a07/Untitled%202.png)

Time complexity of an algorithm signifies the total time required by the program to run to completion. The time complexity of algorithms is most commonly expressed using the **big O notation**.

Big O notation gives us an industry-standard language to discuss the performance of algorithms. Not knowing how to speak this language can make you stand out as an inexperienced programmer.

![A%20Practical%20Introduction%20to%20Algorithms%206565611634d44f76b5f74b529f620a07/Untitled%203.png](A%20Practical%20Introduction%20to%20Algorithms%206565611634d44f76b5f74b529f620a07/Untitled%203.png)

### Exercise

Using breadcrumbs to remove duplicate items in an array.

[My answer for listing removing duplicates from an array.](https://repl.it/@SaifShines/breadcrumbs?lite=true)

My answer for listing removing duplicates from an array.

<aside>
💡 One thing here to keep in mind is that, using breadcrumbs object here means we are trading space for time complexity. Since we are remembering if the element already existing in the array by creating key with array element mapped to Boolean value.

</aside>

## Memoization

### Exercise 1

```jsx
// Task 1: Write a function, times10, that takes an argument, n, and multiples n times 10
// a simple multiplication fn

console.log('~~~~~~~~~~~~~~TASK 1~~~~~~~~~~~~~~');
console.log('times10 returns:', times10(9)); 

function times10 (n){
  return n*10;
};

// Task 2: Use an object to cache the results of your times10 function. 
// protip 1: Create a function that checks if the value for n has been calculated before.
// protip 2: If the value for n has not been calculated, calculate and then save the result in the cache object.

const cache = {};

const memoTimes10 = (n) => {
  if(n in cache){
    console.log('already found in cache: %o', cache);
    return cache[n];
  } else{
    console.log(' %d is not found in cache. cache is %o', n,cache);
    cache[n] = times10(n);
    return cache[n];
  }
}

console.log('~~~~~~~~~~~~~~TASK 2~~~~~~~~~~~~~~');
console.log('Task 2 calculated value:', memoTimes10(9));	// calculated
console.log('Task 2 cached value:', memoTimes10(9));	// cached
```

<aside>
💡 Did you see `n in cache` in first if condition? I didn't know that look existed till now.

</aside>

[memoize-exercise-2-prompt](https://repl.it/@SaifShines/memoize-exercise-2-prompt#main.js)

## Recursion

1. Identify base case(s).

2. Identify recursive case(s).

3. Return where appropriate.

4. Write procedures for each case that bring you closer to the base case(s)

```jsx
var callMyself = function() {

  if() {
    // base case
    return;
  } else {
    // recursive case
    callMyself();
  }
    
  return;
};
```

### Algorithms in JS codebase

[trekhleb/javascript-algorithms](https://github.com/trekhleb/javascript-algorithms)
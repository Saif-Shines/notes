# 4 Semesters in 6 Hours

ℹ️ 

[Four Semesters of Computer Science in Six Hours](http://btholt.github.io/four-semesters-of-cs/)

# Big O

## Finding Big O

Things that I have learnt so far,

1. It basically doesn't matter much when there are coefficients or other constants those are added much. For example, $`2x^2+2x+1`$ will essentially matters in terms of size or time complexity is the $x^2$
2. O(n) is basically when a single loop is in there.
3. O($n^2$) is when there is a loop inside loop.
4. O($n^3$) is when there is loop inside loop inside a loop. This essentially means there can be a mistake in your code.
5. $O(log (n))$ is when input values increase, the space or time complexity decreases.

## Recursion

1. Calling a a function from a memory doesn't cost much.
2. When you try calling a function let's say an 200 times, then computers needs to keep track of all of those via stacks and execute it. It carries cost.
3. Always, favour for readability than performance. Most of the times recursive nature, favours readability, than the iterables (using loops). Unless, recursive nature is bottle neck, don't use loops.
4.
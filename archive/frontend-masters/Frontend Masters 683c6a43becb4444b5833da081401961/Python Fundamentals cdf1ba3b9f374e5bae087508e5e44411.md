# Python Fundamentals

# Resources

[Learn Python](https://www.learnpython.dev/)

[https://github.com/nnja/python](https://github.com/nnja/python)

[Course Introduction](https://learnpython.netlify.com/01-introduction/)

# Introduction

## Using the Python REPL

```bash
>>> name = "Saif"
>>> name
'Saif'
>>> dir(name)
['__add__', '__class__', '__contains__', '__delattr__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__getitem__', '__getnewargs__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__iter__', '__le__', '__len__', '__lt__', '__mod__', '__mul__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__rmod__', '__rmul__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', 'capitalize', 'casefold', 'center', 'count', 'encode', 'endswith', 'expandtabs', 'find', 'format', 'format_map', 'index', 'isalnum', 'isalpha', 'isascii', 'isdecimal', 'isdigit', 'isidentifier', 'islower', 'isnumeric', 'isprintable', 'isspace', 'istitle', 'isupper', 'join', 'ljust', 'lower', 'lstrip', 'maketrans', 'partition', 'removeprefix', 'removesuffix', 'replace', 'rfind', 'rindex', 'rjust', 'rpartition', 'rsplit', 'rstrip', 'split', 'splitlines', 'startswith', 'strip', 'swapcase', 'title', 'translate', 'upper', 'zfill']
>>> type(name)
<class 'str'>
>>> help(str) # gives the explaination for methods
```

`type()` - Tell you the type of the variable

`dir()` - Tells you all the methods your can access 

Style Guide

[PEP 8: The Style Guide for Python Code](https://pep8.org/)

The equivalent of nothingness is Python is `None`. It’s usually `null` in other languages.

<aside>
💡 When you divide numbers (6/2) then the result is float `3.0`

</aside>

<aside>
💡 In general, use `"` double quotes.

</aside>

```bash
>>> name = "Saif"
>>> f"Hello, {name}"
'Hello, Saif'
>>>
```

```bash
>>> def add_numbers(x,y):
...     return x+y;
... 
>>> add_numbers(1,2);
3
>>> add_numbers(33);
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: add_numbers() missing 1 required positional argument: 'y'
```

The defaults should always come at the last arguments

```bash
>>> def greeting(greet, name):
...     print(f"{greet}, {name}")
... 
>>> greeting("Good Morning", "Saif");
Good Morning, Saif
>>> def default_greeting(greet="Good Morning", name): # default argument coming first
  File "<stdin>", line 1
    def default_greeting(greet="Good Morning", name):
                                                   ^
SyntaxError: non-default argument follows default argument
>>> def say_greeting(name, greet="Hello"):
...     print(f"{greet}, {name}")
... 
>>> say_greeting("Saif");
Hello, Saif
>>>
```

The functions should be always called with arguments passed, in case they are defined with paramenters.

```bash
>>> say_greeting("Saif");
Hello, Saif
>>> say_greeting() # miss passing argument
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: say_greeting() missing 1 required positional argument: 'name'
```

Ways to call functions

```bash
>>> def create_query(lang="javascript", num_stars=50, sort="desc"):
...     print(f"Language: {lang}\n Number of Starts: {num_stars}\n Sort: {sort}")
... 
>>> create_query() # You can call with no arguments
Language: javascript
 Number of Starts: 50
 Sort: desc
>>> create_query("Python") # non positional mention, just the order
Language: Python
 Number of Starts: 50
 Sort: desc
>>> create_query(num_stars=789, lang="Golang") # change order but remember the labels
Language: Golang
 Number of Starts: 789
 Sort: desc
>>>
```

Don’t use Lists as Arguments. Or use it, consciously

```bash
>>> # Don't do this: Lists as Arguments
>>> def foo(a, b=[]):
...     b.append(a);
...     print(f"a:{a} & b:{b}");
... 
>>> foo(2) # appended once
a:2 & b:[2]
>>> foo(2, [1,"sa"]); # appended with default list passed
a:2 & b:[1, 'sa', 2]
>>> foo(7);
a:7 & b:[2, 7]
>>> foo(88)
a:88 & b:[2, 7 , 88] # Appended with OLD FUNCTION CALL!!!
```

### On naming

[https://youtu.be/YklKUuDpX5c](https://youtu.be/YklKUuDpX5c)

# Lists

### Sorting -

```bash
>>> lottery_numbers = [23,22,55,53, 345, 88, 5784] # original order
>>> sorted(lottery_numbers)
[22, 23, 53, 55, 88, 345, 5784]
>>> lottery_numbers
[23, 22, 55, 53, 345, 88, 5784] # Still the same old order
>>> sorted(lottery_numbers, reverse=True)
[5784, 345, 88, 55, 53, 23, 22]
>>> lottery_numbers
[23, 22, 55, 53, 345, 88, 5784]
```

<aside>
💡 Notice the difference between `sorted(..)` and `.sort(..)` . The former only returns the sorted value, but sort() actually sorts elements at their address.

</aside>

### Sort in place

```bash
>>> lottery_numbers
[23, 22, 55, 53, 345, 88, 5784]
>>> # Second way of sorting lists in place
>>> lottery_numbers.sort()
>>> lottery_numbers
[22, 23, 53, 55, 88, 345, 5784]
>>> lottery_numbers.reverse()
>>> lottery_numbers
[5784, 345, 88, 55, 53, 23, 22]
>>>
```

### inserting

```bash
# at the beginning of the list
>>> names
['shaik', 'Saif']
>>> names.insert(0,"Modiji")
>>> names
['Modiji', 'shaik', 'Saif']

# at the end of the list
>>> names.append("Buddha")
>>> names
['Modiji', 'shaik', 'Saif', 'Buddha']
>>>

# joining two lists
>>> favorite_colors = ['red', 'blue', 'green']
>>> names.extend(favorite_colors)
>>> names
['Modiji', 'shaik', 'Saif', 'Buddha', 'red', 'blue', 'green']
>>>

```

### lookups

```bash
>>> names
['Modiji', 'shaik', 'Saif', 'Buddha', 'red', 'blue', 'green', 'Saif']
>>> "Saif" in names
True
>>> "Shaik" in names
False
>>> names.index("Saif")
2
>>> names.count("Saif")
2
>>>
```

### Removing

```bash
>>> names
['Modiji', 'shaik', 'Saif', 'Buddha', 'red', 'blue', 'green', 'Saif']
>>> names.remove('Modiji') # removes first item
>>> names
['shaik', 'Saif', 'Buddha', 'red', 'blue', 'green', 'Saif']
>>> names.remove('Saif') # removes first seen item
>>> names
['shaik', 'Buddha', 'red', 'blue', 'green', 'Saif']
>>> names.remove('anonymous') # throws valueerror if item is not on the list
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: list.remove(x): x not in list
>>> names.pop() # Removes last element from the list
'Saif'
>>> names
['shaik', 'Buddha', 'red', 'blue', 'green']
>>> names
['shaik', 'Buddha', 'red', 'blue', 'green']
>>> names.pop(3)
'blue'
>>> names
['shaik', 'Buddha', 'red', 'green']
>>> len(names)
4
>>> names.pop(4) # Try to remove item outside it's length
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
IndexError: pop index out of range
>>>
```

# Tuples

```bash
>>> # tuple
>>> x = ()
>>> type(x)
<class 'tuple'>
>>> x = (1)
>>> type(x)
<class 'int'>
>>> x = (1,)
>>> type(x)
<class 'tuple'>
>>> x = (,)
  File "<stdin>", line 1
    x = (,)
         ^
SyntaxError: invalid syntax
```

### Example

```bash
>>> student = ("Saif", 25, 5.9, "Freshworks") 
>>> student[0]
'Saif'
>>> student[0] = "Harry Potter"
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'tuple' object does not support item assignment
>>>

# Pull data out of Tuple
>>> student
('Saif', 25, 5.9, 'Freshworks')
>>> name, age, height, company = student
>>> name
'Saif'
>>> age
25
>>> height
5.9
>>> company
'Freshworks'
>>>

# Errors when count miss match (during unpack)
>>> foo, bar, baz = student
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: too many values to unpack (expected 3)
>>>

# Throw away values using " _ "
>>> foo, bar, baz, _ = student
>>> foo
'Saif'
>>> bar
25
>>> baz
5.9
>>> _
'Freshworks'
>>> foo, _, bar, _ = student
>>> _
'Freshworks'
>>>
```

<aside>
💡 It’s not `( )` that makes the difference. But it’s the `,` that makes the difference. For example:

</aside>

```bash
>>> x = "hey", 3, 5 # Still a Tuple
>>> type(x)
<class 'tuple'>

# You can also use it in functions
>>> def response():
...     return 200, "OK"
... 
>>> response()
(200, 'OK')
>>> type(response())
<class 'tuple'>
>>> code, name = response()
>>> code
200
>>> name
'OK'
>>>
```

# Sets

- Unique values
- They are quite stored in hashes when they are stored

```bash
>>> type({4})
<class 'set'>
>>> names = {"Saif", "Salvi", "Saif"}
>>> names
{'Saif', 'Salvi'}
>>> len(names)
2

>>> # let's check for hashes
>>> hash('Saif')
-7578949889830876947
>>> hash('Salvi')
-7125716497704105779
>>> hash([]) # Try on a list?
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: unhashable type: 'list'
>>> hash("Nina")
-479387936693142892
>>>

# adding and removing items from a list
>>> colors = {"red", "blue", "green"}
>>> colors.add("pink")
>>> colors.discard("green")
>>> colors
{'pink', 'blue', 'red'}
>>>
```

### discard and remove

```bash
>>> colors
{'pink', 'blue', 'red'}
>>> colors.remove("pink") # removes same like an list
>>> colors
{'blue', 'red'}
>>> colors.remove("pink") # throws error
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
KeyError: 'pink'
>>> colors.discard("pink") # doesn't throw error even if it's not there in the set
>>>
```

### Combining Sets & quirks

```bash
>>> # Combining two sets
>>> names
{'Saif', 'Salvi'}
>>> colors
{'blue', 'red'}
>>> numbers = {1,2}
>>> names.update(colors, numbers)
>>> names
{1, 2, 'Salvi', 'blue', 'Saif', 'red'}
>>> # Remember that update(..) accepts an sequence as argument
>>> names.update("Hello") # the string is a sequence of characters under the
>>> names
{1, 2, 'Salvi', 'l', 'H', 'e', 'Saif', 'red', 'o', 'blue'}
>>>
```

## `Set` operations

```bash
>>> # set operations
>>> rainbow_colors = { "violet", "indigo", "blue", "green", "yellow", "red" }
>>> favorite_colors = { "white", "blue", "grey" }
>>> # union
>>> rainbow_colors.union(favorite_colors)
{'violet', 'green', 'grey', 'yellow', 'red', 'indigo', 'white', 'blue'}
>>> rainbow_colors
{'violet', 'green', 'red', 'blue', 'yellow', 'indigo'}
>>> rainbow_colors | rainbow_colors
{'violet', 'green', 'red', 'blue', 'yellow', 'indigo'}
>>> rainbow_colors | favorite_colors
{'violet', 'green', 'grey', 'yellow', 'red', 'indigo', 'white', 'blue'}
>>> # intersection uses &
>>> rainbow_colors & favorite_colors
{'blue'}
>>> # difference using hat ^
>>> rainbow_colors ^ favorite_colors
{'violet', 'green', 'grey', 'yellow', 'red', 'indigo', 'white'}       
>>> favorite_colors ^ rainbow_colors
{'violet', 'green', 'white', 'grey', 'red', 'yellow', 'indigo'}
>>>

```

# Dictionaries

```bash
>>> # Dictionaries
>>> example_dict = { "one": 1, "two":2, "three": 3}
>>> # access
>>> example_dict["one"]
1
>>> number_dict = { 1: "foo", 2: "bar", 3: "baz"}
>>> number_dict[1]
'foo'
>>> number_dict["1"]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
KeyError: '1'
>>>
```

### access

```bash
>>> # two ways of accessing items
>>> # 1 - using .get()
>>> example_dict.get("one")
1
>>> example_dict
{'one': 1, 'two': 2, 'three': 3}
>>> example_dict.get('four')
# 2 - way
>>> example_dict['four']
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
KeyError: 'four'
>>>

# default value
>>> example_dict
{'one': 1, 'two': 2, 'three': 3}
>>> example_dict.get('four', 'default')
'default'
>>>
```

keys, values, and items

```bash
>>> nums = {'one':1, 'two':2, 'three':3, 'four': 4}
>>> nums
{'one': 1, 'two': 2, 'three': 3, 'four': 4}
>>> nums.keys()
dict_keys(['one', 'two', 'three', 'four'])
>>> type(nums.keys())
<class 'dict_keys'>
>>> nums.values()
dict_values([1, 2, 3, 4])
>>> nums.items()
dict_items([('one', 1), ('two', 2), ('three', 3), ('four', 4)])
>>> "one" in nums # search
True
>>>
```

# Booleans

```bash
>>> # Boolean
>>> type(True)
<class 'bool'>
>>> bool(True)
True
>>> bool(False)
False
>>> bool(0)
False
>>> bool(-1)
True
>>> bool([])
False
>>> bool(())
False
>>> bool({})
False
>>>
```

<aside>
💡 It’s usually `None`, `""`, `{}` and empty containers - sets, dictonary, tuples  are all `Falsy`. But anything with an item init are `Truthy`

</aside>

## Comparisions

<aside>
💡 Numbers are compared as usually

</aside>

Uppercase letters are lowered valued in ASCII

```bash
>>> 2>3
False
>>> "b"< "c"
True
>>> "bat" < "cat"
True
>>> "t" < "T"
False
>>> [1,2, 3] == [1,2,3]
True
>>> [1,2] == [0,2]
False
>>> #
>>>
```

<aside>
💡 Python has this concept called Identity vs. Equality
Equality is `=`, `==`, `>=`, `<` 
Identity is the `is` operator

</aside>

```bash
>>> a = [1,2,3]
>>> b = [1,2,3]
>>> a is b
False
>>> x = a
>>> x is a
True
>>> x is not a
False
>>> a is not b
True
>>>
```

[and, or, not](https://www.learnpython.dev/02-introduction-to-python/090-boolean-logic/30-and-or-not/)

# Loop and Control Statements

```bash
>>> for color in colors:
...     print(f"the current color is {color}")
... 
the current color is blue
the current color is red
>>> color # there is no scope, so last set value is set forever
'red'
>>>
```

Using `range(..)`

```bash
>>> range(5)
range(0, 5)
>>> list(range(5))
[0, 1, 2, 3, 4]
>>> for num in range(5):
...     print(num)
... 
0
1
2
3
4
>>> list(range(1,5)) # Notice: `1` is inclusive, `5` is excluded
[1, 2, 3, 4]
>>> for num in range(0,7,2) : #step argument (positonal) 
...     print(num)
... 
0
2
4
6
```

Looping over dictionaries

```bash
>>> hex_colors = {
... "red":"#68jj",
... "blue":"#uh443",
... "yellow":"#tehuf7",
... "green":"#yhey3",
... }
>>> hex_colors
{'red': '#68jj', 'blue': '#uh443', 'yellow': '#tehuf7', 'green': '#yhey3'}
>>> for item in hex_colors:
...     print(f" item is {item}");
... 
 item is red
 item is blue
 item is yellow
 item is green
>>> for color,hex_value in hex_colors.items(): # returns tuples (red, #68jj)
...     print(f"color:{color} & hex:{hex_value}");
... 
color:red & hex:#68jj
color:blue & hex:#uh443
color:yellow & hex:#tehuf7
color:green & hex:#yhey3
>>>
```

<aside>
💡 There are `break`, `continue`, statements concerning with the loops.
- `break` will exit the loop complete
- `continue` will take the control back to the next element in the iterator

One important thing to note: If there are nested for loops, the break and continue will only work at the inner most or concerened scope of loop.

</aside>

# Importing Modules

```python
def upper_case_name(name):
    return name.upper()

if __name__ == '__main__': 
  name = "Saif"
  name_upper = upper_case_name(name)
  print(f"The name in upper case: {name_upper}")
```

```python
import name_lib

my_name = 'Salvi'
upper_name = name_lib.upper_case_name(my_name)

print(f"the new upper name in this file is: {upper_name}")
```

********Note********

```python
if __name__ == '__main__'

# __name__ usually outputs the current file that's running
# If you are in name_lib.py, this usage means "if pythin is running my current file (not imported) then make those lines run only for my current execution.
# This means even if other_program.py has imported name_lib.py, we use the methods declaried in name_lib.py without executing lines under if condition.
```

### Handle exceptions

```python
try:
    int("a")
except ValueError as e:
    print("you can't do that! sorry!", e)

print("this is end of my program")
```

## Installing external libraries

```bash
python -m pip install requests
```

# Getting Help

```bash
>>> dir(list)
['__add__', '__class__', '__class_getitem__', '__contains__', '__delattr__', '__delitem__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__getitem__', '__gt__', '__hash__', '__iadd__', '__imul__', '__init__', '__init_subclass__', '__iter__', '__le__', '__len__', '__lt__', '__mul__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__reversed__', '__rmul__', '__setattr__', '__setitem__', '__sizeof__', '__str__', '__subclasshook__', 'append', 'clear', 'copy', 'count', 'extend', 'index', 'insert', 'pop', 'remove', 'reverse', 'sort']
>>> help(list.reverse)

#Help on method_descriptor:

reverse(self, /)
    Reverse *IN PLACE*.
(END)
#
```
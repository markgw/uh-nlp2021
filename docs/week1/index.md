# Week 1: Python for Java programmers, installing stuff and simple NLP

**NB: The contents, including the assignments, on this page can change up until the end of the first lecture.**

This day has two parts. The first is a short introduction to the Python 3 programming language. We assume a good general programming knowledge. This mean e.g. background in the Java programming language, on the level of Introduction to Programming (Ohjelmoinnin perusteet) and Advanced Programming (Ohjelmoinnin jatkokurssi) as taught at the department. For those who are already familiar with Python, this part serves as a refresher.

The second part is about installing some required third party libraries and verifying they work by doing some simple NLP tasks that are common as the initial stages in NLP pipelines.

**Even if you are very familiar with Python, you should read everything onwards from the section "Installing stuff" since the following sections contain the marked assignments of this week**. At the same time, skimming through the document might be useful even if you've used Python before.

> Sections that look like this designate assignments. While it's advisable to try out all examples, at the minimum work through these. This week only, some assignments that look like this are not returned to Moodle.

This week only, the page also contains assignments that look like shown below. Any such assignments are graded and count towards your course outcome. In the following weeks, all assignments are marked and no distinction is made in the instructions between marked (submitted) and unmarked assignments.

> **MARKED ASSIGNMENT**
>
>This is an example of an assignment you are expected to return to Moodle for grading.

## Part 1: Introduction to Python

Note that the following is not a complete description of the Python programming language. You are expected to consult additional resources during the course. The official Python documentation is an excellent starting point. In addition, you might wish to consult the ebook [A whirlwind tour of Python](https://jakevdp.github.io/WhirlwindTourOfPython/), from which this document borrows heavily. In addition, you'll want to consult the NLTK book and NLTK documentation during the latter days.

### On Python 2 and Python 3

Python currently exists in two different flavors, Python 2 and Python 3. For all relevant purposes, **you should only be writing Python 3** unless you find yourself in exceptional circumstances. Python 2 will only receive one final update in the Summer of 2020, after which not even security patches will be released.

Also more specifically for NLP purposes, Python 3 simplifies the handling of non-ASCII characters singificantly by making UTF-8 strings the default. In Python 2, you must take special care when handling any language that is not expressed solely using ASCII characters.

All examples hereafter are in Python 3 unless otherwise denoted. Any mentions of "Python" in the material will, by default, refer to Python 3. Any code you return on this day or any later day will be ran using Python 3.

### Installing Python

Python 3 comes in multiple versions, the latest of which is Python 3.8.1. However, as most Linux distributions lag a bit behind in the Python 3 version they provide, Python 3.5 is currently a *de facto* "minimum version". This holds true for this course as well: the tools we are installing assume that you have at least Python 3.5.

####  If you are using a university-provided computer with Cubbli Linux
If you are working on either a Freshmen laptop with Linux or a computer lab desktop that has Linux, your computer should have Python 3.6.9 (or later) installed on it by default. Check by running `python3 -V` (note capital V). You should be good to go.

Computer labs with Linux computers can be found in Exactum, for example in the rooms B221 and BK107.

#### If you are using your own computer with Windows
The course staff is not well versed in getting Python working with Windows, so you are on your own in terms of installing Python and additional dependencies. If you want to do things easy, just use computer labs in Exactum. If you are sure you want to install Python on your own computer, read on for some options.

You can find instructions for installing Python 3 on your Windows computer at https://installpython3.com/windows/. You can also try out using [Anaconda](https://www.anaconda.com/), which is a relatively popular wrapper around Python. Note, however, that Anaconda's package manager `conda` sometimes lags a bit behind `pip`, meaning that the latest versions of all libraries might not be available. We suggest using `pip` where possible.

You can also try out the [Windows Subsystem for Linux](https://docs.microsoft.com/en-us/windows/wsl/about), which allows you to run Linux within Windows. It should come preinstalled with a sufficiently up-to-date version of Python 3.

Finally, you can use the university's VDI version of the Cubbli Linux. This enables you to run a Linux either via the VMWare Horizon client as a "program" within Windows, or as a tab in your browser. See [instructions for VDI](https://helpdesk.it.helsinki.fi/en/instructions/computer-and-printing/workstation-service/virtual-desktops-vdi).

#### If you are using your own computer with Linux
You probably have at least python 3.5 installed, unless you are on a **very** old installation. Check by running `python3 -V` (note capital V). Version 3.5 and newer are all good.

If your Python 3 is older than 3.5, or is not installed at all, you can install it using your Linux distribution's package manager, such as `apt` for Ubuntu. The details of upgrading your Python 3 installation are left as an exercise for the reader.

#### If you are using your own computer with MacOs
Your Mac probably has Python 2.7 installed by default. This is **not** sufficient and you should install at least Python 3.5. For instructions on how to install Python 3, see e.g. the website https://installpython3.com/mac/.

**Note:** If you are using the instructions from the linked website, the section "Virtual Environments" uses a method different from the method used in this guide. Your options are to ignore that part of the linked guide or to "translate" the `pip` commands used in the course material to `pipenv` commands yourself.

### Running Python

There are a few different ways to run Python code, the simplest of which is using the Python REPL. REPL is short for Read Evaluate Print Loop; the REPL reads your input, evaluates it, prints the output and then repeats the same in a loop.

> Start your Python REPL by typing `python3` in the terminal. Then type in `import this`, press enter and read the Zen of Python which describes "good Python" in a poetic format. You can quit the REPL by either typing `quit()` or pressing Ctrl-D.

The second way of running Python is by writing Python into a file and running the file through the intepreter.

> Create a new file named `zen.py`, write the `import this` statement into it and save it. Run the file in the terminal via `python3 zen.py` and verify the input matches your expectations.

The statement `import this` above is a special case. Normally to output from a .py file, you need to call `print()`.

>  Write and run the program `hello.py`, with the following contents:
> ```python
> "this is a string outside of print()"
> print("this is a string inside a print()")
> ```
> Did the output match your expectations? Determine what happens when you run the same two lines in the REPL.

More experience Python users, especially in the scientific community, often write in Python "notebooks". These are something of a middle-ground between the two previous methods, also allowing the author to embed markdown and LaTeX math. Notebooks, however, have several non-obvious "footguns" that are likely to bite even experienced developers now and then. This is not to say that Notebooks are *bad*, merely that they are not a silver bullet. I suggest that you, especially if you are beginner, work with the REPL and a standard editor like VSCode or PyCharm instead of using notebooks for now. If you are interested to learn more about the footguns in notebooks, see Joel Grus' talk ["I don't like notebooks"](https://www.youtube.com/watch?v=7jiPeIFXb6U) after working through the rest of these assignments.

### Comments

Comments are marked by `#`.
```python
print(2)  # This line prints "2"
```

There is no multiline comment, but multiline strings can be sometimes used for the same purpose:

```python
def double(number):
    """
    This is a method that doubles the argument.

    This text here is really a multiline string that just isn't
    assigned to any variable or used in any other way. Strings
    like this at the start of a function/method or class are
    called "docstrings" and can be used to describe what the
    method does, just like Java's JavaDoc comments. Many IDEs
    extract and show these just as they'd do with Java's JavaDoc
    comments.
    """
    return 2 * number
```

### Newlines are statement terminators

Ending the line is the Python equivalent to Java's semicolon "`;`". If you wish to continue a statement to multiple lines, you need to explicitly use `\` at the end of the line to indicate this:
```python
x = 1 + 2 + 4 + \
    5 + 123
```

At the same time, you are *allowed* to use semicolons to terminate statements, even if this practice is often considered "bad style".
```python
x = 1; y = 2
print(x)  # 1
print(y)  # 2
```

The above could also be written as
```python
x, y = 1, 2
```
for the same effect. This is in fact a more powerful feature since it allows you to do the following:
```python
x = 1
y = 2
x, y = y, x  # swap x and y without an additional variable
```

> Try the last example out in REPL and verify it works.


### Whitespace

Python uses *significant whitespace*. In other words, the indentations rules of Java, which are merely a convention, are actually enforced in Python. Where as you are technically allowed to write the following piece of Java
```java
for(int i : someList) {
System.out.println(i);    
}
```
the equivalent Python snippet
```python
for i in some_list:
print(i)
```
would crash with an `IndentationError`.

This enforcement of indentation also allows Python to get rid of the braces used in Java. The correct way to implement the above Python snippet is as
```python
for i in some_list:
    print(i)
```

Notably, while Python enforces indentation, it does not care *how* you indent as long as you are consistent. You are free to use tabs or any number of spaces for indentation. The convention is to use 4 spaces per indentation.

You are also allowed to use arbitrary whitespace *within* lines, eg. the following is allowed, but not necessarily good code:
```python
seconds_per_year = 60      *     60     *      24*365  
```

> Determine the difference between these two for-loops:
> ```python
> for x in range(4):
>     y = x * 2
> print(y)
>```
>```python
> for x in range(4):
>     y = x * 2
>     print(y)
> ```

Note how `y` exists outside of the loop in the first example even when it's only ever introduced and set within the loop. In broad terms, only classes and methods limit scope in Python.

### Output

Printing is done using the `print` function:
```python
print("some string")  # outputs "some string"
```

Calling `print()` with multiple arguments prints them all on a single line with spaces as the separator. You are free to mix different types of arguments
```python
print("this", "is", "my", "string", 1)  # prints "this is my string 1"
```

### Variables and types

Python variables are assigned using the `=` operator.

Perhaps the most obvious difference between Java and Python is the typing. Like Java, Python is *strongly* typed. That is, it a Python string consisting of only numbers can **not** be treated implicitly as a number:
```python
some_number = "1"
print(2 * some_number) # Does NOT print "2"
```

But at the same time, unlike Java, Python is *dynamically* typed, so that for example the following is completely legal:
```python
my_variable = "1" # this is a string
my_variable = 2 # this is a number
```

> Determine what using the multiplication operator between a string and a number does. What if the number a non-positive integer? What if the number is a decimal, such as `2.5`?

Note the "snake_casing": by convention, only class names are in CamelCase.

#### Objects

Everything in Python is an object.

You can inspect an object's type using `type()`:
```python
>>> x = 1.2
>>> type(x)
<class 'float'>
```

Objects' properties and methods are accessed using the same notation as in Java:
```python
>>> x = 1.2
>>> x.is_integer()  # A method call
False
>>> x.imag  # Property
0.0
>>> x.real  # Property
1.2

```

You can also inspect an object using `dir()`:
```python
>>> dir(x)
['__abs__', '__add__', '__bool__', '__class__', '__delattr__', '__dir__', '__divmod__', '__doc__', '__eq__', '__float__', '__floordiv__', '__format__', '__ge__', '__getattribute__', '__getformat__', '__getnewargs__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__int__', '__le__', '__lt__', '__mod__', '__mul__', '__ne__', '__neg__', '__new__', '__pos__', '__pow__', '__radd__', '__rdivmod__', '__reduce__', '__reduce_ex__', '__repr__', '__rfloordiv__', '__rmod__', '__rmul__', '__round__', '__rpow__', '__rsub__', '__rtruediv__', '__setattr__', '__setformat__', '__sizeof__', '__str__', '__sub__', '__subclasshook__', '__truediv__', '__trunc__', 'as_integer_ratio', 'conjugate', 'fromhex', 'hex', 'imag', 'is_integer', 'real']
```

The methods/members that start with two underscores are "magic methods" that define things such as "what does addition using `+` mean for this object" (`__add__`). Normally you are not expected to call them directly.

> Explore the methods of a string using, e.g. `dir("some string")`.

### Arithmetic operations.

Math is largely the same as with Java. The most notable difference is in that integers are automatically translated to floats as needed. This is mostly relevant for division. Whereas in Java `1 / 2` is zero (`0.5` rounded down), in Python it is `0.5`. To get the same result as Java's integer division produces, you'll need to use the `1 // 2` operator.

Exponentiation is done as `a ** b`.

There is no `++` operator, but you can combine assignment with the arithmetical operations for the same effect: `a += 1` is the same as `a = a + 1`. The `+` can be substituted with any other arithmetical operator, meaning `a %= 2` is the same as `a = a % 2`.

### Comparisons

Comparisons are similar to Java and use the operators `==`, `<`, `<=`, `>`, `>=` and `!=`. The primary difference is that, for the purposes of determining whether some value is between two others, the following expression can be used:
```python
>>> x = 4
>>> 0 <= x <= 10
True
>>> 0 <= x < 3
False
```

> Convince yourself that this works as you'd expect

### Boolean Operators

Boolean operators are `and`, `or` and `not`:
```python
>>> x = 1
>>> (x > 0) and (x < 5)
True
>>> (x > 0) and not (x >= 6)
True
>>> (x < 10) or (x > 100)
True
```

### Lists, Indentity and Membership

Python lists are constructed using the bracket syntax:
```python
numbers = [1, 2, 3]
```

They are accessed like Java arrays:
```python
>>> numbers[0]
1
```

You can use the operators `in` and `not in` to determine whether an element is in a list (or set or any other collection):
```python
>>> 1 in numbers
True
>>> 5 in numbers
False
>>> 2 not in numbers
False
```

You can also check whether two objects are identical using `is` and `is not`:
```python
>>> a = [1, 2]
>>> b = [1, 2]
>>> a == b  # "equal": same list contents in same order
True
>>> a is b  # "identical": exact same object
False
```

### Basic Types

The basic Python types are `int` for integers, `float` for floating point numbers, `complex` for complex numbers, `bool` for Boolean values and `str` for strings. Finally, there is the `NoneType` which is the type of `None`, Python's equivalent for `null`.

#### Integers
```python
>>> x = 1
>>> type(x)  # A number without a decimal is an integer
<class 'int'>
>>> 2 ** 200  # Integers won't overflow. They will just take increasing amounts of memory and will be more costly to operate upon.
1606938044258990275541962092341162602522202993782792835301376
>>> 5 / 2  # Default division casts to float
2.5
>>> 5 // 2  # But you can use the // operator for Java-like integer division
2
```

#### Floats
```python
>>> x = 0.5  # Floats can be defined by adding a decimal
>>> x = 5e-6  # You can also use the exponential notation
>>> 0.1 + 0.2 == 0.3 # Floats suffer from precision issues, just like in Java
False
>>> 0.1 + 0.2  # This is because floats can't represent all numbers.
0.30000000000000004
>>> 0.3  # Python does things to hide this from you
0.3
>>> "{0:.20f}".format(0.3)  # But it can be forced to surface
'0.29999999999999998890'
```

#### Strings
```python
>>> x = "string"  # Strings are delimited by double quotation marks "
>>> x = 'string'  # or by single quotation marks '
>>> x = "a string with ' in it"  # Using one inside the other is fine...
>>> x = 'a string with " in it'  # ...both ways around
>>> x = "A string"
>>> x.upper()  # You can uppercase, ...
'A STRING'
>>> x.lower()  # lowercase, ...
'a string'
>>> len(x)  # get the length in characters, ...
8
>>> x[0]  # use the string as an array of characters, ...
'A'
>>> "a string".capitalize() # and capitalize strings, ...
'A string'
```

#### None

`NoneType` has a single possible value, `None`, which is equivalent to Java's `null`.

```python
>>> x = print("hi!") # Methods that do not return anything...
>>> x  # ...really return None
None
```


#### Booleans

The possible boolean values are `True` and `False`. `True` is equivalent to the integer `1` and `False` is equivalent to the integer `0`. This means you can do arithmetic with booleans:
```python
>>> sum([1, 1, 1])
3
>>> sum([True, True, False])
2
```

This is useful in cases where you want to count the number of `True`s in some collection.

Empty strings, empty lists (rather, collections), zeros and `None` are all falsey. You can use them in `if`s directly:
```python
if "" or 0 or [] or None:
    print('This statement is not reached')
```

#### Casting between types

You can cast between types using the types as methods:
```python
>>> x = 1
>>> type(x)
<class 'int'>
>>> x = str(x)
>>> type(x)
<class 'str'>
```

### Data Structures

Python has four primary built-in data structures: lists, tuples, dictionaries and sets.

#### Lists

Lists are ordered, mutable, variable length collections. You can think of them as equivalent to Java's `ArrayList`.
```python
>>> lst = [1, 2, 3]
>>> len(lst)  # Get length using "len"
3
>>> lst[0]  # Indexed like Java arrays
1
>>> lst[-1]  # You can index from the end as well
3
>>> lst[-2]
2
>>> lst.append(4)
>>> lst
[1, 2, 3, 4]
>>> lst += ["a", "b", [True, False]]  # You can combine lists
>>> lst
[1, 2, 3, 4, "a", "b", [True, False]]  # And lists are not limited to one type at a time
```

Note how, when indexing from the end, the final element's index is `-1` rather than `-0`. The second to last element is then `-2`.

#### Slicing

Lists can be **sliced** using the indexing syntax, retrieving a sublist
```python
>>> lst = [1, 2, 3, 4]
>>> lst[1:3] # Basic syntax is [<from, inclusive> : <to, exclusive>]
[2,3]
>>> lst[:3]  # Omitting the first value defaults to "from start"
[1, 2, 3]
>>> lst[1:]  # Omitting the second value defaults to "to end"
[2, 3, 4]
>>> lst[:]  # Omitting both makes a full copy, which is sometimes useful
[1, 2, 3, 4]
>>> lst[:-2] # You can also use negative indexing to index from end
[1, 2]
>>> x = list(range(10))  # Use `range` and `list` to get longer lists of integers
>>> x
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
>>> x[::2]  # You can also add a second colon with a third number to designate the step size
[0, 2, 4, 6, 8, 10]
>>> x[1:5:2]
[1, 3]
```

> Play around with the indexing until you can determine why the last example behaves as it does.

#### Tuples

Tuples are mostly like lists, but are immutable.
```python
>>> my_tuple = (1,2,3)
>>> len(my_tuple)
3
>>> my_tuple[1]
2
>>> my_tuple[-1]
3
>>> my_tuple[0:2]
(1, 2)
>>> my_tuple.append(1)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'tuple' object has no attribute 'append'
>>> my_tuple[0] = 15
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'tuple' object does not support item assignment
```

#### Dictionaries

Dictionaries are largely analoguous to Java's `Map` structure. They are a mutable key-value storage.
```python
>>> x = {'a':1, 'b':2} # Created using {} and a list of key:value pairs
>>> x
{'a': 1, 'b': 2}
>>> x['a']  # Getting by key
1
>>> x['nope']  # Getting a non-existent key is an error
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
KeyError: 'nope'
>>> val = x.get('nope')  # Use .get() to safely get a value
>>> val is None
True
>>> x.get('nope', 'default')  # .get() also takes a default value
'default'
>>> x['c'] = 3  # Adding to a non-existing key creates new key-value pair
>>> x
{'a': 1, 'b': 2, 'c': 3}
>>> x['a'] = 4  # A key can have only a single value
>>> x
{'a': 4, 'b': 2, 'c': 3}
>>> 'a' in x  # use "in" to check whether key exists
True
>>> 2 in x  # Can't use "in" to check for value existance
False
>>> x[1] = 'a' # Keys and values can be of mixed types
>>> x
{'a': 4, 'b': 2, 'c': 3, 1: 'a'}
```

#### Sets

Sets are like lists, but are unordered and cannot contain duplicates.

```python
>>> a = {1, 2, 3} # Create like a dictionary but without the values
>>> b = {3, 4, 5}
>>> a | b  # Sets support set operations: OR
{1, 2, 3, 4, 5}
>>> a & b  # AND
{3}
>>> a - b  # etc.
{1, 2}
>>> a ^ b
{1, 2, 4, 5}
>>> a.add("a") # Contents can be of mixed types
>>> a.add({"a"}) # But must all be hashable (Adding a set to a set)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: unhashable type: 'set'
```

#### Empty collections, converting between collections.

Notice how both the set and the dictionary use the `{}` delimiters. This means that the statement `x = {}` is ambiguous.

> Use the `type()` method to determine what is created by calling `x = {}`

You can also call `list()`, `set()`, `dict()` and `tuple()` to create empty collections. Supplying another collection as argument, eg. `set([1,2,3,3])` produces a new collection with the elements of the argument collection.

This is especially useful when you want to get the unique elements of a list:
```python
>>> some_list = [0, 0, 1, 1, 2, 2]
>>> some_list
[0, 0, 1, 1, 2, 2]
>>> set(some_list)
{0, 1, 2}
>>> list(set(some_list))
[0, 1, 2]
```

### Control Flow

Python has three primary control flow statements: `if`, `for` and `while`.

#### If
```python
a = 1
if a == 2:
    print('a is 2')
elif a > 2:
    print('a is bigger than 2')
else:
    print('a is less than 2')
```

#### For
`for` loops are like Java's "for each" loops.

```python
for i in [0, 1, 2, 3, 4]:
    print(i)  # Prints numbers from 0 to 4.

for i in some_dictionary:
    print(i)  # Prints the keys of a dict
```

There is no direct syntactical equivalent for Java's `for(int i = 0; i < 10; i++)` loops, but you can get similar behaviour using the `range` method together with a `for.. in` loop:
```python
for i in range(10):
    print(i) # Prints numbers from 0 to 9.

for i in range(10, 20):
    print(i) # Prints numbers from 10 to 19.

for i in range(10, 20, 2):
    print(i) # Prints every second number from 10 to 19.
```
`range` lazily produces numbers as you go, so it's OK to have a huge `range` and only ingest one or two values. Note the similarity of the parameters to slices.

> Try iterating over a string using a `for in` -loop. How does the string behave? Does this match your expection?

> What happens if you iterate over a `some_dict.items()`?

#### While
While loops are exactly like in Java:
```python
i = 0
while i < 10:
    print(i)
    i += 1
```

#### `break` and `continue`
These work as you'd expect from Java. In other words, `continue` jumps to the next iteration of the loop and `break` ends the loop.
```python
>>> for i in range (100):
...   if i % 2 == 0:
...     continue
...   if i >= 10:
...     break
...   print(i)
...
1
3
5
7
9
```

#### `for .. else`

You can add an `else` statement to a `for` or a `while`. Code in the `else` is ran if the loop ends withouth encountering a `break` statement. This is useful e.g. when trying to find a suitable value from collection.
```python
for token in ["this", "is", "not", "a", "great", "example"]:
    if token == "not":
        break
else:
    print("Never reached the break statement")
```

### Defining methods

Methods are defined using the keyword `def`. By convention, methods are named using snake_casing.
```python
def add_and_negate(arg1, arg2):
    return -(arg1 + arg2)

print(add_and_negate(1, 1))  # -2
```

Methods can have default arguments:
```python
def pow(n, power=2):
    return n**power

print(pow(3))  # 9
print(pow(3,3))  # 27
```

Note how the methods do not have attached type information. This enables you to write powerful methods without repetition but is not without its downsides. To this end, newer version of Python support *type hints*, which are essentially voluntary typing information. Several IDEs show the type hints and can automatically detect cases where you are misusing a methods.

```python
>>> from typing import Any  # The type of "whatever, anything goes"
>>> def print_box(content: Any, box_char: str = '#') -> None:
...     # Note the combination of a type hint and a default value
...     # Also note how the type of the return value is specified.
...     # Here, the return type is None because we return nothing.
...     content = str(content) # We said content can be Any, so let's cast it into a string first
...     width = 4 + len(content)
...     print(box_char * width)
...     print(box_char, content, box_char)
...     print(box_char * width)
...
>>> print_box('Hi!')
#######
# Hi! #
#######
>>> print_box('Hello, Word!', '*')
****************
* Hello, Word! *
****************
```

### Lambda functions

Functions can also be declared anonymously as *lambda functions*.
```python
>>> add = lambda x, y: x + y
>>> add(1, 2)
3
```

This is mostly relevant when you want to supply a one-off function as an argument to another function but don't really care about the function otherwise. For example, you could sort a list of numbers by their absolute values:
```python
>>> x = list(range(-5, 5))
>>> x
[-5, -4, -3, -2, -1, 0, 1, 2, 3, 4]
>>> x.sort(key=lambda x: abs(x)) # Key is called for each item and sorting is done based on the outputs of key.
>>> x
[0, -1, 1, -2, 2, -3, 3, -4, 4, -5]
```

As a slightly more complex example, you might use it to sort a list of dictionaries by a specific value using the built-in function `sorted` (which returns a new sorted copy rather than sorting in place):
```python
>>> data = [{'first':'Guido', 'last':'Van Rossum', 'YOB':1956},
...         {'first':'Grace', 'last':'Hopper',     'YOB':1906},
...         {'first':'Alan',  'last':'Turing',     'YOB':1912}]
>>> sorted(data, key = lambda person: person['YOB'])
[{'first': 'Grace', 'last': 'Hopper', 'YOB': 1906}, {'first': 'Alan', 'last': 'Turing', 'YOB': 1912}, {'first': 'Guido', 'last': 'Van Rossum', 'YOB': 1956}]
```

Note how it's allowed to split the declaration of the collection over multiple lines even without using the `\` characters.

### Iterators

We've previously seen how we can iterate over a range using `range` or iterate over a collection using the `for` loop. There are a few other iterators that are very useful:

The `enumerate()` iterator allows us to iterate over both the indices and values of a collection:
```python
>>> my_list = ['a', 'b', 'c']
>>> for index, value in enumerate(my_list):
...     print(index, value)
...
0 a
1 b
2 c
```

Note how we modify the `for in` loop to set two variables at the same time. In reality, the `enumerate` method is returning a tuple of values:
```python
>>> x = next(enumerate(my_list))
>>> x
(0, 'a')
```

The for loop just automatically unpacks the tuple for us, so that we don't have to write the code as
```python
>>> for index_value_tuple in enumerate(my_list):
...     index = index_value_tuple[0]
...     value = index_value_tuple[1]
...     print(index, value)
...
0 a
1 b
2 c
```

The `zip` iterator iterates over multiple lists at once:
```python
>>> list_a = ['a', 'b', 'c']
>>> list_b = ['A', 'B', 'C']
>>> for a, b in zip(list_a, list_b):
...     print(a, b)
...
a A
b B
c C
```

> Determine what happes when you `zip` lists of different sizes.

See the `itertools` package in the Python documentation for additional iterators that are occasionally useful.

### Comprehensions

One of the most beautiful Python concepts, in the opinion the author, are *list comprehensions*, or just *comprehensions* in general. They can be used to succintly describe very complex patterns.

Let us consider for example the following list comprehension:
```python
>>> x = [i**2 for i in range(5)]
>>> x
[0, 1, 4, 9, 16]
```

The comprehension describes the idea of iterating over `range(5)`, observing each value `i` of it in turn and squaring them. The values produced in this way are then used to populate a list. This in an example of describing *what we want* rather than how to accomplish some task.

The syntax here is `<expr> for <value> in <iterable>`. In addition, the syntax allows us to include an optional `if` statement following the iterable, for example like this:

```python
>>> x = [i**2 for i in range(10) if i % 3 != 0]
>>> x
[1, 4, 16, 25, 49, 64]
```

The same syntax can also be used with other types of collections such as a set
```python
>>> data = [{'first':'Mark', 'last':'Hamil'},
...         {'first':'Mark', 'last':'Twain'},
...         {'first':'Alan',  'last':'Turing'}]
>>> {person['first'] for person in data}
{'Alan', 'Mark'}
```
and a dictionary
```python
>>> d = {person['last']: person['first'] for person in data}
>>> d['Hamil']
'Mark'
```

Finally, comprehensions can be used to produce *generators* using the `()` braces. Generators produce their values lazily, meaning that values are produced as-needed, rather than all at once when the generator is defined. This can potentially save enormous amounts of memory.

Standard function syntax can also be used to produce more complex generators. The main difference is that the `return` statement is replaced by `yield`:

```python
>>> def fibonacci():
...     a, b = 0, 1
...     while True:
...         yield a
...         a, b  = b, a + b
...
>>> fibo = fibonacci()
>>> next(fibo)
0
>>> next(fibo)
1
>>> next(fibo)
1
>>> next(fibo)
2
>>> next(fibo)
3
>>> next(fibo)
5
>>> next(fibo)
8
```

To produce values, the code in the generator is ran until a `yield` statement is encountered. The yielded value is then returned to the caller. When the next value is requested, the generator continues from where it stopped (with the same state) and runs until it hits `yield` again. Notice how we used the `while True` loop to ensure an infinite amount of fibonacci numbers.

### Classes and objects

Classes are defined using the `class` keyword. The constructor(s) are defined by the "magic method" `__init__()`:
```python
class Person:
    alive = True

    def __init__(self, name):
        self.name = name

    def die(self):
        self.alive = False

    def __str__(self):
        return "{}{}".format(
            self.name,
            "" if self.alive else " (deceased)"
        )

    def __repr__(self):
        return "<Person: " + self.__str__() + ">"

    def __eq__(self, other):
        if type(other) != Person:
            return false
        return self.name == other.name

```
This example also demonstrates a few other properties of classes. The argument `self`, which corresponds to Java's `this` is always explicitly the first argument of any instance method, including the constructor.

The method `__str__` is equivalent to Java's `toString` and is used when the object is cast to a string. Here we use *string formatting* to conditionally append "(deceased)" after the name of all dead people. String formatting is fairly complex, so you'll want to consult the official python documentation on that as you go.

`__repr__` also represents the object as a string, but is used in e.g. error messages and the REPL. By convention, the string returned by `__repr__` should look like a valid Python expression that could be used to recreate an object with the same value. If this is not possible (e.g. here it's not possible to construct a dead person), a string of the form `<...some useful description...>` should be returned. `__repr__` is also used as a fall-back if  `__str__` is omitted.

Finally, `__eq__` defines how equality is defined between two objects. It's similar to Java's `equals()` method. See Python's documentation for further magic methods.

You would then use the class thus:
```python
>>> p = Person('Per Example')
>>> p
<Person: Per Example>
>>> p.die()
>>> p
<Person: Per Example (deceased)>
>>> print(p)
Per Example (deceased)
```

## Part II: Installing stuff, NLTK and SpaCy

While Python has a good standard library, we are going to need specialized tools like the Natural Language Tool Kit (NLTK). Let's first install NLTK.

### Virtual environments

Installing a huge number of dependencies globally is a bad idea for a multitude of reasons. We'll want to set up a *virtual environment*, which is like a sandbox where we can play: Python modules you install in the virtual environment are not visible to other virtual environments. At the same time, the venv is not a virtual machine or a real sandbox: malicious code can do whatever it pleases.

In your shell, run the following command to set up a new virtual environment, replacing `<venv_path>` with some directory:
```shell
$ python3 -m venv <venv_path>
```
This part only needs to be ran once.

Next, we want to activate the virtual environment.
```shell
$ source <venv_path>/bin/activate
```
This command needs to be run again every time you start a new shell.

You can now start a Python interpreter by tying in
```shell
(venv) $ python
```

Note how you no longer need to type in `python3`, as in the first example of this page.

> Verify that the Python interpreter that started is running Python 3 by checking the version information on the first line printed. It should start with "Python 3.x.y" for some values of "x" and "y".

### Installing NLTK

Exit the Python interpreter. We'll next install some useful dependencies using PIP, the Python package manager.
```shell
(venv) $ pip install --upgrade pip
(venv) $ pip install nltk
```
The first line asks pip to upgrade itself. This does not need to be run every time. The second like installs the `nltk` package. Normally, if ran outside of the venv, this command would try to install `nltk` for every user on the computer. You would likely get an error since you are not (hopefully) running as a super user. If you want to install packages **outside of the venv** so that they are available globally for you only, use the flag `--user` to install into your home directory. **This flag is not needed when working in a venv**.

Open up the Python interpreter again, and run the command `import nltk`:
 ```shell
 (venv) $ python
Python 3.6.7 (default, Oct 22 2018, 11:32:17)
[GCC 8.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import nltk
>>>
```
This loads the nltk module. If you got an error message, something went wrong in a previous step.


### Basic use of NLTK

Next up, we'll download a corpus that comes with the NTLK. In your Python interpreter, write `nltk.download()`. A window should pop up. Select the tab "Corpora", select the corpus "*inaugural*" and press "download". The status should switch from "not installed" to "installed". We've now downloaded a corpus of the inaugural speeches of US presidents. Close the window.

You should find yourself back in the Python interpreter. We'll also want to load the NLTK module `punkt`, which is allows us to easily tokenize text. Do so by running the command `nltk.download('punkt')`. You can also find it in the NLTK downloader's "models" tab.

We can now access the corpus via `nltk.corpus.inaugural`. For example, running `nltk.corpus.inaugural.fileids()` produces a list of all the files that make up the corpus.

```python
>>> nltk.corpus.inaugural.fileids()
['1789-Washington.txt', '1793-Washington.txt', <many more>]
```

Let's check how the length of the inaugural speech has developed over time:
```python
>>> for corp in nltk.corpus.inaugural.fileids():
...   print(corp, len(nltk.corpus.inaugural.words(corp)))
...
1789-Washington.txt 1538
1793-Washington.txt 147
1797-Adams.txt 2585
<etc>
```
> Eye over the list. Are the last few speeches long, short, or about average?


Next up, let's take a close look at Kennedy's speech in 1961 by loading it up as a list of sentences:
```python
>>> sents = nltk.corpus.inaugural.sents('1961-Kennedy.txt')
```

Observe the data loaded into the variable `sents`. It should contain a list of lists. The inner lists are sentences, where each element of the list is a token like a word or punctuation.

We can use the built-in `max` function to find the longest sentence:
```python
>>> max(sents, key=len)
['Let', 'the', 'word', 'go', 'forth', 'from', 'this', 'time', 'and', 'place', ...]
```
The `key` argument expects a method it calls for each element in the input list to determine it's numeric value. The same effect could be achieved more verbosely via `max(sents, key = lambda item: len(item))`

It's getting quite tedious to keep writing the `nltk.corpus.inaugural` part over and over again. Thankfully, this can be helped by writing `from nltk.corpus import unaugural`:

```python
>>> from nltk.corpus import inaugural
>>> inaugural.fileids()
['1789-Washington.txt', '1793-Washington.txt', ...]
```
This demonstrates how we can import individual submodules. We could even import individual methods or classes if we were so inclined.


Since the word "inaugural" is relatively hard to type, we can help our life further by importing the module by a different name:

```python
>>> from nltk.corpus import inaugural as corpus
>>> corpus.fileids()
['1789-Washington.txt', '1793-Washington.txt', ...]
>>>
```

As the first graded assignment, let's find the very longest sentence in the whole inaugural corpus and the president behind it.

First, we'll use a list comprehension to iterate over the individual speeches. The comprehension will, for each speach, add to the list a tuple containing the file of the speech and the longest sentence:

```python
>>> longest_sents = [(speech, max(corpus.sents(speech), key=len)) for speech in corpus.fileids()]
>>> longest_sents[0]
('1789-Washington.txt', ['I', 'dwell', 'on', 'this', 'prospect', 'with', 'every', 'satisfaction', ...])
```

> Ensure you understand what is happening in the list comprehension.

We can then use the `max` function and it's `key` argument to find the tuple with the longest sentence (counted in tokens):

```python
>>> longest = max(longest_sents, key = lambda item: len(item[1]))
```
> Why is the lambda expression needed here? What happens if you just use `key=len`?

Call `' '.join(longest[1])` (note the space inside quotes) to get a slightly nicer looking string representation of the longest sentence of any US presidential inaugural speech. Also check `longest[0]` for which speech this sentence is from.

> **MARKED ASSIGNMENT**
>
> Submit to Moodle who's speech contained the longest sentence, when the speech was given and what the longest sentence was.

### Simple NLP stuff

Next up, let's use NLTK to determine for each word which part of speech it is.

> Do this by passing the longest sentence (as list of words) to `nltk.pos_tag()` and storing the result. The result should look something like `[..., ('without', 'IN'), ('effect', 'NN'), ('.', '.')] `.

Let's figure out the distribution of the various POS tags. To do this easily, we can use the `Counter` class provided in the built-in package `collections`.

> Import the `Counter` class from the package `collections`

`Counter`, as the name implies, is used to count things. We want to count the POS tags, so we need to obtain a copy of the list of the tagged tokens with **only** the POS tags. That is, instead of `[..., ('without', 'IN'), ('effect', 'NN'), ('.', '.')]` we want `[..., 'IN', 'NN', '.']`.

> Construct the tag-only list using, e.g., a list comprehension.

We can now use `Counter` to determine the distribution of the POS tags. Note that `Counter` is an object and not a function, so we need to construct an instance.

> Call the constructor of `Counter` to obtain a new instance, passing the tags-only list as an argument to the constructor. Store the returned object in a variable.

Use the `most_common(n)` method of the object you just constructed to determine the 3 most common POS tags and their counts.

> **MARKED ASSIGNMENT**
>Return to Moodle the 3 most common POS tags, their counts, their meanings and the code you used to obtain the result.
>
> To find out what the tags mean, install the NLTK module "tagsets" and read the output of `nltk.help.upenn_tagset()`. Don't worry if you don't understand them completely, but try to get some idea by looking at the examples.


### SpaCy and entity recognition

Let us also set up another NLP library, SpaCy. Start by installing **inside your virtual environment** the `pip` package `spacy`. Recall how you installed NLTK above.

Next up, we need to install some pretrained models: training one ourselves would take large amounts of time, computation power and data, none of which we have. Also, the SpaCy default models are relatively good for our purposes here.

To install the English language default models, write `python -m spacy download en` in your terminal. **Note:** if you opened a new terminal, remember to source your virtual environment first!

The above should have downloaded a model called `en_core_web_sm` and constructed a shortcut called `en`. SpaCy also allows you to define in more details which models, exactly, you want to download and use. Optionally see the SpaCy's [documentation](https://spacy.io/usage/models#download) for more details.

>**MARKED ASSIGNMENT**
>
> Go to https://spacy.io/models/en and observe the three different English language model sets available. How much larger is `en_core_web_lg` compared to `en_core_web_sm` in terms of megabytes taken up by the model? Observe the accuracy scores reported for named entity recognition task (the rows in "NER accuracy"). An accuracy of 1 means that the model was correct in all test cases, whereas an accuracy of 0 would mean that it was wrong in all test cases. How much accuracy did the larger model gain for how many fold increase in model size?

In Python, import `spacy` and then load up a module containing English language models by calling `nlp = spacy.load('en')`.

Next, try out SpaCy's default **entity recognition model** by writing up a sentence with a few entities (people, places, organizations) and storing it in a variable `sent`. Then call `nlp(sent).ents` and observe the detected entities. Try out a few variations of inputs.

>**MARKED ASSIGNMENT**
>
> What entities does SpaCy's `en` module detect from the sentence `"President Obama said to reporters from the Washington Post that the Federal Reserve had overstepped in its decision to decrease the margins on inter-bank loans last Wednesday"`? What about if you remove the first word?

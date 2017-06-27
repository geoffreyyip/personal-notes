# ES7 to Python 3.5

## Lists

You can shallow copy arrays with `arr[:]`. This is similar to `[...arr]` in ES7.

Concatenation is easier. `arr1 + arr2` instead of `arr1.concat(arr2)`.

Assignment to slices is possible, and may even change the size of the array. 

```python
>>> letters = ['a', 'b', 'C', 'D', 'E', 'f', 'g']
>>> letters[2:5] = []
>>> letters
['a', 'b', 'f', 'g']
```

`Array.prototype.push` is now `list.append`. Don't confuse the two.

`Array.prototype.concat` is now `list.extend`. Both Python and Javascript will take Iterables.

Insertion is available as a standard method. Use `list.insert(i, x)` 

* `i` represents the index to *insert before*
* `x` represents the object to insert

You can remove the first item that matches your query with `list.remove(x)`.

You can pop a specific location with `list.pop([i])`

* Note the `[]` notation denotes an optional parameter. If you leave it off, it'll pop the last element similar to Javascript's `Array.prototype.pop`.
* If you're looking to just delete a value, use `del list[i]`. This is more expressive, and doesn't return a value. You can also pass ranges in, like in `del list[i:j]`

*List comprehensions* are an idiomatic way to generate lists with no side effects in Python. It lets you use Javascript's `map`, and `filter` methods very elegantly:

```python
>>> squares = [x**2 for x in range(10)]
[0, 1, 4, 9, 16, 25, 36, 49, 64, 81]

>>> vec = [-4, -2, 0, 2, 4]
>>> [x for x in vec if x >= 0]  # filters
[0, 2, 4]
>>> [abs(x) for x in vec]  # maps
[4, 2, 0, 2, 4]
```

Check the official guide for more uses. This feature can get pretty deep.

## Tuples

These are exclusive to Python. They don't exist in Javascript, at least not natively.

They are immutable data structures. You can declare them with arrow notation:

```
>>> t = 12345, 54321, 'hello!'
>>> t[0]
12345
```

Python has weird quirks for 0 and 1-length tuples.

```
>>> one = 12345, 
>>> one
(12345, )

>>> none = ()
>>> none
()
```

## Sets

Python has a richer API for Sets than Javascript. You declare them the same way you declare Arrays, but replace the square brackets `[]` with curly braces  `{}`.

```python
>>> # show that duplicates get removed from sets
>>> basket = {'apple', 'orange', 'apple', 'pear', 'orange', 'banana'}
>>> print(basket)                      
{'orange', 'banana', 'pear', 'apple'}
```

Benefits are fast membership testing, automatic deduping, and the whole host of mathematical set operations like union, intersection, difference, and symmetric difference. (Look them up. They go pretty deep.)

Empty sets must be declared with `set()`. Using `{}` declares an empty dictionary.

Set comprehensions are also possible!

## Dictionaries

Javascript calls this an *object*. Normal languages aren't so vague. Python calls this a `dict`. Beyond that, they're very similar. 

Python features to take advantage of:

The [`dict()`](../library/stdtypes.html#dict) constructor builds dictionaries directly from sequences of key-value pairs:

```
>>> dict([('sape', 4139), ('guido', 4127), ('jack', 4098)])
{'sape': 4139, 'jack': 4098, 'guido': 4127}

```

In addition, dict comprehensions can be used to create dictionaries from arbitrary key and value expressions:

```
>>> {x: x**2 for x in (2, 4, 6)}
{2: 4, 4: 16, 6: 36}

```

When the keys are simple strings, it is sometimes easier to specify pairs using keyword arguments:

```python
>>> dict(sape=4139, guido=4127, jack=4098)
{'sape': 4139, 'jack': 4098, 'guido': 4127}
```

When looping through dictionaries, the key and corresponding value can be retrieved at the same time using the `items()` method.

```python
>>> knights = {'gallahad': 'the pure', 'robin': 'the brave'}
>>> for k, v in knights.items():
...     print(k, v)
...
gallahad the pure
robin the brave
```

Use the `zip()` function to loop over two or more sequences at the same time. The two sequences must be of the same length for this to work.

```python
>>> questions = ['name', 'quest', 'favorite color']
>>> answers = ['lancelot', 'the holy grail', 'blue']
>>> for q, a in zip(questions, answers):
...     print('What is your {0}?  It is {1}.'.format(q, a))
...
What is your name?  It is lancelot.
What is your quest?  It is the holy grail.
What is your favorite color?  It is blue.
```

## Booleans

`0` still coerces to false. `''` still coerces to false.

Booleans are now capitalized. `true` is now `True`. `false` is now `False`.

*Short-circuiting behavior* still exists. 

```python
>>> string1, string2, string3 = '', 'Trondheim', 'Hammer Dance'
>>> non_null = string1 or string2 or string3
>>> non_null
'Trondheim'
```

`===` no longer exists in Python. Instead, you just have `==`. Python doesn't have the same *implicit type coercion* behavior that Javascript has, so there's no need to have both `==` and `===`. In Javascript, you have the option of using `==` to compare values with type coercion, and of using `===` to compare values without type coercion. Python requires explicit type coercion for anything to get changed. 

Or more bluntly, `===` in Javascript is `==` in Python. They compare values 'as-is'. `==` in Javascript doesn't exist in Python, because it can't. *Implicit type coercion* doesn't exist in Python. Instead, Python has `is` when you want to compare identities instead of values.

**Note #1: Remember that Javascript's triple equals and Python's `is` are not the same. Javascript's `===` compares values and types, whereas Python's `is` compares identities. Note the following code:**

```
>>> 1000 is 10**3
False
>>> 1000 == 10**3
True
```

`1000` and `10**3` refer to two different identities underneath the hood. So they return false with an `is` comparison. You may notice that `is` returns true from smaller integers like `4 is 4`. This is because Python caches small integers underneath the hood.

**For the above reasons, convention dictates using `==` by default, and using `is` when you need to compare identities. Point is that `==` doesn't have the same pitfalls as it does in Javascript.**

You can compare sequences without weird `for...in` loops! Use the following syntax:

```
(1, 2, 3)              < (1, 2, 4)
[1, 2, 3]              < [1, 2, 4]
'ABC' < 'C' < 'Pascal' < 'Python'
(1, 2, 3, 4)           < (1, 2, 4)
```

As long as the items are of the same type, the comparison will work. If two sequences differ, the comparison will run based on the smaller-sized sequence.

## Control Flows

`for...in` still exists. But you should brush up on it, since `for...in` is discouraged in Javascript and encouraged in Python.

Sequence iteration is easier. You can do `for i in range(5):` or `for i in range(3, 9):`

`if...else` statements are written *without parentheses*. 

`pass` has a neat trick. To define a blank Python function, you can do:

```python
def initlog(args):
    pass
```

The Javascript equivalent would be:

```javascript
function initlog(args) {}
```

Docstrings are encouraged in Python! They are strings placed in triple-quotes right under the function definition. 

```python
>>> def fib(n):   
...     """Print a Fibonacci series up to n.
...
...     The second line is blank by convention, like a Git commit
...     message. Additional information follows in separate pargraphs.
...
...     Multiple paragraphs are okay too!
...     """
...     a, b = 0, 1
...     while a < n:
...         print(a, end=' ')
...         a, b = b, a+b
...     print()
...
... fib(2000)
0 1 1 2 3 5 8 13 21 34 55 89 144 233 377 610 987 1597
```

Scope is now referred to as *symbol tables*. It works as it does in Javascript. Though there's extra functionality. You are allowed to define `global` variables through *global statements*.

Functions default to returning `None` in Python. This is similar to functions defaulting to returning `null` in Javascript. (Note that Javascript has both `null` and `undefined`. Python just has `None`. You should not rely on Javascript functions to return `null` unless it has bee explicitly specified.)

*Default argument values* are still a thing. But **important warning: the default value is only evaluated once. This makes a difference when the default is a mutable object such as a list, dictionary, or instances of most classes. For example:**

```python
def f(a, L=[]):
    L.append(a)
    return L

print(f(1))  # [1]
print(f(2))  # [1, 2]
print(f(3))  # [1, 2, 3]
```

**In the above example, the `L=[]` retains its value between function invocations. That's because the function maintains the same *symbol table* between invocations. Calling `L.append(a)` modifies the array, but it does not change the value of the reference to that array.**

**This behavior differs from Javascript. In Javascript, functions are evaluated at call-time, which means a new function scope will be built on each call. Compare the above with the following:**

```javascript
function append(value, array = []) {
  array.push(value);
  return array;
}

append(1); //[1]
append(2); //[2], not [1, 2]
```

**In other words, Python evaluates the default argument on its first invocation. Javascript evaluates the default arguments on each invocation.**

*Keyword arguments* are new! You can't do that in ES7. But you can do the following in Python:

```python
def parrot(voltage, state='a stiff', action='voom', type='Norwegian Blue'):
    pass

parrot(1000)                                          # 1 positional argument
parrot(voltage=1000)                                  # 1 keyword argument
parrot(voltage=1000000, action='VOOOOOM')             # 2 keyword arguments
parrot(action='VOOOOOM', voltage=1000000)             # 2 keyword arguments
parrot('a million', 'bereft of life', 'jump')         # 3 positional arguments
parrot('a thousand', state='pushing up the daisies')  # 1 positional, 1 keyword
```

Keyword arguments must follow positional arguments. You cannot use a keyword argument and a positional argument for the same argument. It will throw an error.

Javascript's `arguments` objects exists in Python... but better! It's called an *arbitrary argument list*. You can prepend a star to an argument name `*foo` to capture a *list* of all extra *positional arguments*.  This helps with *variadic* functions.

Improvements of `*foo` over `arguments`

* you explicitly state `*foo` in the function declaration
* `*foo` gets passed as an actual *list* whereas `arguments` is only Array-like

Compare the following implementations

```javascript
// pre ES6
function f(a, b) {
  var args = Array.prototype.slice.call(arguments, f.length);
  return args;
}
```

```javascript
// ES6 onwards
function f(a, b, ...args) {
  return args;
}
```

```python
# python
def f(a, b, *args):
    return args
```

You can also capture `dict` in Python! You can prepend two stars to an argument name `**bar` to capture a *dict* of all extra *keyword arguments*. An example follows below. 

```python
def cheeseshop(kind, **keywords):
    keys = sorted(keywords.keys())
    for kw in keys:
        print(kw, ":", keywords[kw])
        
cheeseshop("Limburger", 
           shopkeeper="Michael Palin",
           client="John Cleese",
           sketch="Cheese Shop Sketch")

# client : John Cleese
# shopkeeper : Michael Palin
# sketch : Cheese Shop Sketch
```

**Parameters must be in the following order:**

* Positional arguments
* Arbitrary argument list `*foo`
* Keyword arguments
* Extra keyword arguments `**bar`

*Destructuring arguments*  and *spread parameters* still exists in Python. It's called *unpacking argument lists*. Compare the following:

```python
# python
>>> args = [3, 6]
>>> list(range(*args))            # call with arguments unpacked from a list
[3, 4, 5]

>>> d = {"voltage": "four million", "state": "Missouri"}
>>> def parrot(voltage, state, action):
    	print("The voltage is " + voltage + " in " + state)
>>> parrot(d)
"The voltage is four million in Missouri"
```

```javascript
// javascript
const args = [3, 6]
list(range(...args))
// [3, 4, 5]

const obj = {"voltage": "four million", "state": "Missouri"}
function parrot(voltage, state, action) {
  console.log("The voltage is " + voltage + " in " + state);
}
parrot(obj)
// "The voltage is four million in Missouri"
```

Anonymous functions exist through *lambda expressions*. *Lambda expressions* are short anonymous functions that are syntactically restricted to a single expression and implicitly return. In other words, they're simplified arrow functions. For example:

```python
>>> def make_incrementor(n):
...     return lambda x: x + n
...
>>> f = make_incrementor(42)
>>> f(0)
42
>>> f(1)
43
```

 `lambda` can take multiple arguments. Ex. `lambda a, b: a + b` Note the implicit return.

*Closures*, in the Javascript sense, are possible but they're not as idiomatic. You can return a function within a function from Python, and that returned function will maintain read access to its surrounding environment, but you won't be able to write to those variables (aka rebind those variables) unless you invoke `nonlocal`. 

`nonlocal` works like `global`. By default, variables in the `global` symbol table can only be read, not written to. Declaring `global foobar` gives you write access to that global variable. Similarly, declaring `nonlocal foobar` lets you rebind the closest available `foobar`. Compare the following two implementations:

```javascript
function makeCounter() {
  const count = 0;
  return function() {  // note you can return the function directly
    count += 1;  // note you can rebind the surrounding scope right away
    return count;
  }
}
```

```python
def make_counter():
    count = 0
    def counter():
        nonlocal count  # allows write access to upper-level symbol table
        count += 1
        return count
    return counter
```

`nonlocal count` is considered a qualified variable reference. It specifies additional information about the scope it's accessing.

> A special quirk of Python is that – if no [`global`] statement is in effect – assignments to names always go into the innermost scope. Assignments do not copy data — they just bind names to objects. The same is true for deletions: the statement `del x` removes the binding of `x` from the namespace referenced by the local scope. 

*~ From the Python docs.*

## Modules

They're built into Python. Unlike Javascript, where it's hacked together and there's 6 different systems out there.

Modules are also known as *scripts*.

Within a script, a global variable `__name__` is available to refer to the name of your module as imported.

Like most Javascript module systems, the API methods are not loaded into the namespace of the invoker. Rather they are available as methods on your import.

```python
>>> import fibo

>>> fibo.fib(1000)
1 1 2 3 5 8 13 21 34 55 89 144 233 377 610 987
>>> fibo.fib2(100)
[1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89]
>>> fibo.__name__
'fibo'
```

**Python methods are publicly available by default. You don't have to `export` or use revealing modules or any hacks to do it.**

Python methods and variables are global to the module they were defined in. They will not get injected into other files when the module as a whole gets imported.

**For performance reasons, each module will only get imported once. Subsequent module calls will import the same module as the first call. If you somehow change the module during runtime, you will need to rerun the entire interpreter or use `importlib.reload(module_name)` to bust the cache.**

Tricks with ```if __name__ == "__main__":```

* Invoking a module from the command line sets `__name__` to `__main__`. Therefore, you can use ``if __name__ == "__main__":``for script or testing purposes.

**Python imports do not specify file paths. They only specify the package name. You can't do `import './foobar'` like you do in Javascript. You can only do `import foobar`. Python takes care of the file location stuff for you.**

File locations are determined via the `sys.path` variable. First, it searches the *current working directory*. Then it searches the *standard library*. Then it searches all the other directories specified in the `PYTHONPATH` environment variable.

By default, the *standard library* is first. But when you initialize a Python program, you also give that program the chance to modify `sys.path`. Programs will usually place their containing directory at the start of `sys.path`, which causes the containing directory to be searched before the standard library.

**You can use 'dotted package names' to structure Python's module namespace. Organize your subpackages with folder structures. For example, if you had the following file structure, and you wanted to target `sound/effects/echo.py`.**

```python
sound/                          Top-level package
      __init__.py               Initialize the sound package
      formats/                  Subpackage for file format conversions
              __init__.py
              wavread.py
              wavwrite.py
              aiffread.py
              aiffwrite.py
              auread.py
              auwrite.py
              ...
      effects/                  Subpackage for sound effects
              __init__.py
              echo.py
              surround.py
              reverse.py
              ...
      filters/                  Subpackage for filters
              __init__.py
              equalizer.py
              vocoder.py
              karaoke.py
              ...
```

**...you could write `import sound.effects.echo`. Make sure you include `__init__.py` in your directory structure! That tells python to treat the files in that folder as submodules. The files can be blank, but they have to be there.**

`dir()` method is a globally available function that can find out which variables a module defines.

```python
dir(sys)  
['__displayhook__', '__doc__', '__excepthook__', '__loader__', '__name__',
 '__package__', '__stderr__', '__stdin__', '__stdout__',
 '_clear_type_cache', '_current_frames', '_debugmallocstats', '_getframe',
 '_home', '_mercurial', '_xoptions', 'abiflags', 'api_version', 'argv',
 'base_exec_prefix', 'base_prefix', 'builtin_module_names', 'byteorder',
 'call_tracing', 'callstats', 'copyright', 'displayhook',
 'dont_write_bytecode', 'exc_info', 'excepthook', 'exec_prefix',
 'executable', 'exit', 'flags', 'float_info', 'float_repr_style',
 'getcheckinterval', 'getdefaultencoding', 'getdlopenflags',
 'getfilesystemencoding', 'getobjects', 'getprofile', 'getrecursionlimit',
 'getrefcount', 'getsizeof', 'getswitchinterval', 'gettotalrefcount',
 'gettrace', 'hash_info', 'hexversion', 'implementation', 'int_info',
 'intern', 'maxsize', 'maxunicode', 'meta_path', 'modules', 'path',
 'path_hooks', 'path_importer_cache', 'platform', 'prefix', 'ps1',
 'setcheckinterval', 'setdlopenflags', 'setprofile', 'setrecursionlimit',
 'setswitchinterval', 'settrace', 'stderr', 'stdin', 'stdout',
 'thread_info', 'version', 'version_info', 'warnoptions']
```

Calling `dir()` without an argument lists the user-defined variables defined in the current environment. (To get the system supplied variables, use `import builtins` then `dir(builtins)`)

*Relative imports* are possible within subpackages. 

```python
from . import echo
from .. import formats
from ..filters import equalizer
```

*Absolute imports* are also possible within subpackages. You must start from the top-level directory.

```python
import sound.effects.echo
import sound.effects.surround
from sound.effects import *
```

Note that *relative imports* rely on the `__name__` property. Since the `__name__` of the top-level invocation of the interpreter is always `__main__`, modules intended as entry points must be imported using *absolute imports*.

## Input / Output

`str()` returns human-readable output, similiar to `Object.prototype.toString()` in Javascript. `repr()` returns machine-readable output. For example:

```python
>>> s = 'Hello, world.\n'
>>> str(s)
'Hello, world.
'
>>> repr(s)
"'Hello, world.\n'"  # includes escaped characters
```

*Template literals* kind of exist in Python. There's two ways of doing it. 

First there's `from string import Template` available from the standard library. This maps nicely to Javascript's *template literals*. However, it's designed more for end-user applications instead of production, so I'm not gonna cover that here. Check the official docs for more info.

 then there's the natively available `.format()` string method (no import needed). Instead of using existing variable declarations, you invoke a `.format()` method on the string and pass in the variables you want to use. This has pros and cons. 

In Javascript:

```javascript
var a = 5;
var b = 10;
console.log(`${a} plus ${b} adds up to ${a + b}.`);  
// note you can evaluate the expression within the string
// It's like supplying an anonymous function.
```

In Python:

```python
>>> 'The story of {0}, {1}, and {other}.'.format('Bill', 'Manfred', other='Georg')
'The story of Bill, Manfred and Georg'
# Note that you can use both positional and keyword references.
```

Python's got a rich ecosystem of string formatting to build on. Check the official docs for recipes.

### File Objects

`open(filename, mode)` is encouraged and commonly used. It defaults to read mode and text mode. Note that `open()` returns the file object, not the actual content. Also note that *write* will **overwrite** content and that *append* adds the content to the end.

Specify *binary mode* for non-text files. Corruption can occur otherwise.

Best practice for *writing* and *reading* files is to use `with`. Benefits include:

* File closes after with statement, no need for `f.close()`
* You can name scope the file as `f` .

```python
>>> with open('workfile') as f:
...     read_data = f.read()

>>> f.closed  
True
```

You can then invoke `f.read([size])`, `f.readline()` or `f.write(str)`.

`f.read()` will return newline characters as well.

`f.readline()` will read up to the newline but exclude the newline character.

File position can be changed with `f.seek()` but that's another thing entirely.

### Serialization, JSON

`JSON` is still a global variable.

`JSON.stringify` is now `JSON.dumps` in Python. There's also `JSON.dump(x, file_object)` to write directly to a file object. 

`JSON.parse` is now `JSON.loads` or `JSON.load` for *file objects*.

*Pretty printing* is now invoked with `json.dumps({'4': 5, '6': 7}, sort_keys=True, indent=4)`

`pickle` is available for reasonably complex Python objects. `pickle` is also insecure by default, since it can contain function definitions, an can be used to inject arbitrary code.

## Errors

`SyntaxError` refers to parsing problems.

*Exceptions* like `NameError` and `TypeError` occur after parsing, and during execution. Look up *built-in exceptions* if you need help debugging.

`try...catch...finally` is now `try...except...else...finally`.

`finally` works the same in both. It's a clause that runs no matter what "on the way out" of a `try` block.

`else` clause is new. It only runs on successful executions in the `try` block.

So the possible control flows are:

* `try` -> `except` -> `finally`
* `try` -> `else` -> `finally`

`finally` is useful for releasing external connections (files and network connections), as you'll want to free up memory regardless of success.

`throw Error` is now `raise Exception`.

**Conditional `except` clauses are possible in Python!**

```python
>>> while True:
...     try:
...         x = int(input("Please enter a number: "))
...         break
...     except ValueError:  # takes an argument!
...         print("Oops!  That was no valid number.  Try again...")
```

Two important caveats:

* treat the `except ValueError` portion as a `switch...case` statement. It's not meant for complex conditional `if...else` logic
* You can technically accomplish this in Javascript, but it's horrible inconsistent and not in the specification.

## Classes

Holy crap. This is gonna be a doozy.

*Scopes* are still the same. They are textual regions where a namespace is directly accessible.

Namespace is an abstraction. Symbol tables are the implementation details of namespaces. Symbol tables can be used for other things besides namespaces.

Function invocations create their own namespace, (as do for loops and if-else blocks?). When the function returns, the namespace gets deleted. (Technically, it gets forgotten. If you returned a closure from that function, then a reference will still exist to the variables in that namespace. That causes the namespace to persist, if not completely, then at least partially.)

> Python does automatic memory management (reference counting for most objects and garbage collection to eliminate cycles). The memory is freed shortly after the last reference to it has been eliminated.

Imported modules have their own encapsulated global namespaces. Using the qualifier `global` will fetch variables defined at the top-level of the imported module. It will have nothing to do with the *importing file or interpreter*.

Scopes are determined statically, but used dynamically (though not always and this is an implementation detail that may get changed in the future).

**Classes are still functions in Python, and must be executed before they have any effect**

> Class definitions, like function definitions (`def` statements) must be executed before they have any effect. (You could conceivably place a class definition in a branch of an `if` statement, or inside a function.)

In Python:

```python
class MyClass:
    """A simple example class"""
    i = 12345  # i can be assigned without a constructor

    def f(self):
        return 'hello world'
    
x = MyClass()  # constructor invocation
```

In Javascript:

```javascript
class MyClass {
  constructor() {
    this.i = 12345  // constructor must exist to assign i as an attribute
  }
}

const x = new MyClass()  // constructor invocation
```

**Javascript must specify a constructor if it wants to instantiate properties. Python does not.**

**`constructor()` is now `__init__()`. It has some differences:**

* You must pass `self` explicitly as the first argument. This implements object self-referencing, kinda like `this` does in Javascript. Though `this` in Javascript is counterintuitive, so maybe that's not the best way of thinking about it.
* Instead of `this.prop`, you use `self.prop`. Don't conflate the two. `self` is explicitly bound to its class functionality, whereas `this` is a weird Javascript implementation detail that happens to work for classes... usually. If you invoke a Python method from a different callsite, it will still work as normal, whereas if you invoke a Javascript method from a different callsite, the value of `this` will be different.

```python
>>> class Complex:
...     def __init__(self, realpart, imagpart):
...         self.r = realpart
...         self.i = imagpart
...
>>> x = Complex(3.0, -4.5)
>>> x.r, x.i
(3.0, -4.5)
```

Python instances are still treated like objects. You can assign properties to them. Those properties can be Strings, Lists, Dicts, or Functions to them, just like in Javascript.

**Beware. This flexibility brings the same caveats as Javascript does. Because you can change objects on the fly, it's possible for you to overwrite references to class methods.**

```python
>>> class MyClass:
>>>     def f(self):
>>>         return 'hello world'
>>>     
>>> x = MyClass()
>>> x.f()
'hello world'
>>> x.f = 'blargle'
>>> x.f()
TypeError: 'str' object is not callable 
```

**Instance functions with `self` passed in as the first parameter are not referred to as function objects. They are referred to as method objects. Official section from Python docs says it best:**

>  Usually, a method is called right after it is bound:
>
  ```
x.f()
  ```
>
>In the `MyClass` example, this will return the string `'hello world'`. However, it is not necessary to call a method right away: `x.f` is a method object, and can be stored away and called at a later time. For example:
>
  ```
xf = x.f
while True:
    print(xf())
  ```
>
>will continue to print `hello world` until the end of time.
>
>What exactly happens when a method is called? You may have noticed that `x.f()` was called without an argument above, even though the function definition for `f()` specified an argument. What happened to the argument? Surely Python raises an exception when a function that requires an argument is called without any — even if the argument isn’t actually used...
>
>Actually, you may have guessed the answer: the special thing about methods is that the instance object is passed as the first argument of the function. In our example, the call `x.f()` is exactly equivalent to `MyClass.f(x)`. In general, calling a method with a list of *n* arguments is equivalent to calling the corresponding function with an argument list that is created by inserting the method’s instance object before the first argument.
>
> If you still don’t understand how methods work, a look at the implementation can perhaps clarify matters. When an instance attribute is referenced that isn’t a data attribute, its class is searched. If the name denotes a valid class attribute that is a function object, a method object is created by packing (pointers to) the instance object and the function object just found together in an abstract object: this is the method object. When the method object is called with an argument list, a new argument list is constructed from the instance object and the argument list, and the function object is called with this new argument list.

**Instance method objects are ALWAYS invoked with `self` as the first argument. Instance methods are explicitly bound to the `class` in which it was defined.**

Python has class variables! Class variables are different from instance variables. Class variables are shared (and read-accessible) across all instances. Javascript has this in the form of `prototype` properties.

```python
class Dog:
    kind = 'canine'         # class variable shared by all instances

    def __init__(self, name):
        self.name = name    # instance variable unique to each instance

>>> d = Dog('Fido')
>>> e = Dog('Buddy')
>>> d.kind                  # shared by all dogs
'canine'
>>> e.kind                  # shared by all dogs
'canine'
```

**Beware of sharing variables when the class variable is mutable. Javascript shares this same vulnerable in that changes on shared mutable data structures get persisted across all instances. There's a similar pitfall described above for default parameters.**

```python
class Dog:
    tricks = []             # mistaken use of a class variable
    
    def __init__(self, name):
        self.name = name
        
    def add_trick(self, trick):
        self.tricks.append(trick)

>>> d = Dog('Fido')
>>> e = Dog('Buddy')
>>> d.add_trick('roll over')
>>> e.add_trick('play dead')
>>> d.tricks                # unexpectedly shared by all dogs
['roll over', 'play dead']
```

**Correct class design says you should make `tricks` an instance variable. You can do this by adding `self.tricks = []` to `__init__()`**

**Python does not have private of anything. It's all based on convention. Python developers generally prefix a single underscore to denote an unstable part of an API.**

Cool tricks you can do in Python. You can turn any class into an iterator by defining `__iter__(self)` and `__next__(self)` methods. 
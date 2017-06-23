# `this`

The cool part of `this` is that you can reuse a function against multiple contexts (or as a part of multiple objects).

`this` changes value depending on call site, instead of being lexically scoped. It's kinda like dynamic scoping. It lets you implicitly pass along an object reference.

`this` is always available in every place you go.

The following two statements are equivalent:

```javascript
function identify(species) {
  return this.name + species;
}

let me = { name: "Finn" }
let you = { name: "Jake" }

identify.call(me, " the human") // Finn the human
identify.call(you, " the dog") // Jake the dog
```

```javascript
function identify(context, species) {
  return context.name ;
}

let me = { name: "Finn" }
let you = { name : "Jake" }

identify(me, " the human"); // Finn the human
identify(you, " the dog"); // Jake the dog
```



## What it's not

* It's not a reference to the function's lexical scope, even though that is the reasonable way of looking at it.

* It's not a reference to the function in which it was defined.

  You can already do that like so:

  ```javascript
  function fib() {
    return fib();
  }
  ```

* It has nothing to do with the location in which it was defined

  ```javascript
  function foo(num) {
    console.log( "foo: " + num );

    // keep track of how many times `foo` is called
    this.count++;
  }

  foo.count = 0;

  for (let i = 0; i < 5; i++) {
      foo(i);
  }
  // foo: 1, foo: 2, foo: 3, foo: 4

  // how many times was `foo` called?
  console.log( foo.count ); // 0 -- WTF?
  ```


  *If you look closer, you'll realize that `this.count++` is declaring a variable in the global scope. The first thing it runs it, it sets `window.count` equal to `null`. Then it tries to increment it. You can't increment `null`, so instead you get `NaN`. Subsequent increments will still return `NaN`.*

* **Repeat: `this` has nothing to do with where the variable was defined or written. You cannot expect it to have access to the area in which it was defined. Even thought it usually does.**

## Bindings

### Default Binding

If a function is invoked from the script (and not from another function or object), then `this` refers to everything in the global scope.

Unless strict mode is on, in which case `this` is undefined.

### Implicit Object Binding

If a function is invoked from within an object, then `this` will get bound to that object.

```javascript
function foo() {
  console.log(this.a);
}

var obj = {
	a: 2,
	foo: foo
};

obj.foo(); // 2, callsite is obj
```

Put another way, if a function call is preceded by an object reference, then `this` will equal that object.

```javascript
function foo() {
  console.log(this.a);
  console.log(obj.a);
  console.log(this.a === obj.a);
}

var obj = {
	a: 2,
	foo: foo
};

obj.foo(); // 2, 2, true
```

**Note, this is an implicit binding. If you reference the function, but then call it from somewhere else, it won't have the same effect.**
```javascript
function foo() {
  console.log(this.a);
}

var obj = {
  a: 2,
  foo: foo,
};

var bar = obj.foo;
bar();  // Reference Error! Callsite is the global context!
```

*Here, bar gets invoked from the global context, which means it does not have access to `obj.a`.*

**This most commonly occurs with callbacks (of all kinds, both synchronous and asynchronous).**

```javascript
function foo() {
  console.log(this.a);
}

var obj = {
  a: 2,
  foo: foo,
};

var ONE_SECOND = 1000;
setTimeout(obj.foo, ONE_SECOND);  // Reference Error! setTimeout invokes from the global context.
```

### Explicit Binding

`call`, `apply`, and `bind` are three different ways of explicitly stating what you want `this` to mean.

They all take the context as their first parameter.

`call` and `apply` will invoke the function immediately.

`bind` lets you partially apply parameters (or apply all parameters!) and return a new function for future invocation.

### `new` Binding

Putting `new` in front of a function is known as a *constructor call*. It creates a new object, and sets that object as the `this` binding for that function call. At the end of that function call, the `new`-invoked function will automatically return the constructor object.

`new` can be considered a callsite.

*Note, there is no such thing as a constructor function. There are only constructor calls of regular functions. Any function can be invoked with `new` (though not all functions should be treated as constructors).*

### Precedence

"new binding" > "explicit binding" > "implicit binding" > "default binding"



# Classes + Prototypes

There's a lot of debate over what makes a class a class, and what designates an object-oriented language. 

Paragraphs in this section will talk more about the implementation, rather than the semantics.

In Java, instances are said to be instantiated from classes. Implementation-wise, instances are copies of classes, with maybe an argument or two to set a property (E.g. `new Person('boy')`);.

In Java, you can also inherit from classes to form new classes. This is known as *inheritance*. The relation is known as *child classes* and *parent classes*. A *child class* is a copy of the *parent class* and is completely separate from the original. Another common terminology is *superclass* and *subclass*.
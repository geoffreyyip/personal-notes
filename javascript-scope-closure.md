# Scope

It's an assistant to the *engine*. The *engine* is what executes the code from start to finish. The scope is what collections and maintains a look-up list of all variables.

## Lookup

Lookups occur after compilation (yes, Javascript is a compiled language) during the engine's execution process. There are two kinds of look-ups. There's the assignment lookups, and there's the retrieval lookups.

##### Assignment lookups are when you attach a value to a variable.

```javascript
// `a` gets looked up for purposes of assignment
var a = 2;

// same for `b` in `b = 2`
var b;
b = 2;
```

*Note, variable declaration happens during compilation. Even if you simply delcare a variable like `var b` and never use it, the scope still carves out space for it.*

##### Retrieval lookups are when you get the value defined at a variable.

```javascript
// assignment happens with `function foo() {}` and retreival happens with `foo()`
function foo() {}
foo()  

// retrieval happens at `var b = a`, engine needs to look up what `a` is
var a = 2;
var b = a;
```

##### Assignment lookups and retrieval lookups will throw different errors for undeclared variables.

* In the case of an assignment lookup, if the engine has traversed all scopes ranging from the current to the global scope and found nothing, it will declare a new variable in the global scope. Unless strict mode is on. Then the engine will throw a `Reference Error`.

* In the case of a retrieval lookup, if the engine has traversed all scopes ranging fomr the current to the global scope and found nothing, the engine will throw a `Reference Error`

  *Note, `Reference Error` differs from `Type Error`. A `Reference Error` means the retrieval lookup failed. A `Type Error` means the retrieval lookup succeeded but you tried to do something weird with the value, like access a property on `null` or run a string as a function.*

##### Functions use implicit assignment lookups.

The following two statements are equivalent:

```javascript
function foo(a) {
	var b = a;
	return a + b;
}

var c = foo( 2 );
```

```javascript
var c = function() {
	var a = 2; // implicit variable declaration! 
	var b = a;
	return a + b;
}
```

## Dynamic Scope vs. Lexical Scope

Almost all programming languages, Javascript included, use *lexical scope*.

*Lexical scope* is also known as *static scope*. Variables maintain access to the outer scope in which they were defined in, even after that scope has returned. 

This makes scope very visual. You can deduce outer scopes just by looking at the source code.

```javascript
function foo() {
    let x = 5;

    return function bar() {
        console.log(x + 1);
    }
}

var taf = foo();
taf()  // maintains access to the `x` variable, even though foo() is done running
```

*Dynamic Scope* is when the outer scope is determined at runtime. 

This is more confusing. You can write functions for variables that haven't even been declared yet.

```javascript
function foo() {
    console.log(x);  // `x` is not defined lexically
}

function dummy1() {
    var x = 5;
    foo();  // logs 5
}

void dummy2() {
    var x = 10;
    fun();  // logs 10
}
```

## Closures

> Closure is when a function is able to remember and access its lexical scope even when that function is executing outside its lexical scope.

Kyle Simpson, You Don't Know JS: Scope + Closures

Or as I defined above, closures are all the scopes that variables maintain access to, even after those scopes have completed their initial functionality and returned.

##### You can define revealing modules with closures!

```javascript
function CoolModule() {
	var something = "cool";
	var another = [1, 2, 3];

	function doSomething() {
		console.log( something );
	}

	function doAnother() {
		console.log( another.join( " ! " ) );
	}

	return {
		doSomething: doSomething,
		doAnother: doAnother,
	};
}

var foo = CoolModule();

foo.doSomething(); // cool
foo.doAnother(); // 1 ! 2 ! 3
```

It's called a revealing module, since you're only allowing access to certain functions. Whatever's defined in the `return {...}` is what invoking functions will get. Or, in other words, you can think of it as a **public API for the module**.

Alternatively, you can create a singleton with an IIFE.

```javascript
var foo = (function CoolModule() {
	var something = "cool";
	var another = [1, 2, 3];

	function doSomething() {
		console.log( something );
	}

	function doAnother() {
		console.log( another.join( " ! " ) );
	}

	return {
		doSomething: doSomething,
		doAnother: doAnother,
	};
})();

foo.doSomething(); // cool
foo.doAnother(); // 1 ! 2 ! 3
```
## ES6 Modules

* ES6 Modules are automatically strict-mode code.

* It is considered a 'static' module system. All module dependencies must be loaded, parsed and linked eagerly before any code runs. There is no lazy loading.

  This helps avoid loading the same dependency multiple times. You can avoid cycles by skipping anything already executed.

* Top-level variables are automatically scoped to that module.

* There is no specification on how `import` is supposed to work. It's up to the individual environments and build tools to figure that out. How Node does is will be different from how the browser or Webpack does it.
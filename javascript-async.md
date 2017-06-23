## What is asynchronous programming?

> One of the most important and yet often misunderstood parts of programming in a language like JavaScript is how to express and manipulate program behavior spread out over a period of time.
>
> This is not just about what happens from the beginning of a `for` loop to the end of a `for` loop, which of course takes *some time* (microseconds to milliseconds) to complete. It's about what happens when part of your program runs *now*, and another part of your program runs *later* -- there's a gap between *now* and *later* where your program isn't actively executing.

*Kyle Simpson: You Don't Know JS: Async & Performance*

Programs execute in chunks. There's the now chunk and there's the later chunk. The now chunk requires the engine's full attention, and locks the interpreter until the chunk is completed. It's referred to as a *blocking* chunk of code. So if you have a script with a bunch of now chunks, it will execute predictably. The chunks of code will run sequentially from top to bottom, with each chunk waiting for the previous one to finish.

Now, most Javascript statements are composed of now chunks. They are *synchronous*. But sometimes, you have a Javascript statement that is composed of both now chunks and later chunks. Stuff like network requests. In these cases, the now chunk still gets run like usual. The now chunk requires the engine's full attention, locks the interpreter and blocks other statements from running until it's completed. 

But the later chunk is interesting. It gets deferred. See, the Javascript engine has no sense of time. It's a butler that takes code it and runs it. It needs someone else to schedule the code. So the Javascript engine hands it over to their support team, the hosting environment. The hosting environment does have a sense of time; it's the scheduler. It says I'll keep track of this asynchronous code, and then hand this code back to you when the time is right.

That is the core of asynchronous programming.

See also [Video: What is the event loop?](https://www.youtube.com/watch?v=8aGhZQkoFbQ).

## What is parallelism and concurrency?

**Note, this definition is language agnostic. It doesn't map to ECMAScript's "runs in parallel" defintion too nicely. "run in parallel" is more accurately thought of as concurrency since browser environments use event loops and schedulers to interleave multiple tasks together as once**

> **Concurrency** is when two or more tasks can start, run, and complete in overlapping time periods. It doesn't necessarily mean they'll ever both be running at the same instant. Eg. multitasking on a single-core machine.
>
> **Parallelism** is when tasks literally run at the same time, eg. on a multicore processor.
>
> Quoting [Sun's *Multithreaded Programming Guide*](http://docs.oracle.com/cd/E19455-01/806-5257/6je9h032b/index.html):
>
> - Concurrency: A condition that exists when at least two threads are making progress. A more generalized form of parallelism that can include time-slicing as a form of virtual parallelism.
> - Parallelism: A condition that arises when at least two threads are executing simultaneously.

Parallelism is about things being able to occur simultaneously (i.e. during the exact same instant). Normally, this is does through separate processes or threads. Javascript does not natively do parallelism but it does do concurrency.

## Function Atomicity

Javascript functions are atomic, which means that if you have a `foo()` function and then a `bar()` function, and both are asynchronous, and `bar()` finishes while `foo()` is executing, `bar()` will wait for `foo()`  to finish all of its processes before running. They *run to completion*. Functions are the indivisible unit and you can always expect statements within a function to run in order.

This seems obvious, but it's important to keep in mind. Since in other languages, this may not be the case. Say `foo()` and `bar()` compiled down to a dozen lower-level lines of code. In other languages, the lower-level lines of code may mix together. `foo()` may start running, then `bar()` may run at the same time and the two functions may interfere with each other. If the functions act on the same memory space, then this may result in a race condition.

Since Javascript functions act atomically. this means that either `foo()` runs then `bar()` runs, or `bar()` runs then `foo()` runs.

This is still *non-deterministic* but it's *non-deterministic* on a function level, which is much easier than if the inner lines were weaved together.

## Callback Hell

**It's not just about the nesting. It's also about the execution order.**

```js
doA( function(){
	doB();

	doC( function(){
		doD();
	} )

	doE();
} );

doF();
```

If all the functions are asynchronous, then the execution order goes A -> F -> B -> C -> E -> D.

If all the functions are synchronous, then the execution order goes A -> B -> C -> D -> E.

If it's a mixture of both, then it becomes chaotic.

**And it's not just about the execution order, it's also about the inversion of control.**

The callback may get passed to a third-party utility. You are then trusting the third-party utility to do the right thing.

If they sometimes invoke your callback synchronously, then that invokes your callback "too early" and throws off your execution order. This can introduce race conditions.

> If you have an API which takes a callback, and *sometimes* that callback is called immediately, and *other times* that callback is called at some point in the future, then you will render any code using this API impossible to reason about, and cause the release of [Zalgo](http://t.umblr.com/redirect?z=http%3A%2F%2Fknowyourmeme.com%2Fmemes%2Fzalgo&t=NmMwMzY2NGFkN2JhNzg4YzlhMGY4MjJhMWQwN2VjMGE4NGY0MDhkMCw3NDduUUZ0VQ%3D%3D&b=t%3AqZa3tMNNGjX7PQ45aXJ-jw&p=http%3A%2F%2Fblog.izs.me%2Fpost%2F59142742143%2Fdesigning-apis-for-asynchrony&m=1).

If they invoke your callback multiple times, then that could mutate application state more times than intended.

If they swallow errors and exceptions, then that messes up your logging utilities.

If you fail to pass along necessary environment, or they choose to ignore it, that that messes up your program.

If they have their own asynchronous logic to defer your callback, then your function may get called later than expected.

**Promises will address both these issues.**

## Callback Patterns

**Error-first, or the Node convention, is to reserve the first parameter of each callback to be an error argument. **

**Split callback, or the ES6 Promise way, is to supply two callbacks, one that gets called on success, and another that gets called on failure.**

Note the difference. Error-first is using one callback with two parameters, while split callback is using two callbacks with one parameter each. In error-first, that one callback is always getting called. In split callback, only one of the two supplied callbacks will get invoked, since the async function will either fulfill or fail.

In split callback, the success callback is known as the `onFulfilled()` and the failure callback is known as the `onRejected()`. The completion of either is known as *resolving*, notably reflected in the `new Promise.resolve()` syntax.

## Promises

Promises normalize now and later values to mean the same thing. By treating all values as future values, you create functionality that is temporally consistent. They will always run in the future, and so you can reason about values (or the Promise of values) in a time-independent way.

This prevents calling "too early" or creating a Zalgo. Promises always resolve asynchronously, even if the value is available immediately.

Promises are also externally immutable. Once they are resolved, they can be passed around to multiple parties without fear of modification.

Promises resolve exactly once. They can invoke the `resolve` callback or the `reject` callback, but never both, and never more than once.

Promises capture any and all errors and return them asynchronously.

Finally, Promises behavior nicely as flow control mechanisms. Within any given Promise chain, you can rely on the code running sequentially from top to bottom. 

*Note, you should not rely on scheduling of callbacks across multiple Promise chains. If, for whatever reason, you need to resolve a Promise within a Promise, that callback will get invoked at least two ticks later. First. the environment needs to schedule unwrapping the Promise, then the environment needs to schedule the callback function acting on the value within the Promise. This is because Promises always treat inside values as future values.*
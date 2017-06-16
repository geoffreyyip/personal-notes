### `module.exports` vs `exports`

`module.exports` > `exports`. `exports` is simply a helper function for `module.exports`. `module.exports` is what gets returned to the invoker.

### `ES6 Modules` vs. `Node Modules`

*Needs technical review*

They're completely different things. It's not just syntax. It's also semantics. `ES6 Modules` can be async, whereas `Node modules` are meant to be sync.
### Libraries

* `eslint` configured to Airbnb's preset
* `prettier` auto-formatting for Sublime Text
* `npm test` pre-commit hook: this will ensure everything passes before I commit it
* `npm lint` pre-commit hook: ensures proper code style before commiting
* `nodemon` for automatic file watching and recompiling

### Transpilers

* `babel` set to `babel-preset-env` to allow all the latest features (up to `es2017` last I checked)
* `babel-plugin-transform-object-rest-spread` so I can use `{ ...obj1 }` in my code. (This is ESNext. It's in a proposal stage and likely to be added next year.)

### Github

* Issues should be split into:
  * Priority:{Low,Medium,High}
  * Type:{Enhancement,Bug,Question}
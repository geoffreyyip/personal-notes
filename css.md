## Viewport

Best practice is to set `<meta name="viewport" content="width=device-width, initial-scale=1" />` Viewport is not an official web standard, so there is no official specification for it. But it is effectively supported in all modern-day browsers.

Viewport can get really messy. For edge cases and weird quirks, check out [MDN's writeup](https://developer.mozilla.org/en-US/docs/Mozilla/Mobile/Viewport_meta_tag)

## @Media

Media queries are almost always set to screen. (E.g. `@media screen and (max-width: 900px)`) Ocassionally, you'll have one for print.

`(orientaton: landscape)` and `(max-height: 400px)` are sometimes used.

Parantheses must be used. Not using them is an error.

## vertical-align

Defaults to baseline. Aka, the line on which letters sit on. Note that the baseline does not refer to the top of the line or the bottom of the line. Sometimes you got descenders like `p` or `y` or `q` where the letter dips below. `bottom` refers to where the descenders stop. Other times have fonts with ascenders. This is true for some fonts but not all fonts.

This is normally set to either `middle` or `baseline`. `middle` helps vertically center icons with the surround text.

## @include/ @mixin

@include complements @mixin. @include is what you use to invoke a @mixin.

@mixin defines a bundle of styling properties and variables to be included in another bigger css selector. @mixins can be thought to be included rather than inherited.

## _partials.scss

Partials are denoted with a leading underscore (E.g. `_box.scss`). They do not get directly compiled to CSS. Rather, other `.scss` files import them, and the used portions get compiled to CSS.

## !default and !global

`!default` and `!global` are ways of overriding the regular SASS variable behavior. The `!default` flag assigns a value if and only if a value does not yet exist. Aka, you can have a `_default.scss` file that has ordinary values, and you can have a `_local.scss` file that overrides the ordinary values. To do so, import the `_local.scss` file first.

`!global` lets you define global variables from within a CSS selector. Normally, variables inside CSS selectors are local to that scope. Using `!global` lets you override that.

## @function

Functions and mixins are very similar. They both accept the same arguments. But they return different values. Mixins return CSS styles and variables. Functions return a single data value. (E.g. they return SCSS lists or maps or strings.)

## CSS + SASS Specificity1

For a given element, properties with `!important` flags have the highest specificity. After that are inline styles. After that come external stylesheets.

Within external stylesheets, specificity is strongest for id selectors, then class selectors, then tag selectors. Attribute and

CSS Selectors proceed in this order:

1. Tag Selectors `<h1>`
2. Class Selectors `.info-card` And attribute selectors `a[href]`
3. Id Selectors `#primary-card`

Inline styling will always trump id selectors. Id selectors will always trump class selectors. Class selectors will always trump tag selectors. This works regardless of how close the selector is to the actual tag. In other words, an id selector on a parent element will trump a class selector on a child element.

All other things being equal: `inline styling` > Id Selectors > Class Selectors > Tag Selectors.

Pseudo-classes get lumped into class selectors. (E.g. `:hover`, `:active`) Pseudo-elements get lumped into tag selectors. (E.g. `:after`, `:before`,)

Multiple selectors of the same kind will trump one selector of the same kind, but not selectors of a higher kind. Aka 100 class selectors will trump 99 class selectors, but not 1 id selector.

Tree proximity is a tie-breaker. It'll matter if all other forms of specificity are tied.

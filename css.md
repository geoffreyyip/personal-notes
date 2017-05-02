## About Lists

`<ul>` and `<ol>` differences are mainly due to semantics. `<ul>` will default to bullet points. `<ol>` will default to numbers. `<ol>` has some additional functionality, like `reversed` and `start`, which lets you start a list at a given number. But in general, the tags behave the same.

One rule that must be followed. Children of `<ul>` and `<ol>` must be `<li>`. Once you have the `<li>`, you can put anything inside it. Note the marker (bullet point, numbered, hollow dot) will change depending on nesting level.

To change the bullet-point or numbers to something different, set a `list-style-type` value.

To add your own custom list marker, set `list-style-type: none`, specify a background image per `<li>` tag, and add a left margin.

By default, the list marker is given its own vertical section, aka no text may be present below the list marker. This is known as a `list-style-position: outside` and it's the default value. If you want text to be present below the list marker, when text wraps to a second one, then set `list-style-position: inside`.

For readability, put `list-style-position` and `list-style-type` in the same property. 

```CSS
list-style: circle inside;
```

Final note: most CSS Reset and Normalize scripts will remove this information.

### On Horizontal Lists and Their Quirks

Sometimes you may want a horizontal list. For example, if you have a fixed navigation bar with multiple buttons, you may wrap those buttons inside of `<li>` tags. You have three options to make a list horizontal: change `display` to `inline` or `inline-block` and using `float`.

If changing `display`, `inline-block` is preferred over `inline` as it gives greater control. You can modify margins and set fixed dimensions for `inline-block`. You can't with `inline`. Note that changing display sets `list-style-type: none`. 

`float: left` works, too. But the `list-style-type` will persist, while the `margin-left` disappears. Remember, float wasn't designed for layout purposes. So you'll need to manually assign a margin and manually remove the `list-style-type`.

## Grid Systems

Space between columns is called a _gutter_. 

Most pages have a `960px` container width. To make your grid system snap to that, you want to _gutter_ between columms to match the _gutter_ between the container edge and the first column. 

One way of doing this is setting a `30px` margin on the container, a `15px` margin on the grid, and a `15px` margin for each column. That way, the `margin-right` of one column plus the `margin-left` of the following column adds up to a `30px` _gutter_. And the `15px` margin on the grid, plus the `15px` `margin-left` of the first column also adds up to a `30px` _gutter_. This ensures a evenness between columns.

## How to Design a Solid Button Component

(Need to work on this)
Button should have a `border` property that matches its `background` color on `:hover`.

```CSS
.btn {
  border: 1px solid #fff;
}

.btn:hover {
  background: #fff
}
```

Button should have a `cursor` value of `pointer`.

## Background, Background-Color, Background-Image

`background` can set either a `background-image` or a `background-color`. Most stylesheets use `background` as a catch-all.

### Background Images

Use `background` to set. 

By default, `background-repeat` will be set to `repeat` causing the image to tile multiple times from top-left. You can set it to `repeat-x`, `repeat-y` or `no-repeat`. `no-repeat` is the most common value.

### Multiple Background Images

CSS3 allows multiple background images. To do so, list multiple comma-delimited `background` properties on the same line.

```CSS
div {
  background:  url("foreground.png") 0 0 no-repeat, url("middle-ground.png") 0 0 no-repeat, url("background.png") 0 0 no-repeat;
}
```

Note that within each comma-separated value, you have `background-image background-position background-repeat`.

Images will go from foremost to backmost. The first background property will get layered on top of successive background properties.

### Background-Position

By default, `background-position` will be set to `0% 0%` which means flush with the top-left corner of its container. To offset, specify two values: a horizontal offset, then a vertical offset. Think of this as the `(X, Y)` convention. (Note this differs from the `margin` and `padding` two-parameter convention. There the two parameters means vertical space, then horizontal space.) If the second value is ommited, then vertical offset will default to `50%` and the image will be vertically centered.

If the two values are pixels, then image will be offset with respect to the left edge, then the top edge. If the two values are percentages, then things get really weird. For 50%, the halfway point of the image will match up with the halfway point of the container. For 75%, the 75% horizontal point will match up with the 75% point of the container.

You can also use keywords like `left top` or `right top` or `left bottom` or `left top`. These will flush the image with the respective corner.

Finally, you can specify four values, aka `keyword, value, keyword, value`. For example, `left 50px bottom 20px`. That way, instead of offsetting from the left-top edges, you can offset from the left-bottom edges. 

Three values is possible but not recommended.

The keyword `center` is strange. If you specify `background-position: center`, then it will horizontally and vertically center. If you specify `left center` or `top center` or `bottom center` or `right center`, then it will flush to that edge, and center with respect to that edge.

### Background-Gradients

`linear-gradient()` counts as a background image. Make sure to define a solid color fallback value, in case the gradient function fails.

```CSS
div {
  background: #466368;
  background: linear-gradient(#648880, #293f50);
}
```

The first value gets overriden by the second one in a modern browser. An older browser won't understand the second line, and will keep the solid color defined in the first line.

### Background-Size

Note: Images will attempt to preserve their original aspect ratio. In other words, if you set a horizontal size, the vertical size will change to maintain the aspect ratio.

There are two main schemes you can use to specify background-size. The simplest is to supply two values. Like `background-position`, `background-size` follows the `(X, Y)` convention. Values can be pixels or percentages, and will be set with respect to the **container element, not the image itself**. For example, setting `background-size: 100% 100%` will stretch the image to fit its container. This can destroy the original aspect ratio. If you want to preserve the original aspect ratio, then you can do `background-size: 100% auto` or `background-size: auto 100%`. The former sets the image's horizontal size to match the container's horizontal size, and then automatically scales the vertical dimension. The latter sets the image's vertical size to match the container's vertical size and then automatically scales the horizontal dimension.

The second scheme is to supply a `cover` or `contain` keyword. Both will preserve the original aspect ratio. `cover` will transform the image until the content cover the entire container. `contain` will transform the image until the entire image is within the container.

## Background-Clip + Background-Origin

`background-clip` defaults to `border-box`. `background-origin` is set to `padding-box`. `background-clip` tells the browser when to truncate or cut off the image. `background-origin` says where to start the background. By default this will mean the top-left corner of the padding. But you can change it to the top-left corner of the content with `content-box` or the top-left corner of the `border` with `border-box`. You can combine that with `background-position: bottom right` to originate the background from a different corner.

## Font

`font` properties have the following signature: `font-style`, `font-variant`, `font-weight`, `font-size`, `line-height`, and `font-family`. `font-size` and `line-height` have to be separated by a `/`. (E.g. `14px/22px`). That's also why the second value is usually bigger.

No commas separate the properties, except for `font-family` which is a variadic property that uses commas.

## Color

`color` property always refers to the text color. Note that there is no `font-color` property. Fonts are files, kinda like mp3s are files. You can apply a color on top of a font file the same way you can apply a volume or playback speed to a mp3 file. But you can't pick a font-color. Font files just have the typeface information.

## Line-Height

General rule of thumb: line height should be set to 150% of the font height. Generally setting the `line-height` property to `150%` is enough. But font files can be weird. So always experiment and measure.

Nice CSS Trick: To center text within buttons, alert messages and single-text blocks, set `line-height` and `height` to be the same value.

Ex.
```
.btn {
  line-height: 22px;
  height: 22px;
}
```

## Text-Shadow vs. Box-Shadow

`text-shadow` puts a shadow on the text. `box-shadow` puts a shadow on the entire text.

## Text-Align vs. Float

`float` moves the entire element. `text-align` moves just the text inside.

## Floats / Clear

Floats are tricky. Float property defaults to none. Setting `float` to either `left` or `right` will take that element outside the normal flow. This causes the width of the element to default to the width of the content inside, kind of like an inline element. This is fine for pictures set within a paragraph of text. But it's not fine for columns of text, or aside navigation bars. 

You can solve this by setting a fixed `width` on each element. Add a margin to prevent floated elements from touching each other.

Note that `float` elements are not `inline` elements. Even though elements default to the width of their content like `inline` elements, they do not change their display to `inline`. In fact, `float` elements are meant for `block` elements. It's a best practice to set `float` only on `block` elements.

Something to help put all these quirks in context: `float` was designed to let text wrap around images. Images are typically displayed as `block` elements since they may span over multiple lines of text. But they also have a fixed width or resolution. So the default behavior of `float` is to default the width of the element to match the width of the image content. For this specific purposes, `float` works pretty well. But once you start using `float` outside that context, say for making two-column layouts, `float` starts to get weird.

If you use `float` to make two column layouts, then content that occurs after the two columns (say a footer) may blend in with the floats. You need a way to separate the footer from the two floated columns. There are two ways of doing this. The first way is to set a `clear` property on any element that comes after the `float`. This tells elements that are listed after the floats NOT to wrap around the floated content, and instead resume the normal flow. The second way is to use a **clearfix** and wrap the `float` elements within a parent container, add pseudo-classes `:before` and `:after` for CSS to inject content, and then set `clear: both` on that injected content. Typically the injected content is a blank string that takes up no display space. That way you can encapsulate all the weird quirks of `float` within a HTML container. Usually, the container has a `class="clearfix"` attribute.

The `clearfix` method is favored. It's more upfront code. But it's also a reusuable bundle of class properties. And code reuse is always awesome.

## Box Model

### Default Values

Block elements have a default `width` of `100%`. They will expand to fill the space of their container. Inline-block elements contract and expand to fit their content. Inline elements can't have a fixed width, and will always have a width equal to the content size. Block elements will have their height default to the size of their content.

A quirk of inline elements is that they take horizontal margin but not vertical margin.

Box-sizing defaults to content-box, an additive model. You have your content width + height and then you add paddings, borders, and margins on top of that. Much more convenient is to use border-box. It makes the math easier. Modern browsers support it, and you should only use box-sizing on older browsers.

### Box, Inline, Inline-Box

`Inline` elements can't have fixed widths or heights. Unless you use `inline-box`. Then the elements can be set on the same line as other elements and have width/ height set.

### Margin and Padding Colors

Margins and padding colors are transparent by default. Margin will take on the background color of their containers. Padding will take on the background color of their elements.

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

## CSS + SASS Specificity

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

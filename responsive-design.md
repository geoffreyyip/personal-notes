### Best Practices

Include this line:
`<meta name="viewport" content="width=device-width, initial-scale=1">`

Pixels and viewports are weird and confusing. Don't worry about them. Just do this, and work with the CSS pixels. This will resize the content of the page to the `device-width`, making it responsive.

### How to Get Automatically Stacking Columns at Smaller Sizes

Option#1: Use a grid system, like Bootstrap Something like the code below will stack on desktops, but collapse on smaller screens.

```HTML
<section class="col-md-6"></section> 
<aside class="col-md-6"></aside>
```

Option#2: Define your own media queries. Use the same classes as above but define your own.

```CSS
.col-md-6 {
  float: left;
  width: 50%;
}

@media screen and (max-width: 420px) {
  .col-md-6 {
    float: none;
    width: auto;
  }
}
```

_(Note: `width: auto` is better than `width:100%`. If you specify `width: 100%` on those elements, the content will expand to fit the width of the parent container, and then margin will get applied on top of that. If you specify `width: auto` then the content width will (try to) adjust such that content + padding + border + margin fits inside their parent container.)_

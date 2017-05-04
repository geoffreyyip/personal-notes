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

Make sure you set `<button type="something">`. By default, it's set to `type="submit"` and will trigger a page reload.

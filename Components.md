### Buttons are not links

Save buttons for actions: submitting a form, registering, logging in, etc.

### :hover, :focus, :active

(Need to work on this)
Button should have a `border` property that matches its `background` color on `:hover`.

```CSS
.btn-alt {
  border: 1px solid #fff;
}

.btn-alt:hover {
  background: #fff
}
```

Button should have a `cursor` value of `pointer`.

Make sure you set `<button type="not the default value">`. By default, it's set to `type="submit"` and will trigger a page reload.

Remember to set the `:focus` state! This can happen when a user is tabbing between different parts of a page. For simplicity, just copy the `:hover` styling.

Also remember to set the `:active` state.

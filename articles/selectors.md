# A Crash Course in CSS Selectors

<!-- TODO(maddie): more of an intro -->
To get started, there are four kinds of selectors in CSS.

### `.class`

The selector `.class` selects every element with that class name. It is meant for reusable components, like `.panel`, `.form-control`, etc.

For example, if you had this as your HTML
```
<div class="fun">
  <h1>Hooray!</h1>
  <p class="special fun">Hello World!</p>
</div>
```
and this as your CSS
```
.fun {
  padding: 10px;
  background-color: pink;
}

.special {
  color: blue;
}
```

The `div` and the `p` elements would both have `10px` padding and they would have pink backgrounds. Only the `p` tag would be colored blue because it is the only element with the class `.special`.

### `#id`

### `*`

### `element`


<!-- TODO(maddie): referential selectors -->
<!-- TODO(maddie): naming classes and ids -->

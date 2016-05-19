# A Crash Course in CSS Selectors

<!-- TODO(maddie): more of an intro -->
To get started, there are four kinds of selectors in CSS.

## `.class`

The `.class` selector selects every element with that class name. It is meant for reusable components, like `.panel`, `.form-control`, etc.

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

## `#id`

The `#id` selector selects the only element with that ID. It is meant to be the most specific selector you can use to hone in on a very specific element and nothing else.

For example, if you had this as your HTML
```
<div></div>
<div></div>
<div id="special"></div>
<div></div>
```
and this as your CSS
```
#special {
  background-color: yellow;
}
```
Only the element with `id="special"` would be yellow.

In most cases when building out CSS for a large and complex product, you generally won't want to use IDs. Styling IDs does not scale. Sure, today you only have one `settings` button, but what if some day you want to add another button right next to it that's styled the same way? In general, a good practice is to write all CSS as if you don't know how many elements you will want to style that way, as it is least error-prone.

## `*`

In many ways the exact opposite of `#id`, `*` selects literally *every* element in the DOM. Use this with caution.

## `element`


<!-- TODO(maddie): referential selectors -->
<!-- TODO(maddie): naming classes and ids -->

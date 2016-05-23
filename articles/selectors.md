# A Crash Course in CSS Selectors

<!-- TODO(maddie): more of an intro -->

## The Building Blocks

To get started, there are four kinds of selectors in CSS that are the building blocks for everything else, classes, ids, elements, and the universal selector. They are the atoms and every other selector is a molecule made up of these parts.

### `.class`

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

### `#id`

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

### `element`
<!-- TODO(maddie): link to semantic markup article -->
This selector refers to the actualy DOM element. When you style all `div`s, you are selecting every `<div></div>` in the DOM, regardless of what class or IDs they have. If you are taking advantage of semantic markup, you can use these selectors pretty effectively without needing to add extra classes.

### `*`, the Universal Selector

In many ways the exact opposite of `#id`, `*` selects literally *every* element in the DOM. Use this with caution.

## Naming Classes and IDs

It is important to choose the names you give classes and ID's carefully. Syntactically, its usually best to use lowercase words connected by hyphens like `.class-name` (fun fact: this is called kebab case). You want to pick class names and IDs the same way you would pick a variable name in any other coding project, keeping them specific, consistent, and easily extendable.

## Putting Things Together
<!-- TODO(maddie): referential selectors -->
You can accomplish a lot with just the simple selectors outlined above, but sometimes you want to get fancy and complex with your CSS. You can chain together selectors to be more specific.

For example, if you had this as your HTML

```
<div>
  <p>Hello</p>
</div>
<p>World</p>
```

and this as your CSS

```
div p {
  font-weight: bold;
}
```

You would get something that looked like this:

**Hello** World

The selectors read from outside in, so first looking at all DOM elements that are `<div>`s, then inside searching for `<p>` elements. This is useful when you want to override default styling for a special component like a panel or a modal.

### Combinators

What if you want something more specific than just searching for descendants of a parent element? There are combinators, or operations you can do on selectors to get funky with your code and generate some cool stuff.

#### Concatenating Selectors

You can increase the specificity of a selector by concatenating two core building blocks together. This selects only the elements that apply to both conditions.

Examples:

`div.special` selects all elements that look like `<div class="special"></div>`

`.class-name.other-class-name` selects all elements that have `class="class-name other-class-name"` in their HTML.

`input[type="text"]` selects all elements that look like `<input type="text"></input>`. This is a special case utilizing what is called an attribute selector, where it picks up other properties of the DOM element, in this case `type`. This works for most attributes, but its better to avoid this except when it affects the HTML that is displayed, like when using types for different `input` tags. 

#### >

`>` is the direct child selector. It will only select elements that are directly below its parent.

For example, if you had this as your HTML

```
<div>
  <div class="bar">
    <p>Hooray!</p>
  </div>
  <p>Hello</p>
</div>
<p>World</p>
```

and this as your CSS

```
div > p {
  font-weight: bold;
}
```

Still only the word "Hello" would be styled.

#### ~

`~` is the sibling selector. So any elements that are at the same level and exist under the same parent will be selected.

For example, if you had this as your HTML

```
<p>Yay!</p>
<div class="selected">
  <p>Hello</p>
</div>
<p>World</p>
```

and this as your CSS

```
.selected ~ p {
  font-weight: bold;
}
```

The words "World" and "Yay!" would be bolded this time, because they are at the same level as the `<div class="selected">` and they are all exist under the same parent `<body>`.

#### +

`+` is the adjacent sibling selector, so only the sibling immediately following the first selector will be chosen. Note that this is not the first element of this type following the selector, its only if the second element is directly adjacent to the first.

For example, if you had this as your HTML

```
<div class="selected"></div>
<h1>Testing</h1>
<p>Yay!</p>
<div class="selected">
  <p>Hello</p>
</div>
<p>World</p>
```

and this as your CSS

```
.selected + p {
  font-weight: bold;
}
```

Only the word "World" would be bolded. Take special note that "Yay!" is not bolded because there is another element between it and `<div class="selected">`.

<!-- TODO(maddie): link to part 2 -->
**Keep an eye out for Parts 2 and 3 of this blog post where I will discuss pseudo elements and more advanced selectors that can be used with SASS (a beautiful CSS preprocessor).**

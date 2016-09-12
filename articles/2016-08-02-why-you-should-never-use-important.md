# :no_good: Why you should never use `!important`

Using `!important` in a large complex stylesheet system is a terrible idea. It makes it nearly impossible to build upon a pre-existing design, and gives all your coworkers many a headache. Avoid it at all costs.

## First of all, what is `!important`?

`!important` is a keyword that can be added to the end of a style declaration to make that rule override all contradicting rules that come after it.

For example, if you had this as your CSS:

```css
div {
  color: blue !important;
  color: green;
}
```

The `<div>` is going to be blue. Even though you told it to be green.

This `!important` keyword is extremely powerful, but with great power comes great responsibility. But before we get into exactly _how_ `!important` is powerful, we have to go back and revisit the process that determines the order in which CSS styles are applied.

## A Refresher on How CSS Determines Which Styles to Apply

The first thing to know is that CSS is an acronym for "Cascading Style Sheets". The styles _cascade_ on top of each other when they're applied to an element. Assuming all styles have the same weight, they will be applied from top to bottom.

For example, a div with the following stylesheet

```css
div {
  border: 5px solid green;
  margin: 40px;
  height: 50px;
  width: 100px;
  background: yellow;
  transform: rotate(45deg);
  height: 100px;
  background: pink;
  transform: skew(20deg);
}
```

would end up looking like this:

![skew image](../images/skew-image.png)

The styles at the top of the declaration get applied first and the later styles (specifically `background` and `transform`) overwrite the previously applied styles.

For a visual representation of how this looks:

![skew image progression](../images/skew-image-progression.gif)

## Specificity

A single element can be targeted in CSS using a bunch of different combinations of selectors, depending on its place within the DOM (for a refresher on selectors, read [this](2016-05-19-a-crash-course-in-selectors.md), [this](2016-05-23-selectors-part-2-pseudo-classes.md), and [this](2016-06-07-selectors-part-3-sass.md)). Depending on the selector you use to target an element with styling, the level of *specificity* of those styles can vary. Higher levels of specificity overwrite lower levels. The cascading effect outlined in the previous section is applied from lower specificity levels to higher, and then within each level.

### How is specificity calculated?

The specificity of a selector can be calculated using the following expression:

`100 * a + 10 * b + c`

- `a`: The number of `#id`s in the selector
- `b`: The number of `.class` selectors, attribute selectors (like `[type="number"]`), and pseudo-class selectors (like `:hover`)
- `c`: The number of type selectors (`div`, etc.), universal selectors (`*`) and pseudo-elements (like `::before`).

[Here](https://www.w3.org/TR/CSS2/cascade.html#specificity) is some more information on calculating specificity from W3.

### Example

Let's consider the selector `.navbar ul.menu li#first a:not(:visited)`

There is one id (`#first`), so `a = 1`.

There are two class selectors (`.navbar` and `.menu`) and two pseudo-classes (`:not` and `:visited`), so `b = 4`

There are three type selectors (`ul`, `li`, and `a`).

So we have `(100 * 1) + (10 * 4) + (1 * 3) = 143`. The specificity of this selector is 143.

## Why is `!important` so goddamn awful?

In the context of the calculation above, an `!important` keyword is worth 1000. When you add `!important` to a rule, the specificity of that rule just increased by a full order of magnitude. So to override an `!important` rule, you have to either give the new selector 1000 "specificity points" (10 `#id`s or 100 `.classes`, etc. PLEASE DON'T EVER DO THIS) or you can give that overriding rule an `!important` tag, too.

So let's say you inherit some styling that you want to overwrite, but it has an `!important` tag. You style the element how you want and you slap an `!important` on it to make sure it overrides the previous style. Then in six months you are styling something else that contradicts the other style rule you already wrote. You have two choices. You can either refactor all the code to remove the `!important` tag you wrote initially (which also requires removing the first `!important` you were trying to deal with in the first place). Or you can just use another `!important`.

As you can see, this just creates an endless cycle of dealing with a highly specific style rule by overwriting it with something just as specific. As this happens, the code gets messier and messier and the power of the `!important` weakens with every use. When everything is `!important`, nothing is.

So just save yourself and your coworkers the stress and heartbreak and just avoid using `!important` in the first place (especially when you're building a stylesheet that will be inherited by other projects).

## Is is _ever_ okay to use `!important`?

Yes! There are some notable use cases where it is okay to use `!important`. They don't happen often, but when they do, the `!important` tag will actually make your life better, not worse.

### Utility Classes (Be Careful)

This use case is really only helpful if you are absolutely certain you want a style to persist throughout your stylesheet. Utility classes can use the `!important` tag successfully.

Let's start with a utility class that just hides an element, `.hidden`.

```css
.hidden {
  display: none;
}
```

Because `.hidden` only has a specificity of `10`, it can be overridden easily. To ensure that it always works as intended, we modify it to

```css
.hidden {
  display: none !important;
}
```

For another example, consider `.clearfix`. Clearfixing is a common "hack" to allow a parent element containing floats to clearly fit all of its children without affecting the flow and placement of other elements around it. It looks something like this:

```css
.clearfix:after {
  visibility: hidden;
  display: block;
  content: " ";
  clear: both;
  height: 0;
}

.clearfix {
  display: block;
}
```

The `.clearfix` selector has the specificity of `11`. So what happens if a more specific selector targets an element that has the `.clearfix` class on it? The entire point of having that class there gets thrown away.

But when you change the above styling to this:

```css
.clearfix:after {
  visibility: hidden !important;
  display: block !important;
  content: " " !important;
  clear: both !important;
  height: 0 !important;
}

.clearfix {
  display: block !important;
}
```

The `clearfix` class maintains its purpose even when another class accidentally overwrites it.

### User Style Sheets

In most browsers, if you dig deep enough into your settings you can write custom CSS that gets applied on every page. Most often this is done for readability (increasing base font sizes, improving contrast, etc).

Since these styles get applied everywhere, the selectors used probably will not be very specific. Targeting a `div` or the `body` will only give you a specificity of `001`. These would get overridden immediately on almost every page! Using `!important` here allows you to ensure that your user styling is always applied to the elements in question.

### Targeting IE 5/6

Internet Explorer versions 5 and 6 completely ignore the `!important` keyword when the same property is declared in the same block.

```css
div {
  background: blue !important;
  background: green;
}
```

If you're trying to do something fancy with multiple browsers where this trick comes in handy, by all means use it. However, you'd probably be better off using a library like [Modernizr](https://modernizr.com/) to handle browser differences and versions.

## Conclusion

`!important` is a powerful modifier to your CSS declarations. It can both help and hinder your progress, depending on its use. Make sure to seriously consider whether or not using `!important` is necessary, and be conservative in its use. Your teammates, and anyone who tries to build a UI off of your code in the future, will thank you.

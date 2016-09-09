# :name_badge: Semantic HTML Elements

For this article, I assume you have a basic working knowledge of HTML. If this is not the case, read over a few of [these tutorials](http://htmldog.com/guides/html/beginner/), and then come back.

### HTML5

HTML5 was the newest iteration of of HTML published in 2014. This version primarily focused on enabling multiple devices and systems to interpret the markup languge. With a growing focus on responsive websites, screen readers and accessibility, the WWW introduced semantic tags. Specifically, they introduced new tags to represent new formats of media (`<canvas>`, `<video>`, `<audio>`), as well as ones to represent page structure (`<main>`, `<section>`, `<header>`, `<footer>`, `<nav>`, etc). For a full list of all available elements offered as of HTML 5, please refer to the [documentation](https://developer.mozilla.org/en-US/docs/Web/HTML/Element).

### Why are semantic tags better?

The goal of semantic HTML elements is to better represent the content within. A `<div>` can represent just about any block-level element, but it is better to be more specific.

#### Accessibility

Screen readers, parsers, and search engines that try to read your HTML and attempt to understand its structure. If you use semantic tags, you are giving these systems an easier time. Your audience is diverse and has a wide range of abilities/needs that you must address by including [ARIA attributes](http://html5doctor.com/using-aria-in-html/) in your HTML code. These attributes are necessary to ensure that accessibility API's can get the relevant information about your page. As of HTML5, semantic elements have the ARIA *role* built in by default. While it is important to consider ARIA attributes and include them whenever possible, using the right element for the content is a good bare minimum.

#### Readability

It also makes your code significantly easier to read by other humans. In a complex or large page with levels and levels of `<div>`s, it becomes impossible to understand the flow of information. Too many `<div>`s can fill up a page until it becomes unreadable. This is commonly referred to as _divitis_. Avoid it at all costs.

However, it is much easier to understand if you take advantage of elements like `<main>`, `<section>`, `<header>`, etc. because it helps you visualize the flow of information as you read through the HTML.

#### Styling

An additional benefit of using semantic tags, is that you can write more general CSS selectors. For a refresher on specificity of CSS selectors, read the [blog post](2016-08-02-why-you-should-never-use-important.md) I wrote on the subject). The benefit of general selectors is that you can establish a foundation of styles (for example, all `<section>`s have a blue background and 10 pixels of padding), that can easily be overwritten in a more specific situation, without having to add too many extra classes or incorporating `!important`.

It should be stated that this approach only works in certain cases, usually smaller or simpler UI's. Styling on tag selectors is considered a bad practice if the UI is too large to be appropriately represented as such. There are situations where it can work, but you are more likely going to need to use class selectors for your styling.  

### Frameworks and Templating

If you're using a front-end framework like Ember, you will notice that a lot of components will be wrapped in `<div>`s. This is ok, as the benefit of a `<div>` is that it's generic enough to be used everywhere. If you are committed to using semantic tags, you have two choices here. You can either wrap the template inside the component with the appropriate tag,

```handlebars
{{#some-component}}
  <section>
    <!-- section content goes here -->
  </section>
{{/some-component}}
```

This will render to the following HTML

```html
<div id="ember123">
  <section>
    <!-- section content goes here -->
  </section>
</div>
```

Another option (one that I personally prefer), is to change the `tagName` attribute of the component to match the appropriate semantic tag,

```handlebars
{{#some-component
  tagName='section'
}}
  <!-- section content goes here -->
{{/some-component}}
```

Which will render to

```html
<section id="ember123">
  <!-- section content goes here -->
</section>
```

The reason I prefer this option is because the wrapper `<div>` in choice 1 isn't actually necessary, and it can get in the way of some of the interesting things you can do with relative pseudo-selectors (for a refresher on what that is, here's the [blog post](2016-05-23-selectors-part-2-pseudo-classes.md) I wrote about it).

## Example

Here is an example of what the outline of a page would look like using semantic HTML5 elements.

```html
<header></header>
<nav></nav>
<main>
  <article>
    <section></section>
    <section></section>
  </article>
  <aside>
    <section></section>
    <section></section>
    <section></section>
  </aside>
</main>
<footer></footer>
```

Even without adding any CSS or seeing the content itself, you can easily visualize what the page will look like and how its structured.

For an example of how this might look in a page with content and CSS, check out this [codepen demo](http://codepen.io/raemadeline/pen/bwdgvJ)

## Conclusion

When in doubt, use the HTML element that most directly aligns with the purpose of the content within. `<div>`s have their time and place, but if it's at all possible to replace a `<div>` with a more descriptive element, please do so.

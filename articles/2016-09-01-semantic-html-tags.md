# :name_badge: Semantic HTML Tags

<!-- TODO(maddie): introduction -->

## The Basics

For this first section, I assume you know nothing about HTML so if you're a seasoned veteran I suggest you skip down to ["Getting More Advanced"](#getting-more-advanced).

### What are HTML tags?

### How do they fit together to form a page?

## Getting More Advanced

### HTML5

### Why are semantic tags better?

The goal of semantic HTML elements is to better represent the content within. A `<div>` can represent just about any block-level element, but it is better to be more specific. Screen readers, parsers, and search engines that try to read your HTML and attempt to understand its structure. If you use semantic tags. you are giving these systems an easier time.

It also makes your code significantly easier to read by other humans. In a complex or large page with levels and levels of `<div>`s, it becomes impossible to understand the flow of information. Too many `<div>`s can fill up a page until it becomes unreadable. This is called _divitis_. Avoid it at all costs.

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

The reason I prefer this option is because the wrapper `<div>` in choice 1 isn't actually necessary, and it can get away of some of the interesting things you can do with relative pseudo-selectors (for a refresher on what that is, here's the [blog post](2016-05-23-selectors-part-2-pseudo-classes.md) I wrote about it).

## Examples

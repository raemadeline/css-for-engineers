# :wrench: SASS Functions and Data Structures

As you've probably noticed by now, CSS isn't really a *programming language* by most people's standards. You can do a lot of really cool things, but you cannot do most of the things you learned in your Intro to Computer Science class (for loops, if/else, etc), and this bothers a lot of people who are used to thinking that way.

Enter [SASS](http://sass-lang.com/). I discussed it briefly in my previous article about [selectors](3_sass_selectors.md). The long and short of it is that this preprocessor has *superpowers*, and by that I mean it has one superpower and it is to magically give CSS the ability to do all the things we're accustomed to with other languages.

## Variables

Just like most other programming languages, you can declare variables in SASS and use them to style elements.

Heres a variable:

```scss
$maddies-favorite-color: #e1b719;

.foo {
  background-color: $maddies-favorite-color;
}
```

You can make just about anything into a variable in SASS, strings, colors, numbers, border styling, etc. Be careful though, because SASS isn't strict with variable types and it can get messy. To remedy this, it is always a good idea to be specific when naming variables. `$special-button` could refer to anything, but `$special-button-height` is much easier to recognize.

### Scope

There are both global and local variables. Global variables are defined outside of a selector and can be referenced anywhere. Local variables are defined inside a selector (typically a function or mixin) and can only be accessed inside that function. This is similar behavior to most other programming languages.

```scss
@mixin button-style {
  $button-bg-color: yellow;
  background-color: $button-bg-color;
}
```

It's also worth mentioning that local variables can be defined within a selector can be accessed by any selector nested inside it.

```scss
.container {
  $bg-color: hotpink;

  .inner-container {
    background-color: $bg-color;
  }
}
```

The example above will not throw a `Undefined variable` error.


### Lists

In the case when you have many variables that are all related to each other, you can define them as a list. You can define one-dimensional lists without parentheses, but it is easier to read and understand when they are included.

```scss
$states: (
  "AL", "DC", "DE", "FL", "GA", "IA", "IL",
  "IN", "KY", "LA", "MD", "MI", "MO", "MS",
  "NC", "NJ", "NY", "OH", "PA", "SC", "TN",
  "VA", "WV"
);
```

There are a bunch of standard list operations in SASS (`length`, `index`, `append`, `join`, etc.). The way to access individual values in the list is using `nth`:

```scss
// $result = "FL"
$result: nth($states, 4);
```

**Note that SASS lists are 1-indexed.** It's really annoying.

You can also nest lists inside each other, and treat them the same way you would treat multidimensional arrays in other languages.

```scss
$regions: (
  ( "DC", "DE", "MD", "NJ", "NY", "PA" ),
  ( "AL", "FL", "GA", "KY", "LA", "MS", "NC", "SC", "TN", "VA", "WV" ),
  ( "IA", "IL", "IN", "MI", "MO", "OH" )
);

// Once again, $result = "FL"
$result: nth(nth($regions, 2), 2);
```

### Maps

In addition to lists, we can also set up key-value pairs and maps in SASS. Here's an example of the syntax:

```scss
$widget: (
  "color": "blue",
  "width": 100px,
  "height": 400px
);
```

SASS is pretty loose with variable types, so these values in each key-value pair can be pretty much anything, strings values, other lists or maps, etc.

Similar to lists, you can also do all the standard map operations in SASS (`$map-get`, `$map-remove`, `$map-keys`, `$map-values`, `$map-has-key`, etc.)
Here is a [link](http://sass-lang.com/documentation/Sass/Script/Functions.html) to their documentation.

## Mixins and Functions

For the most part, anything you can do in other languages you can do using SASS functions. You get all the standard control directives (`@while`, `@for`, `@if`, etc.)

Here's an example using the `$states` list above.

```scss
@mixin states($states, $regions) {

  $region-colors: red, blue, green;

  @each $state in $states {
    @for $i from 1 through 3 {
      $region: nth($regions, $i);

      @if index($region, $state) {
        .#{$state}-label {
          color: nth($region-colors, $i);
        }
      }
    }
  }
}
```

Here is how that mixin would be added to a stylesheet.

```scss
.container {
  @include states($states, $regions);
}
```

This will result in the following CSS.

```scss
.container {
  .AL-label {
    color: blue;
  }

  .DC-label {
    color: red;
  }

  .DE-label {
    color: red;
  }
  ...
}
```

Here's a [codepen demo](http://codepen.io/raemadeline/pen/QEOzbG) to show that mixin in use.

For the most part mixins and functions are interchangeable, but their syntax is slightly different. Mixins are `@include`-ed, and functions are called to replace values. Here's the same mixin above but replaced by a function. Instead of declaring the style directly in the mixin, the color is returned as a value.

```scss
@function get-region-color($state) {
  $region-colors: red, blue, green;

  @for $i from 1 through 3 {
    $region: nth($regions, $i);

    @if index($region, $state) {
      @return nth($region-colors, $i);
    }
  }

  @return black;
}
```

Then to get the color, the function needs to be called

```scss
#NY {
  color: get-region-color("NY");
}
```


Here is an identical [codepen demo](http://codepen.io/raemadeline/pen/Krybaq) using the function instead of the mixin.

## In-Depth Example: Refactoring Z-Indexes

Last month, I refactored the way we do `z-index`es at Addepar to leverage the powers of SASS. This refactor encompassed all of the concepts I discussed above, so I thought it would make a good case study.

### What is a `z-index`?

The `z-index` of an element refers to its positioning in the z-axis. If you think of the x- and y- axes as the left-right / up-down directions on the screen, the z-axis is the direction going into and out of the screen. Another way of thinking about it is the way the element layers above or below other elements on the page.

![z-index]({{ site.baseurl }}/images/z_index.gif)

*Image courtesy of http://www.annyhe.com/*

Something to note that is not directly related to the larger topic of this post, but is related to `z-index`es: When applying a `z-index` to elements nested within each other, the child element's maximum place in the layering order is that of its parent, no matter how high the child element's `z-index` is.

For a visual representation of how that looks (Assume that boxes 2 and 3 are children of box 1):

![stacking order]({{ site.baseurl }}/images/stacking.png)

*Image courtesy of webdesign.tutsplus.com*

For this reason, its important to avoid putting `z-index`es on elements that don't absolutely need them, as it can lead to some unintended consequences.

### Why did I need to refactor them?

Initially, all of our `z-index`es were stored as individual variables in a file that looked like this:

```scss
$zindex-navbar:                  1000;		
$zindex-dropdown:                1000;		
$zindex-popover:                 1010;		
$zindex-tooltip:                 1030;		
$zindex-navbar-fixed:            1030;		
$zindex-modal-background:        1040;		
$zindex-modal:                   1050;
...
```

While these initial values are fairly well organized, this quickly fell into chaos as every new component or one-off change to a `z-index` just led to creating new variables with magic numbers, or worse, just putting the magic number directly into the declaration without explanation for why that element had to be layered that way.

### How did I fix it?

Now, our `z-index`es are organized in a nested map structure based on where they appear in the app.

```scss
$z-layers: (
  'base': 0,
  'input': 1,
  'tool-container': 3,
  'loading-animation': 999,
  'navbar': 1000,
  'modal': 1050,
  'time-series': 1060,
  'customer-support': 1070,
  'popover': 1080,
  'tooltip': 1090,
  'alert': 2000,
  'context-chooser': (
    'base': 2,
    'collapse': 4
  ),
  'icons': (
    'caret': 2,
    'sort': 2,
    'recon-expand-table': 100
  ),
  'portal': (
    'header': 3,
    'nav': 10
  ),
  'report': (
    'widgets': 2,
    'footer': 10,
    'container': 100,
    'drop-zone': 110,
    'header-cover': 400,
    'nav': 500,
    'item-outline': 1000,
    'text-overflow': 1120,
    'page': (
      'linked-cue': 500
    ),
    'table': (
      'squash-column': 3
    )
  )
);
```

You'll notice a `base` value for some nested maps. This is considered the "default" value for that section.

Many of the values in this map may seem arbitrary, and this is because I was careful to avoid messing with too many values. This refactor was mainly to keep the `z-index`es organized, and as we refactor each component we can adjust the values in this map accordingly.

I also created this function to access the values. This function takes any number of arguments, and traverses the structure by calling `map-get` and `map-deep-get`, an internal function outlined below.

```scss
@function z($first-layer, $secondary-layers...) {

  @if ($first-layer == null) {
    @return 0;
  }

  @if (length($secondary-layers) < 1) {
    $layer: map-get($z-layers, $first-layer);

    @if (type-of($layer) == 'map') {
      @return map-deep-get($z-layers, $first-layer, 'base');
    } @else {
      @return map-get($z-layers, $first-layer);
    }

  } @else {
    @return map-deep-get($z-layers, $first-layer, $secondary-layers...);
  }
}

@function map-deep-get($map, $keys...) {
  @each $key in $keys {
    $map: map-get($map, $key);
  }

  @return $map;
}
```

The intention of `z()` was to always return a reasonable `z-index` with minimal arguments. The first block of code just returns `0` when its called with no arguments, because the fewer elements that have defined `z-index`es the better. The next block addresses when only one argument is passed in. If that argument is the key for a map, it returns the default value, otherwise it returns the value associated with that key. In the case where there are two or more arguments, it calls `map-deep-get`, which traverses the map following each key in order until it returns the final value.

Then in any case when an element needs a `z-index`, it can be added like this:

```scss
.popover {
  z-index: z('popover');
}

.report-page-linked-cue {
  z-index: z('report', 'page', 'linked-cue');
}
```

### Why is this better?

By using a nested structure, it is easier to see the overall layering structure and keep track of it.

Another benefit is that you can do relative `z-index`es. When I started this refactor, I started by taking an inventory of every `z-index` in our entire CSS system. There were a lot of random magic number values floating around, and most of them were one or two off from a base value. This is because they wanted an element to be either directly above or below an adjacent element.

Now, instead of giving elements `.foo` and `.bar` `z-index`es of `x` and `x+1`,
you can now do

```scss
.bar {
  z-index: z('foo') + 1;
}
```

which makes it much more clear to anyone reading this code what this declaration is trying to accomplish.

Leveraging SASS allowed us to take a messy disorganized system and cut through the chaos, leaving behind a much more sustainable framework.

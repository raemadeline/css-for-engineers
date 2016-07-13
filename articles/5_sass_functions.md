# :wrench: SASS Functions and Data Structures

As you've probably noticed by now, CSS isn't really a *programming language* by most people's standards. You can do a lot of really cool things, but you cannot do most of the things you learned in your Intro to Computer Science class (for loops, if/else, etc), and this bothers a lot of people who are used to thinking that way.

Enter [SASS](http://sass-lang.com/). I discussed it briefly in my previous article about [selectors](3_sass-selectors.md).The long and short of it is that this preprocessor has *superpowers*, and by that I mean it has one superpower and it is to magically give CSS the ability to do all the things we're accustomed to with other languages.

## Variables

Just like most other programming languages, you can declare variables in SASS and use them to style elements.

Heres a variable:

```scss
$maddies-favorite-color: #e1b719;
```

And here is how you use it in styling:

```scss
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

In the case when you have many variables that are all related to each other, you can define them as a list.

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

  ...
}
```

Here's a [codepen demo](http://codepen.io/raemadeline/pen/QEOzbG) to show that mixin in use.

For the most part mixins and functions are interchangeable, but their syntax is slightly different. Mixins are `@include`-ed and functions are called to replace values. Here's the same mixin above but replaced by a function. Instead of declaring the style directly in the mixin, the color is returned as a value.

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

### What is a `z-index`?

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

### Why is this better?

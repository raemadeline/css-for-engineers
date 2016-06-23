# :wrench: SASS Functions and Data Structures

As you've probably noticed by now, CSS isn't really a *programming language* by most people's standards. You can do a lot of really cool things, but you cannot do most of the things you learned in your Intro to Computer Science class (for loops, if/else, etc), and this bothers a lot of people who are used to thinking that way.

Enter [SASS](http://sass-lang.com/). I discussed it briefly in my previous article about [selectors](3_sass-selectors.md).The long and short of it is that this preprocessor has *superpowers*, and by that I mean it has one superpower and it is to magically make CSS into a "real" programming language (note my sarcasm - CSS is a real language and anyone who says otherwise is mean and wrong).

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

### Lists

```scss
$states: (
  "AL", "DC", "DE", "FL", "GA", "IA", "IL",
  "IN", "KY", "LA", "MD", "MI", "MO", "MS",
  "NC", "NJ", "NY", "OH", "PA", "SC", "TN",
  "VA", "WV"
);
```

+ multidimensional

```scss
$regions: (
  ( "DC", "DE", "MD", "NJ", "NY", "PA" ),
  ( "AL", "FL", "GA", "KY", "LA", "MS", "NC", "SC", "TN", "VA", "WV" ),
  ( "IA", "IL", "IN", "MI", "MO", "OH" )
);
```

### Maps

<!-- Come up with better example -->
```scss
$name: (
  "testing": "hello!",
  "testing2": "world"
);
```

can get more complicated

```scss
$mega-social: (
  "facebook":		( content: "\f204", coords: 0 0 ),
  "twitter":		( content: "\f202", coords: 0 -64px ),
  "linkedin":		( content: "\f207", coords: 0 -128px )
);
```

## Functions

### Control Flow

@while

@for

@if / @else if / @else

@each

### List Operations

length, nth, join, append, index (starts at 1!)

### Map Operations

map-get, map-merge, map-remove, map-keys, map-values, map-has-key

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

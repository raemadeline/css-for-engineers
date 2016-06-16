# Flex

## Displays

A commonly used property in CSS is `display`. This specifies how the element is displayed (if it is shown at all). By default, the display value is either `block` or `inline`, depending on the element (inline elements are things like `h1`s, `p`s, `span`s, etc, and block elements are things like `div`s, `form`s, etc.)

There is also `display: inline-block`, which treats the element as `inline`, but the element can have a defined width and height, which `inline` elements cannot do.

To hide an element, you set it to `display: none`, this will remove the element from the page. The difference between `display: none` and `opacity: 0` (both of which hide the element) is that changing `opacity: 0` maintains the space the element takes up, whereas `display: none` does not. The element will take up no space.

For a demo of each of these `display`s, see this [codepen demo](http://codepen.io/raemadeline/pen/NNybrP)

## What is `flex` and why is it so cool?

Setting an element to `display: flex` is different than any of the other `display` options described above. This display allows all of the direct children of the element to move flexibly within the space the parent takes up. This is valuable for reusing components that might have vastly different content within (like panels!).

You might see `flex` in CSS labeled as `flex-box`, this is an older naming convention that has since been simplified to just `flex`.

Flex is generally supported by most major browsers, but depending on what you're trying to support you may need to use a vendor prefix. Either check out [CanIUse](http://caniuse.com/#feat=flexbox) or install an [Autoprefixer](https://github.com/postcss/autoprefixer) to handle it for you.

## How do I use it?

In addition to adding the property `display: flex` to your parent element, you have to do a bit more work to get the layout of your dreams to become a reality. Additional properties are required on the parent (and often the child elements as well).

### The Parent Element

Below are all the additional `flex` properties you can apply to the parent element. Note that none of these properties really work unless you include the property `display: flex` in your declaration.

#### `flex-direction`

#### `flex-wrap`

#### `flex-flow`

This is just a shorthand to combine `flex-direction` and `flex-wrap`. The syntax is:

```css
flex-flow: <'flex-direction'> || <'flex-wrap'>
```

#### `justify-content`

#### `align-items`

#### `align-content`

### The Child Elements

By applying the above properties to the parent element, you can create a lot of awesome layouts. However, you may occasionally want to customize a specific child element of the parent separately from the rest. These are properties you can add to the direct children of a flex parent.

#### `flex-grow`

#### `flex-shrink`

#### `flex-basis`

#### `flex`

This is a shorthand to combine `flex-grow`, `flex-shrink`, and `flex-basis`. The syntax is:

```css
flex: <'flex-grow'> || <'flex-shrink'> || <'flex-basis'>
```

#### `align-self`

This is similar to the parent property `align-items` in that they both align the element in the axis perpendicular to the `flex-direction`. The key difference here being that `align-self` overwrites the alignment for that element that the parent defined.

[Codepen Demo](http://codepen.io/raemadeline/pen/OXXBRO)

## Cool Examples

### Quick Centering

By giving the parent the properties:

```css
.parent {
  display: flex;
  justify-content: center;
  align-items: center;
}
```

Every direct child of that parent will automatically be centered within the parent without any additional CSS.

http://codepen.io/raemadeline/pen/Pzzpgm

### Full Page Layout

http://codepen.io/raemadeline/pen/pbbKWG

### Bonus: A Game!

If you feel like practicing using `flex` in your CSS, there's a [Tower Defense game](http://www.flexboxdefense.com/) to make that more fun.

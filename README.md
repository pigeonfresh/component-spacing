# component-spacing
Create spacing between components with ease 💅

## Give me the goods
If you know what you are doing you can [dive right in here](#usage).


## Introduction
When I develop UI for platforms with a CMS there is often a need for a system where the CMS Author or developer can create spacing (margins and/or paddings) between components. In some cases these need to be configurable in the CMS. This package provides a way to configure and manage these spacings manually or through classes added by a CMS.

## Installment
```shell
$ yarn add component-spacing
```
```shell
$ npm i -S component-spacing
```

And import it in your main scss files (mind the ~)
```scss
@import "~component-spacing";
```

After that you can configure your settings

## Configuration

### Vertical vs Horizontal
This package assumes you will want to use margins and paddings on the block-axis (for in man cases: top to bottom). If you want to create a spacing system for side scrolling websites you can set the vertical prop to true:
```scss
$vertical: true;
```
### Logical properties
By default, [logical properties](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Logical_Properties) are being used. This means that for `top`, `bottom`, `left` and `right` logical properties will be used like: `inline-start` and `block-end`. Many website will have a "top to bottom"-layout, but for "left to right" support `inline-start` and `inline-end` are very useful.

If you want to stick with `margin-top` and `padding-right` you can set the logical properties to `false`.

```scss
$logicalProperties: false;
```

Browser support for logical properties can be found [here](https://caniuse.com/?search=logical%20properties)  

🚨 Keep in mind that the lack of support in Safari is due to properties that are **not** used in this package. e.g. Safari in 12.1-14 doesn't support the shorthand versions of `padding-line` but **does support** `padding-inline-start` and `padding-inline-end`, which are used in this package.


### Creating spacings
This packages lets you create a system for paddings and/or margins 🤟🏼. You can create different padding and margins values if you want:
```scss
$componentMargin: (
  none: 0,
  default: 100px,
);

$componentPadding: (
  none: 0,
  default: 40px,
  large: 120px,
);
```
Or just use the same values:
```scss
$componentSpacing: (
  none: 0,
  small: 20px, 
  normal: 30px,
  large: 40px,
);

$componentPadding: $componentSpacing;
$componentMargin: $componentSpacing;
```

👉🏼 If you only need paddings or margins it's perfectly fine to just use one of them without they other

### Spacing Values

The values of these spacings don't have to be pixel values. It's perfectly fine to use custom properties, or fluid values:
```scss
$componentPadding: (
  none: 0,
  normal: var(--default-component-spacing),
  insane: 120vh,
  fluid: clamp(20px, 100vh, 2000px),
)
```

### Using mixins in SCSS
If you want to configure the spacing in your stylesheet and not depend on classNames being added by the CMS you can use mixins for this. The footer of a website is a nice example where you might not want to content author to control the spacing. You can still use your spacing system like so:

```scss
[data-component="site-footer"] {
  // Below is the same as: @include component-padding(medium, medium);
  @include component-padding(medium);
  @include component-margin(normal, none);
}
```
This will result in: 
```css
[data-component="site-footer"] {
  padding-block-start: var(--component-spacing);
  padding-block-end: var(--component-spacing);
  margin-block-start: var(--component-spacing);
  margin-block-end: 0;
}
```



### Setting defaults for scss
If you want you can set defaults for both margin and paddings. Now you can include the mixin without arguments.

```scss
$defaultMargin: none;
$defaultPadding: medium;

[data-component="rich-text"] {
  @include component-padding(); // medium padding
  @include component-margin(); // no margin
}
```
👉🏼 No need to set defaults if you are not planning on using them. 

### Creating spacing styles for use with classNames
In case the spacings are handled by a CMS it can be useful to dynamically render your configuration. This package lets you do that by configuring the output. With a mixin you can create those classes dynamically based on your configuration.

#### Compile styles

```scss
$componentSpacing: (
  none: 0,
  normal: 30px,
);

@include spacing-styles;
```
This will output the following class names (for the sake of example the styles are not shown):
```css
.margin-start-none {}

.margin-end-none {}

.margin-start-normal {}

.margin-end-normal {}

.padding-none {}

.padding-normal {}

.padding-start-none {}

.padding-end-none {}

.padding-start-normal {}

.padding-end-normal {}
```

The package assumes the CMS will configure both an end and start values. There are two variables to manipulate the styles that are being output:
```scss
$uniformSpacing: false;
$variableSpacing: true;
```

Setting `$uniformSpacing: true;` you will have the following additional styles:
```css
.padding-none {}

.padding-normal {}

.margin-none {}

.margin-normal {}
```

👉🏼 `$variableSpacing` is responsible for the compiling of the creation of the styles for start and end values like: `padding-start-normal` and `padding-end-normal`. If you only have uniform start- and end paddings and margins you can set `$variableSpacing: false;`.



If you only need margins or paddings you could also use the mixins `create-padding-styles` or `create-margin-styles`.

#### Create appended classNames

If you want to create more specificity you can use a nested mixin:
```scss
[data-component] {
  @include appended-spacing-styles();
}
```
Which will not nest the styles but append it to the parent:
```css
[data-component].padding-bottom-none {
  //
}
```
Or, in case you only need the paddings or margins:
```scss
[data-component] {
  // creates the margin padding styles
  @include appended-margin-styles();
  // creates the appended padding styles
  @include appended-padding-styles();
}
```

#### Configure classNames
By default, the classNames will be constructed by the following map:
```scss
$classNameConfig: (
  padding: 'padding',
  margin: 'margin',
  direction: 'start' 'end',
);
```
This will result into a className for the `normal` value like:
```scss
.padding-start-normal {
  padding-inline-start: 20px;
}
.padding-end-normal {
  padding-inline-end: 20px;
}
```

If you want to use different classNames you can use something like:
```scss
$classNameConfig: (
  padding: 'inner',
  margin: 'outer',
  direction: 'top-spacing' 'bottom-spacing',
);
```
This will result in styles like:
```css
.outer-top-spacing-normal {}

.outer-bottom-spacing-normal {}

.inner-top-spacing-normal {}

.inner-bottom-spacing-normal {}
```

## Usage:
If you know what you are doing just copy and paste the following in your project:

```scss
@import "~component-spacing";

// Variables
// Only keep the ones you are overwriting:
$vertical: true;
$logicalProperties: true;
$uniformSpacing: false;
$variableSpacing: true;
$classNameConfig: (
  padding: 'padding',
  margin: 'margin',
  direction: 'start' 'end',
);

// Update to match your own names and values:
$componentSpacing:(
  none: 0,
  default: 20px,
  large: 120px,
);

$componentMargin: $componentSpacing;
$componentPadding: $componentSpacing;

// Create default paddings for the use with mixins if needed
$defaultPadding: default;
$defaultMargin: large;
```





# component-spacing
Create spacing between components with ease 💅

## Introduction
When I develop UI for platforms with a CMS there is often a need for a system where the CMS Author or developer can create spacing (margins and/or paddings) between components. This package provides a way to configure and manage these spacings manually or through classes added by a CMS.

## TL;DR:
You can find the example configuration HERE



## Installment



## Configuration

### Creating spacings
component-spacing has been created in a way that it's flexible and easy to configure. You can create a map of keys and values for spacing.
```scss
$componentSpacing: (
  none: 0,
  small: 20px, 
  normal: 30px,
  large: 40px,
);
```
For both the padding and margin there is a way to use different names and values. 🤟🏼
```scss
$componentMargin: (none: 0, default: 20px);
$componentPadding: (none: 0, default: 40px, insane: 20vh);
```
If there is no `$componentMargin` or `$componentPadding` it will fallback to `$componentSpacing`.

### Spacing Values
The values of these spacings don't have to be pixel values. It's perfectly fine to use custom properties, or fluid values
```scss
$componentSpacing: (
  none: 0,
  small: var(--spacing-small),
  default: 20vh,
  insane: clamp(20px, 100vh, 2000px),
)
```

### Setting defaults for scss
If during development you encounter a situation where you have a default value you can consider configuring this by setting `$defaultComponentSpacing` to the key with the default value: 
```scss
$defaultComponentSpacing: normal;
```
It's possible to configure different defaults for margin and padding:
```scss
$defaultComponentMargin: none;
$defaultComponentPadding: medium;
```
They will fallback to `$defaultComponentSpacing` if their variable is not specified.

```scss
[data-component="rich-text"] {
  @include component-padding(); // medium padding
  @include component-margin(); // no margin
}
```


### Configuring class names
In case the spacings are handled by a CMS it can be useful to dynamically render your configuration. This package lets you do that by configuring the output. With a mixin you can create those classes dynamically based on your configuration.

```scss
$componentSpacing: (
  none: 0,
  normal: 30px,
);

@include createSpacingStyles;
```
This will output the following class names (for the sake of example the styles are not shown):
```css
.margin-none {}

.margin-normal {}

.margin-top-none {}

.margin-bottom-none {}

.margin-top-normal {}

.margin-bottom-normal {}

.padding-none {}

.padding-normal {}

.padding-top-none {}

.padding-bottom-none {}

.padding-top-normal {}

.padding-bottom-normal {}
```

If you only need margins or paddings you could also use the mixins `createPaddingStyles` or `createMarginStyles`.

If you want to create more specificity you can use a nested mixin:
```scss
[data-component] {
  @include createAppendedSpacingStyles();
}
```
Which will not nest the styles but append it to the parent:
```css
[data-component].padding-bottom-none {
  //
}
```



## Usage
A quick example of how to use this library (after configuration)


This can be done by the developer by:

```scss
[data-component="hero"] {
  @include component-margin(large);
  @include component-padding(medium);
}
// It's possible to have a different spacing for top and bottom by adding a second item
[data-component="footer"] {
  @include component-margin(large none);
  @include component-padding(medium large);
}
```

Or handled by the backend by adding classes that are compiled:
```scss
[data-component] {
  @include createAppendedSpacingStyles();
}
```
```html
<section class="padding-medium margin-top-none margin-bottom-large" data-component="hero"></section>
```

There are also options to only add appended styles for margins and paddings:
```scss
[data-component] {
  @include createAppendedMarginStyles();
  @include createAppendedPaddingStyles();
}
```

### Renaming class names
You can rename the class names. The name is build up from three pieces. Let's take the margin as an example:
`.margin-top-medium`. The class name is built up with the following variables:

```scss
$componentMargin: (
  none: 0,
  small: 30px,
  medium: 60px,
  large: 120px,
);

$spacingDirections: (
  top: 'top',
  bottom: 'bottom',
);

$marginClassName: 'margin';
```
If you need a class name like: 
```css
.inner-spacing-start-medium {
  //
}
.outer-spacing-end-large {
  //
}
```
You would create the following configuration:
```scss
$spacingDirections: (
  top: 'start',
  bottom: 'end',
);

$marginClassName: 'inner-spacing';
```

If you are not able to use custom properties, math functions (like `clamp()`) or relative values (😔) you might need to use this as a last resort:
```scss
$mobileSpacings: (
  none: 0,
  small: 10px,
  large: 30px,
);

$desktopSpacings: (
  none: 0,
  small: 60px,
  large: 120px,
);

[data-component] {
  @include createAppendedSpacingStyles($mobileSpacings);
  
  @media ('min-width: 1024px') {
    @include createAppendedSpacingStyles($desktopSpacings);   
  }
}
```






### Configuration setup example


## To-dos
-[ ] Create a version with logical properties instead of *-top/*-bottom values
-[ ] Create support for horizontal spacing for side scrolling websites
-[ ] Test Test Test
-[ ] Add more configuration options
  - [ ] Create config for directions

# component-spacing
Create spacing between components with ease 💅

## Introduction
When I develop UI for platforms with a CMS there is often a need for a system where the CMS Author or developer can create spacing (margins and/or paddings) between components. This package provides a way to configure and manage these spacings manually or through classes added by a CMS.

## Base example:
If you just want to get started and have everything installed you can just copy this and change/modify:
```scss
@import "~component-spacing";

$componentSpacing: (
  none: 0,
  small: 20px, 
  medium: 40px,
  large: 80px,
);

$defaultComponentSpacing: medium;

// The following variables are default so only copy them if you want to adjust them.
// This will create classNames like '.margin-top-large'

$spacingDirections: (top: 'top', bottom: 'bottom');
$marginClassName: 'margin';
$marginClassName: 'padding';

// If you want to create classNames you can use normal classNames or appended to its parent.
// .margin-top-large or [data-component].margin-top-large

@include create-spacing-styles();

[data-component] {
  @include create-appended-spacing-styles();
}

// if you want to use the component spacing in scss you can use the mixins like so:
.foo-component {
  @include component-margin(large);
  @include component-padding(none, medium);
}
```

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

[data-component="rich-text"] {
  @include component-padding(); // medium padding
  @include component-margin(); // no margin
}
```
They will fallback to `$defaultComponentSpacing` if their variable is not specified.


### Configuring class names
In case the spacings are handled by a CMS it can be useful to dynamically render your configuration. This package lets you do that by configuring the output. With a mixin you can create those classes dynamically based on your configuration.

```scss
$componentSpacing: (
  none: 0,
  normal: 30px,
);

@include create-spacing-styles;
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

If you only need margins or paddings you could also use the mixins `create-padding-styles` or `create-margin-styles`.

If you want to create more specificity you can use a nested mixin:
```scss
[data-component] {
  @include create-appended-spacing-styles();
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

// It's possible to have a different spacing for top and bottom by adding a second argument
[data-component="footer"] {
  @include component-margin(large, none);
  @include component-padding(medium, large);
}
```

Or handled by the backend by adding classes that are compiled:
```scss
[data-component] {
  @include create-appended-spacing-styles();
}
```
```html
<section class="padding-medium margin-top-none margin-bottom-large" data-component="hero"></section>
```

There are also options to only add appended styles for margins and paddings:
```scss
[data-component] {
  @include create-appended-margin-styles();
  @include create-appended-padding-styles();
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
  @include create-appended-spacing-styles($mobileSpacings);
  
  @media ('min-width: 1024px') {
    @include create-appended-spacing-styles($desktopSpacings);   
  }
}
```

## To-dos
-[ ] Create a version with logical properties instead of *-top/*-bottom values
-[ ] Create support for horizontal spacing for side scrolling websites
-[ ] Test Test Test
-[ ] Add more configuration options
  - [ ] Create config for directions

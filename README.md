ClearLess
=========

A reuseable collection of carefully-considered Less mixins to help make development faster and more maintainable.

The core tenets of this mixin library are to *avoid output bloat wherever possible* (via duplicated properties etc) and to *provide flexibile, configurable solutions* to the problems that are addressed by the library (i.e. by using Modernizr classes, browser hacks or not, etc). The aim is to give the author the benefits of reusable shortcuts without obliterating personal style and generating bloated stylesheets.

**Remember!** Just because there is a mixin for it doesn't mean you *need* to use it. If you have a individual case that needs the properties tweaking then it is often better to roll the solution by hand rather than to use the mixin and then override half the generated properties.

Usage
-----

Simply `@import` the `mixins/all.less` file into the top of your main Less file, and then (optionally) override any of the settings as described below.

The `mixins/all.less` file itself simply imports all the individual Less files into one place. The mixins and settings for these individual files are documented under in their various groupings below:

* [Settings](#global-settings)
* [Resets](#resets)
* [Shortcuts & Helpers](#shortcuts--helpers)
* [Typography](#typography)
* [Sprites](#sprites)
* [Icons](#icons)
* [Grids](#grids)



Global settings
---------------

ClearLess defines a few settings that affect the output of some of the mixins. These are just [Less variables](http://lesscss.org/#-variables), and have their default values defined in `mixins/settings.less`.

To override the defaults, simply redefine them after importing the mixins:

```css
@import "mixins/all";

@using-modernizr: true;
@base-font-size: 14;
```

The following global settings are defined:

### @using-modernizr

```css
@using-modernizr: false;
```

If using Modernizr, some of the mixins will swap to using the Modernizr-generated classes to determine support for various features and thus what CSS to generate (see the icon/sprites mixins for an example). Set this to `true` to enable this behaviour.

Modernizr feature tests currently used (if using a custom Modernizr build):

* Generated Content (`.generatedcontent`)

### @using-ieclasses

```css
@using-ieclasses: true;
```

If using [H5BP](http://html5boilerplate.com/)-style conditional comments to add IE-indentifying classes to the HTML element, some mixins will use them to patch IE support where there are known issues. If this is set to `false` then these mixins will fall back to hacks like the [star hack](http://en.wikipedia.org/wiki/CSS_filter#Star_hack) to provide this support instead.

**Other section-specific settings are documented alongside their mixins below.**

Resets
-------

Convinience mixins that should be used *outside of element selectors* as a quick way to generate popular reset stylesheet contents.

### .reset;

```css
.reset();
```

Outputs styles from the '[Meyer Reset](http://meyerweb.com/eric/tools/css/reset/)'.

### .normalize;

```css
.normalize();
```

Outputs styles from [normalize.css](http://necolas.github.com/normalize.css/).


Shortcuts & Helpers
-------------------

These are the most basic mixins. *Shortcuts* typically provide a quick way to generate all the required vendor-prefixed versions of a property, and/or give a more manageable syntax for defining things like CSS gradients. *Helpers* are mixins that typically generate boilerplate code for common use cases, such as `display: inline-block;` statements with IE7 fixes, or image replacement code.


### .border-radius()

Generates a box-radius property with the appropriate vendor prefixes.

```css
.border-radius( <@radius> );
```

* `@radius`: Radius to round all corners to. Defaults to 5px.

```css
.border-radius( <@top-left>, <@top-right>, <@bottom-left>, <@bottom-right>);
```

* `@top-left`: Radius to round the top-left corner to. Defaults to 5px.
* `@top-right`: Radius to round the top-right corner to. Defaults to 5px.
* `@bottom-left`: Radius to round the bottom-left corner to. Defaults to 5px.
* `@bottom-right`: Radius to round the bottom-right corner to. Defaults to 5px.

```css
/* Usage: */
.div1 {
	.border-radius( 5px );
}
.div2 {
	.border-radius( 5px, 7px, 5px, 10px );
}

/* Output: */
.div1 {
	-moz-border-radius: 5px;
	border-radius: 5px;
}
.div2 {
	-webkit-border-top-left-radius: 5px;
	-webkit-border-top-right-radius: 7px;
	-webkit-border-bottom-left-radius: 5px;
	-webkit-border-bottom-right-radius: 10px;
	-moz-border-radius-topleft: 5px;
	-moz-border-radius-topright: 7px;
	-moz-border-radius-bottomleft: 5px;
	-moz-border-radius-bottomright: 10px;
	border-top-left-radius: 5px;
	border-top-right-radius: 7px;
	border-bottom-left-radius: 5px;
	border-bottom-right-radius: 10px;
}
```


### .box-sizing()

Generates a box-sizing property with the appropriate vendor prefixes.

```css
.box-sizing( <@type> );
```

* `@type`: Box-sizing property value. Defaults to border-box.

```css
/* Usage: */
.div {
	.box-sizing( border-box );
}
/* Output: */
.div {
	-moz-box-sizing: border-box;
	-webkit-box-sizing: border-box;
	-ms-box-sizing: border-box;
	box-sizing: border-box;
}
```



Typography
-------

### @base-font-size

`@base-font-size: 16;`

A variable to sets the base font-size (in pixels) that is used as the default in certain calculations (such as in the pixels to rems conversion helper). Units should be omited.

Note that this doesn't stop you doing your base `body` font size calculations/definitions any way you please - this should just be set to the result of that. For instance, if you set your base font size to `62.5%` then just set the `@base-font-size` value to `10`.

### .font-size-rems()

Generates a font-size property with the pixel value converted to **ems**.

```css
.font-size-ems( <@px-size> [, <@context-px-size>] );
```

* `@px-size`: Font size (in pixels) to convert to ems.
* `@context-px-size`: *(Optional)* The font size (in pixels) of the current context. Defaults to the value of `@base-font-size` if not specified.

```css
/* Usage: */
p {
	.font-size-ems( 12 );
}
/* Output: */
p {
	font-size: 0.75em;
}
```

### .font-size-rems()

Generates a font-size property with the pixel value converted to **rems** and provides a pixel based fallback for browsers that do not support rem units.

```css
.font-size-rems( <@px-size> [, <@context-px-size>] );
```

* `@px-size`: Font size (in pixels) to convert to rems.
* `@context-px-size`: *(Optional)* The font size (in pixels) of the current context. Defaults to the value of `@base-font-size` if not specified.

```css
/* Usage: */
p {
	.font-size-rems( 12 );
}
/* Output: */
p {
	font-size: 12px;	
	font-size: 0.75rem;
}
```

Sprites
-------

Documentation coming soon.


Icons
-------

Documentation coming soon.


Grids
-------

Documentation coming soon.




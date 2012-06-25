ClearLess
=========

A reuseable collection of carefully-considered Less mixins to help make development faster and more maintainable.

The core tenets of this mixin library are to *avoid output bloat wherever possible* (via duplicated properties etc) and to *provide flexibile, configurable solutions* to the problems that are addressed by the library (i.e. by using Modernizr classes, browser hacks or not, etc). The aim is to give the author the benefits of reusable shortcuts without obliterating personal style and generating bloated stylesheets.

**Remember!** Just because there is a mixin for it doesn't mean you *need* to use it. If you have a individual case that needs the properties tweaking then it is often better to roll the solution by hand rather than to use the mixin and then override half the generated properties.

Usage
-----

Simply `@import` the `mixins/all.less` file into the top of your main Less file, and then override any of the settings as described below, if required.

Settings
--------

ClearLess defines a few settings that affect the output of some of the mixins. These are just [Less variables](http://lesscss.org/#-variables), and have their default values defined in `mixins/settings.less`.

To override the defaults, simply redefine them after importing the mixins:

```css
@import "mixins/all";

@using-modernizr: true;
@base-font-size: 14;
```

The following global settings are defined:

### @using-modernizr

`@using-modernizr: false;`

If using Modernizr, some of the mixins will swap to using the Modernizr-generated classes to determine support for various features and thus what CSS to generate (see the icon/sprites mixins for an example). Set this to `true` to enable this behaviour.

Modernizr feature tests currently used (if using a custom Modernizr build):

* Generated Content (`.generatedcontent`)

### @using-ieclasses

`@using-ieclasses: true;`

If using [H5BP](http://html5boilerplate.com/)-style conditional comments to add IE-indentifying classes to the HTML element, some mixins will use them to patch IE support where there are known issues. If this is set to `false` then these mixins will fall back to hacks like the [star hack](http://en.wikipedia.org/wiki/CSS_filter#Star_hack) to provide this support instead.

**Other section-specific settings are documented alongside their mixins below.**

Resets
-------

Convinience mixins that should be used *outside of element selectors* as a quick way to generate popular reset stylesheet contents.

### .reset;

`.reset();`

Outputs styles from the '[Meyer Reset](http://meyerweb.com/eric/tools/css/reset/)'.

### .normalize;

`.normalize();`

Outputs styles from [normalize.css](http://necolas.github.com/normalize.css/).


Shortcuts & Helpers
-------------------

These are the most basic mixins. *Shortcuts* typically provide a quick way to generate all the required vendor-prefixed versions of a property, and/or give a more manageable syntax for defining things like CSS gradients. *Helpers* are mixins that typically generate boilerplate code for common use cases, such as `display: inline-block;` statements with IE7 fixes, or image replacement code.



Typography
-------

### @base-font-size

`@base-font-size: 16;`

A variable to sets the base font-size (in pixels) that is used as the default in certain calculations (such as in the pixels to rems conversion helper). Units should be omited.

Note that this doesn't stop you doing your base `body` font size calculations/definitions any way you please - this should just be set to the result of that. For instance, if you set your base font size to `62.5%` then just set the `@base-font-size` value to `10`.

### .font-size-rems

Generates a font-size property the font-size in ems.

```css
.font-size-ems( <@px-size> [, <@context-px-size>] );
```

* `@px-size`: Font size (in pixels) to convert to ems.
* `@context-px-size`: *(Optional)* The font size (in pixels) of the current context. Defaults to the value of `@base-font-size` if not specified.

```css
p {
	.font-size-ems( 12 );
}
```

### .font-size-rems

Calculates the font-size in rems and provides a pixel based fallback for browsers that do not support rem units.

```css
.font-size-rems( <@px-size> [, <@context-px-size>] );
```

* `@px-size`: Font size (in pixels) to convert to rems.
* `@context-px-size`: *(Optional)* The font size (in pixels) of the current context. Defaults to the value of `@base-font-size` if not specified.

```css
p {
	.font-size-rems( 12 );
}
```

Grids
-------

Documentation coming soon.


Sprites
-------

Documentation coming soon.


Icons
-------

Documentation coming soon.






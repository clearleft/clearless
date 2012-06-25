ClearLess
=========

A reuseable collection of carefully-considered Less mixins to help make development faster and more maintainable.

The core tenets of this mixin library are to *avoid output bloat wherever possible* (via duplicated properties etc) and to *provide flexibile, configurable solutions* to the problems that are addressed by the library (i.e. by using Modernizr classes, browser hacks or not, etc). The aim is to give the author the benefits of reusable shortcuts without obliterating personal style and generating bloated stylesheets.

**Remember!** Just because there is a mixin for it doesn't mean you *need* to use it. If you have a individual case that needs the properties tweaking then it is often better to roll the solution by hand rather than to use the mixin and then override half the generated properties.

**All of the examples below show sample output from the mixins.** It's definitely advised that you familiarise yourself with the output so you can judge whether or not to use the mixin in different situations.


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

### @disable-filters

```css
@disable-filters: true;
```

Whether or not certain mixins (like the gradient mixins) should generate IE fallbacks using the MS proprietary `filter` property or not. Disabled by default due to performance issues with filter props.

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

Generates a `box-radius` property with the appropriate vendor prefixes.

```css
.border-radius( <@radius> );
```

* `@radius`: Radius to round all corners to. Defaults to `5px`.

```css
.border-radius( <@top-left>, <@top-right>, <@bottom-left>, <@bottom-right>);
```

* `@top-left`: Radius to round the top-left corner to. Defaults to `5px`.
* `@top-right`: Radius to round the top-right corner to. Defaults to `5px`.
* `@bottom-left`: Radius to round the bottom-left corner to. Defaults to `5px`.
* `@bottom-right`: Radius to round the bottom-right corner to. Defaults to `5px`.

```css
/* Usage: */
.example1 {
	.border-radius( 5px );
}
.example2 {
	.border-radius( 5px, 7px, 5px, 10px );
}
/* Example output: */
.example1 {
	-moz-border-radius: 5px;
	border-radius: 5px;
}
.example2 {
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

Generates a `box-sizing` property with the appropriate vendor prefixes.

```css
.box-sizing( [<@type>] );
```

* `@type`: box-sizing property value. Defaults to `border-box`.

```css
/* Usage: */
.example {
	.box-sizing( border-box );
}
/* Example output: */
.example {
	-moz-box-sizing: border-box;
	-webkit-box-sizing: border-box;
	-ms-box-sizing: border-box;
	box-sizing: border-box;
}
```

### .box-shadow()

Generates a `box-shadow` property with the appropriate vendor prefixes.

```css
.box-shadow( [<@shadow>] );
```

* `@shadow`: box-shadow property value. Defaults to `1px 1px 2px rgba(0,0,0,0.25)`.

```css
/* Usage: */
.example {
	.box-shadow( 2px 2px 3px #999 );
}
/* Example output: */
.example {
	-webkit-box-shadow: 2px 2px 3px #999;
	-moz-box-shadow: 2px 2px 3px #999;
	box-shadow: 2px 2px 3px #999;
}
```

### .transition()

Generates a `transition` property with the appropriate vendor prefixes.

```css
.transition( <@transition> );
```

* `@transition`: transition property value.

```css
/* Usage: */
.example {
	.transition( all .2s ease-in-out );
}
/* Example output: */
.example {
	-webkit-transition: all .2s ease-in-out;
	-moz-transition: all .2s ease-in-out;
	transition: all .2s ease-in-out;
}
```

### .rotate()

Generates a `transform` property with a rotation value and with the appropriate vendor prefixes.

```css
.rotate( <@rotation> );
```

* `@rotation`: rotation value.

```css
/* Usage: */
.example {
	.rotate( 2.5deg );
}
/* Example output: */
.example {
	-webkit-transform: rotate(2.5deg);
	-moz-transform: rotate(2.5deg);
	-o-transform: rotate(2.5deg);
	transform: rotate(2.5deg);
}
```

### .placeholder()

Generates pseudo-selector rules to globally change the colour of placeholder text for inputs. Use outside of element selectors.

```css
.placeholder( [<@color>] );
```

* `@color`: colour value. Defaults to `#DDD`

```css
/* Usage: */
.placeholder( #F00 );
/* Example output: */
:-moz-placeholder {
	color: #F00;
}
::-webkit-input-placeholder {
	color: #F00;
}
```

### #gradient > .vertical()

Uses CSS3 gradient values to generate vertical background gradients with appropriate vendor prefixed implementations. Output varies according to the value of the `@disable-filters` setting.

```css
#gradient > .vertical( <@start-color>, <@end-color> );
```

* `@start-color`: Colour value for the upper start colour.
* `@end-color`: Colour value for the bottom end colour.

```css
/* Usage: */
.example {
	#gradient > .vertical( #F00, #555);
}
/* Example output: */
.example {
	background-color: #555;
	background-repeat: repeat-x;
	background-image: -khtml-gradient(linear, left top, left bottom, from(#F00), to(#555));
	background-image: -moz-linear-gradient(#F00, #555);
	background-image: -ms-linear-gradient(#F00, #555);
	background-image: -webkit-gradient(linear, left top, left bottom, color-stop(0%, #F00), color-stop(100%, #555));
	background-image: -webkit-linear-gradient(#F00, #555);
	background-image: -o-linear-gradient(#F00, #555);
	background-image: -ms-linear-gradient(top, #F00 0%, #555 100%);
}
```

### #gradient > .horizontal()

Uses CSS3 gradient values to generate horizontal background gradients with appropriate vendor prefixed implementations. Output varies according to the value of the `@disable-filters` setting.

```css
#gradient > .horizontal( <@start-color>, <@end-color> );
```

* `@start-color`: Colour value for the left start colour.
* `@end-color`: Colour value for the right end colour.

```css
/* Usage: */
.example {
	#gradient > .horizontal( #F00, #555);
}
/* Example output: */
.example {
	background-color: #555;
	background-repeat: repeat-x;
	background-image: -khtml-gradient(linear, left top, right top, from(#F00), to(#555));
	background-image: -moz-linear-gradient(left, #F00, #555);
	background-image: -ms-linear-gradient(left, #F00, #555);
	background-image: -webkit-gradient(linear, left top, right top, color-stop(0%, #F00), color-stop(100%, #555));
	background-image: -webkit-linear-gradient(left, #F00, #555);
	background-image: -o-linear-gradient(left, #F00, #555);
	background-image: -ms-linear-gradient(left, #F00 0%, #555 100%);
	background-image: linear-gradient(left, #F00, #555);
}
```

### .clearfix()

Generates the appropriate properies to apply the [micro-clearfix hack](http://nicolasgallagher.com/micro-clearfix-hack/) to the element. Output varies according to the value of the `@using-ieclasses` setting.

```css
.clearfix();
```

```css
/* Usage: */
.example {
	.clearfix();
}
/* Example output: */
.example:before,
.example:after {
	content: "";
	display: table;
}
.example:after {
	clear: both;
}
.ie6 .example,
.ie7 .example {
	zoom: 1;
}
```

### .inline-block()

Helper to generate the inline-block display property plus fixes for IE7. Output varies according to the value of the `@using-ieclasses` setting.

```css
.inline-block();
```

```css
/* Usage: */
.example {
	.inline-block();
}
/* Example output: */
.example {
	display: inline-block
}
.ie6 .example,
.ie7 .example {
	display: inline;
	zoom: 1;
}
```


### .ir()

Generates text-removing properties for use in image replacement. Does not specify the background image (or it's positioning) itself - this needs to be specified manually (or use one of the sprite mixins, if appropriate).

```css
.ir();
```

```css
/* Usage: */
.example {
	.ir();
	background-image: url('/text.png');
}
/* Example output: */
.example {
	border: 0;
	font: 0/0 a;
	text-shadow: none;
	color: transparent;
	background-color: transparent;
	background-image: url('/text.png');
}
```
### .hidden()

Hides content from the page. Uses `!important` to override inline-styles added by JS.

```css
.hidden();
```

```css
/* Usage: */
.example {
	.hidden();
}
/* Example output: */
.example {
	display: none !important;
	visibility: hidden;
}
```

### .visually-hidden()

Hides content visually, but leaves is accessible to screenreaders. Also generates helper classes to allow the element to be focusable when navigated to via the keyboard.

```css
.visually-hidden();
```

```css
/* Usage: */
.example {
	.visually-hidden();
}
/* Example output: */
.example {
	border: 0;
	clip: rect(0 0 0 0);
	height: 1px;
	margin: -1px;
	overflow: hidden;
	padding: 0;
	position: absolute;
	width: 1px;
}
.example.focusable:active,
.example.focusable:focus {
	clip: auto;
	height: auto;
	margin: 0;
	overflow: visible;
	position: static;
	width: auto;
}
```

### .size()

Shortcut for generating width and height properties.

```css
.size( <@size> );
```

* `@size`: Value to use for the height and width properties.

```css
.size( <@width>, <@height> );
```

* `@width`: Value to use for the width property
* `@height`: Value to use for the height property

```css
/* Usage: */
.example1 {
	.size( 30px );
}
.example2 {
	.size( 20px, 70px );
}
/* Example output: */
.example1 {
	width: 30px;	
	height: 30px;
}
.example2 {
	width: 20px;
	height: 70px;
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
/* Example output: */
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
/* Example output: */
p {
	font-size: 12px;	
	font-size: 0.75rem;
}
```

Sprites
-------

The sprite mixins give you an easy way to use sprited background images. It assumes the use of a single sprite image with individual images placed on a regular grid. These are defined as settings variables, but can also be supplied on a per-mixin basis.

### @sprite-image

```css
@sprite-image: url('/images/example-sprite.png');
```

The default image to use for the sprite mixins. Can be a Base64 encoded data-uri.

### @sprite-grid

```css
@sprite-grid: 50px;
```

The size of the grid (in pixels) that the individal images are placed on.

### .sprite()

The most basic sprite mixin. Outputs all the required properties to generate your sprited image.

```css
.sprite(<@x>, <@y>[, <@sprite-image>[, <@sprite-grid>]]);
```

* `@x`: The x coordinate of the desired image on the grid.
* `@y`: The y coordinate of the desired image on the grid.
* `@sprite-image`: The sprite image to use. Defaults to the globally defined `@sprite-image` value.
* `@sprite-grid`: The grid size used in the sprite. Defaults to the globally defined `@sprite-grid` value.

```css
/* Usage: */
.example {
	.sprite( 2, 3 );
}
/* Example output: */
.example {
	background-image: url('/images/example-sprite.png');
	background-repeat: no-repeat;
	background-position: -100px  -150px;
}
```

### .sprite-sized()

The same as the `.sprite()` mixin, but allows you to specify the height and width to constrain the element by.

```css
.sprite(<@x>, <@y>, <@width>, <@height>[, <@sprite-image>[, <@sprite-grid>]]);
```

* `@x`: The x coordinate of the desired image on the grid.
* `@y`: The y coordinate of the desired image on the grid.
* `@width`: The width of the image.
* `@height`: The height of the image.
* `@sprite-image`: The sprite image to use. Defaults to the globally defined `@sprite-image` value.
* `@sprite-grid`: The grid size used in the sprite. Defaults to the globally defined `@sprite-grid` value.

```css
/* Usage: */
.example {
	.sprite( 2, 3, 16px, 32px );
}
/* Example output: */
.example {
	background-image: url('/images/example-sprite.png');
	background-repeat: no-repeat;
	background-position: -100px  -150px;
	width: 16px;
	height: 32px;
}
```





Icons
-------

Documentation coming soon.


Grids
-------

Documentation coming soon.


Credits
-------

Many of the mixins, styles and other parts of this library were shamelessly poached from other open-source projects, including Mark Otto's [Preboot](http://markdotto.com/bootstrap/) and the [HTML5 Boilerplate](html5boilerplate.com).

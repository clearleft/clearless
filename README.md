ClearLess
=========

A reuseable collection of carefully-considered [Less](http://lesscss.org/) mixins, or *YALML* (Yet Another Less Mixin Library).

The core tenets of this mixin library are to *avoid output bloat wherever possible* (via duplicated properties etc) and to *provide flexibile, configurable solutions* to the problems that are addressed by the library (i.e. by using Modernizr classes, browser hacks or not, etc). The aim is to give the author the benefits of reusable shortcuts without obliterating personal style and generating bloated stylesheets.

**Before diving in** it is strongly recommended that you peruse the [notes on usage and best practices](#some-notes-on-usage-and-best-practices) at the end of this document, which gives an overview of how you can take full advantage of ClearLess without compromising the generated CSS output.

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
* [Arrows](#arrows)
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

### SETTING: @using-modernizr

```css
@using-modernizr: false;
```

If using Modernizr, some of the mixins will swap to using the Modernizr-generated classes to determine support for various features and thus what CSS to generate (see the icon/sprites mixins for an example). Set this to `true` to enable this behaviour.

Modernizr feature tests currently used (if using a custom Modernizr build):

* Generated Content (`.generatedcontent`)

### SETTING: @using-ieclasses

```css
@using-ieclasses: true;
```

If using [H5BP](http://html5boilerplate.com/)-style conditional comments to add IE-indentifying classes to the HTML element, some mixins will use them to patch IE support where there are known issues. If this is set to `false` then these mixins will fall back to hacks like the [star hack](http://en.wikipedia.org/wiki/CSS_filter#Star_hack) to provide this support instead.

### SETTING: @disable-filters

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

* [.border-radius()](#border-radius)
* [.box-sizing()](#box-sizing)
* [.box-shadow()](#box-shadow)
* [.filter()](#filter)
* [.transition()](#transition)
* [.rotate()](#rotate)
* [.placeholder()](#placeholder)
* [#gradient > .vertical()](#gradient--vertical)
* [#gradient > .horizontal()](#gradient--horizontal)
* [.clearfix()](#clearfix)
* [.inline-block()](#inline-block)
* [.ir()](#ir)
* [.hidden()](#hidden)
* [.visually-hidden()](#visually-hidden)
* [.size()](#size)

### .border-radius()

Generates a `box-radius` property with the appropriate vendor prefixes.

```css
.border-radius( <@radius> );
```

* `@radius`: *(Optional)* Radius to round all corners to. Defaults to `5px`.

```css
/* Usage: */
.example1 {
	.border-radius( 5px );
}
.example2 {
	.border-radius( 5px 7px 5px 10px );
}
/* Example output: */
.example1 {
	-webkit-border-radius: 5px;
	-moz-border-radius: 5px;
	border-radius: 5px;
}
.example2 {
	-webkit-border-radius: 5px 7px 5px 10px;
	-moz-border-radius: 5px 7px 5px 10px;
	border-radius: 5px 7px 5px 10px;
}
```

### .box-sizing()

Generates a `box-sizing` property with the appropriate vendor prefixes.

```css
.box-sizing( [<@type>] );
```

* `@type`: *(Optional)* box-sizing property value. Defaults to `border-box`.

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

* `@shadow`: *(Optional)* box-shadow property value. Defaults to `1px 1px 2px rgba(0,0,0,0.25)`.

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

### .filter()

Generates a `filter` property with the appropriate vendor prefixes.

```css
.filter( [<@filter>] );
```

* `@filter`: *(Optional)* filter property value. Defaults to `grayscale(100%)`.

```css
/* Usage: */
.example {
	.filter( sepia(50%) );
}
/* Example output: */
.example {
	-webkit-filter: sepia(50%);
	-moz-filter: sepia(50%);
	-ms-filter: sepia(50%);
	-o-filter: sepia(50%);
	filter: sepia(50%);
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

* `@color`: *(Optional)* colour value. Defaults to `#DDD`

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

* [@base-font-size](#setting-base-font-size)
* [.font-size-ems()](#font-size-ems)
* [.font-size-rems()](#font-size-rems)
* [.word-wrap()](#word-wrap)

### SETTING: @base-font-size

`@base-font-size: 16;`

A variable to sets the base font-size (in pixels) that is used as the default in certain calculations (such as in the pixels to rems conversion helper). Units should be omited.

Note that this doesn't stop you doing your base `body` font size calculations/definitions any way you please - this should just be set to the result of that. For instance, if you set your base font size to `62.5%` then just set the `@base-font-size` value to `10`.

### .font-size-ems()

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
.font-size-rems( <@px-size> );
```

* `@px-size`: Font size (in pixels) to convert to rems.

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

### .font-face()

Generates a font-face declaration block. Should be used outside of any element selectors. Note that this assumes that you have generated EOT, WOFF and TTF versions of the font, and that they all live in the same location, with the same filename (apart from the extension.

Future versions of this may include workarounds to make this more flexible. 

```css
.font-face( <@font-family>, <@font-path>, <@font-weight>, <@font-style>, <@include-svg> );
```

* `@font-family`: The name to give the font-family
* `@font-path`: The path/URL to the font, including the font name but **with the file extension removed**
* `@font-weight`: *(Optional)* Value for the font-weight property. Defaults to `normal`
* `@font-style`: *(Optional)* Value for the font-style property. Defaults to `normal`
* `@include-svg`: *(Optional)* Value for the font-weight property. Defaults to `normal`

```css
/* Usage: */
.font-face( 'MyNiceFontBold', '/fonts/my_nice_font', bold );
/* Example output: */
@font-face {
    font-family: 'MyNiceFontBold';
    src: url('/fonts/my_nice_font.eot');
    src: url('/fonts/my_nice_font.eot?#iefix') format('embedded-opentype'),
         url('/fonts/my_nice_font.woff') format('woff'),
         url('/fonts/my_nice_font.ttf') format('truetype');
    font-weight: bold;
    font-style: normal;
}
```

### .word-wrap()

Implement word-wrapping for text within the element, with hyphenation where supported. See [http://blog.kenneth.io/blog/2012/03/04/word-wrapping-hypernation-using-css/](Kenneth Auchenberg's blog post) for details.

```css
.word-wrap();
```

```css
/* Usage: */
.example {
	.word-wrap();
}
/* Example output: */
.example {
	-ms-word-break: break-all;
	word-break: break-all;
	word-break: break-word;
	-webkit-hyphens: auto;
	-moz-hyphens: auto;
	hyphens: auto;
}
```


Sprites
-------

The sprite mixins give you an easy way to use sprited background images. It assumes the use of a single sprite image with individual images placed on a regular grid. These are defined as settings variables, but can also be supplied on a per-mixin basis.

* [@sprite-image](#setting-sprite-image)
* [@sprite-grid](#setting-sprite-grid)
* [.sprite()](#sprite)
* [.sprite-sized()](#sprite-sized)
* [.sprite-ir()](#sprite-ir)
* [.sprite-image()](#sprite-image)
* [.sprite-pos()](#sprite-pos)
* [.sprite-pos-sized()](#sprite-pos-sized)

### SETTING: @sprite-image

```css
@sprite-image: '/images/example-sprite.png';
```

The default image to use for the sprite mixins. Can be a Base64 encoded data-uri.

### SETTING: @sprite-grid

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
* `@sprite-image`: *(Optional)* The sprite image to use. Defaults to the globally defined `@sprite-image` value.
* `@sprite-grid`: *(Optional)* The grid size used in the sprite. Defaults to the globally defined `@sprite-grid` value.

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

The same as the `.sprite()` mixin above, but allows you to specify the height and width to constrain the element by.

```css
.sprite(<@x>, <@y>, <@width>, <@height>[, <@sprite-image>[, <@sprite-grid>]]);
```

* `@x`: The x coordinate of the desired image on the grid.
* `@y`: The y coordinate of the desired image on the grid.
* `@width`: The width of the image.
* `@height`: The height of the image.
* `@sprite-image`: *(Optional)* The sprite image to use. Defaults to the globally defined `@sprite-image` value.
* `@sprite-grid`: *(Optional)* The grid size used in the sprite. Defaults to the globally defined `@sprite-grid` value.

```css
.sprite(<@x>, <@y>, <@size>[, <@sprite-image>[, <@sprite-grid>]]);
```

* `@x`: The x coordinate of the desired image on the grid.
* `@y`: The y coordinate of the desired image on the grid.
* `@size`: The height and width of the image (for 'square' images).
* `@sprite-image`: *(Optional)* The sprite image to use. Defaults to the globally defined `@sprite-image` value.
* `@sprite-grid`: *(Optional)* The grid size used in the sprite. Defaults to the globally defined `@sprite-grid` value.


```css
/* Usage: */
.example {
	.sprite-sized( 2, 3, 16px, 32px );
}
.example2 {
	.sprite-sized( 2, 3, 16px );
}
/* Example output: */
.example {
	background-image: url('/images/example-sprite.png');
	background-repeat: no-repeat;
	background-position: -100px  -150px;
	width: 16px;
	height: 32px;
}
.example2 {
	background-image: url('/images/example-sprite.png');
	background-repeat: no-repeat;
	background-position: -100px  -150px;
	width: 16px;
	height: 16px;
}
```

### .sprite-ir()

Augments the `.sprite-sized()` mixin to include image replacement properties as defined in the `.ir()` mixin.

```css
.sprite-ir(<@x>, <@y>, <@width>, <@height>[, <@sprite-image>[, <@sprite-grid>]]);
```

* `@x`: The x coordinate of the desired image on the grid.
* `@y`: The y coordinate of the desired image on the grid.
* `@width`: The width of the image.
* `@height`: The height of the image.
* `@sprite-image`: *(Optional)* The sprite image to use. Defaults to the globally defined `@sprite-image` value.
* `@sprite-grid`: *(Optional)* The grid size used in the sprite. Defaults to the globally defined `@sprite-grid` value.

```css
.sprite-ir(<@x>, <@y>, <@size>[, <@sprite-image>[, <@sprite-grid>]]);
```

* `@x`: The x coordinate of the desired image on the grid.
* `@y`: The y coordinate of the desired image on the grid.
* `@size`: The height and width of the image (for 'square' images).
* `@sprite-image`: *(Optional)* The sprite image to use. Defaults to the globally defined `@sprite-image` value.
* `@sprite-grid`: *(Optional)* The grid size used in the sprite. Defaults to the globally defined `@sprite-grid` value.

```css
/* Usage: */
.example {
	.sprite-ir( 2, 3, 16px, 32px );
}
.example2 {
	.sprite-ir( 2, 3, 16px );
}
/* Example output: */
.example {
	background-image: url('/images/example-sprite.png');
	background-repeat: no-repeat;
	background-position: -100px  -150px;
	width: 16px;
	height: 32px;
	border: 0;
	font: 0/0 a;
	text-shadow: none;
	color: transparent;
	background-color: transparent;
}
.example2 {
	background-image: url('/images/example-sprite.png');
	background-repeat: no-repeat;
	background-position: -100px  -150px;
	width: 16px;
	height: 16px;
	border: 0;
	font: 0/0 a;
	text-shadow: none;
	color: transparent;
	background-color: transparent;
}
```

### .sprite-image()

A [partial mixin](#optimising-output-using-partial-mixins). Just sets the background-image property to the `@sprite-image`. Useful in combination with the other sprite partial mixins below.

```css
.sprite-image([<@sprite-image>]);
```

* `@sprite-image`: *(Optional)* The sprite image to use. Defaults to the globally defined `@sprite-image` value.

```css
/* Usage: */
.example {
	.sprite-image();
}
/* Example output: */
.example {
	background-image: url('/images/example-sprite.png');
	background-repeat: no-repeat;
}
```

### .sprite-pos()

A [partial mixin](#optimising-output-using-partial-mixins). Generates the correct background-position property according to the position and grid. Useful in combination with the other sprite partial mixins.

```css
.sprite-pos(<@x>, <@y>);
```

* `@x`: The x coordinate of the desired image on the grid.
* `@y`: The y coordinate of the desired image on the grid.

```css
/* Usage: */
.example {
	.sprite-pos(2,3);
}
/* Example output: */
.example {
	background-position: -100px  -150px;
}
```

### .sprite-pos-sized()

Similar to the `.sprite-pos()` [partial mixin](#optimising-output-using-partial-mixins) above, but includes the ability to set the size of the element.

```css
.sprite-pos-sized(<@x>, <@y>, <@width>, <@height>);
```

* `@x`: The x coordinate of the desired image on the grid.
* `@y`: The y coordinate of the desired image on the grid.
* `@width`: The width of the image.
* `@height`: The height of the image.

```css
.sprite-pos-sized(<@x>, <@y>, <@size>);
```

* `@x`: The x coordinate of the desired image on the grid.
* `@y`: The y coordinate of the desired image on the grid.
* `@size`: The width of the image.

```css
/* Usage: */
.example {
	.sprite-pos-sized( 2, 3, 16px, 32px );
}
.example2 {
	.sprite-pos-sized( 2, 3, 16px );
}
/* Example output: */
.example {
	background-position: -100px  -150px;
	width: 16px;
	height: 32px;
}
.example2 {
	background-position: -100px  -150px;
	width: 16px;
	height: 16px;
}
```

Icons
-------

The icons mixins let you easily place an icon before or after an element, using absolutely positioned :before and :after pseudo elements to display them. There are also sprited icon mixins build on the sprite mixins above.

The exact output of all the icon mixins depends on the value of the `@using-modernizer` setting.

* [.prepend-icon()](#prepend-icon)
* [.append-icon()](#append-icon)
* [.prepend-sprite-icon()](#prepend-sprite-icon)
* [.append-sprite-icon()](#append-sprite-icon)
* [.prepend-sprite-icon-pos()](#prepend-sprite-icon-pos)
* [.append-sprite-icon-pos()](#append-sprite-icon-pos)
* [.prepend-icon-setup()](#prepend-icon-setup)
* [.append-icon-setup()](#append-icon-setup)
* [.prepend-icon-image()](#prepend-icon-image)
* [.append-icon-image()](#append-icon-image)

### .prepend-icon()

Prepends an icon to the element it's called on.

```css
.prepend-icon( <@icon-image>, <@width>, <@height>[, <@nudge-left>[, <@nudge-top>[, <@pad>]]] );
```

* `@icon-image`: URL or data URI of an image to use for the prepended icon
* `@width`: Width of the image
* `@height`: Height of the image
* `@nudge-left`: *(Optional)* The value of the `left` property for the icon. Defaults to `0`.
* `@nudge-top`: *(Optional)* The value of the `top` property for the icon. Defaults to `0`.
* `@pad`: *(Optional)* Left-padding (in addition to the width of the icon) to apply to the element. Defaults to `10px`

```css
/* Usage: */
.example {
	.prepend-icon( 'img/icon.png', 16px, 32px );
}
/* Example output: */
.example {
	position: relative;
	padding-left: 42px;
}
.example:before {
	position: absolute;
	display: block;
	content: ' ';
	background: url('img/icon.png') no-repeat 0 0;
	width: 16px;
	height: 32px;
	top: 0;
	left: 0;
}
```

### .append-icon()

Appends an icon after the element it's called on.

```css
.append-icon( <@icon-image>, <@width>, <@height>[, <@nudge-right>[, <@nudge-top>[, <@pad>]]] );
```

* `@icon-image`: URL or data URI of an image to use for the prepended icon
* `@width`: Width of the image
* `@height`: Height of the image
* `@nudge-right`: *(Optional)* The value of the `right` property for the icon. Defaults to `0`.
* `@nudge-top`: *(Optional)* The value of the `top` property for the icon. Defaults to `0`.
* `@pad`: *(Optional)* Right-padding (in addition to the width of the icon) to apply to the element. Defaults to `10px`

```css
/* Usage: */
.example {
	.append-icon( 'img/icon.png', 16px, 32px );
}
/* Example output: */
.example {
	position: relative;
	padding-right: 42px;
}
.example:after {
	position: absolute;
	display: block;
	content: ' ';
	background: url('img/icon.png') no-repeat 0 0;
	width: 16px;
	height: 32px;
	top: 0;
	right: 0;
}
```

### .prepend-sprite-icon()

Prepends an icon taken from the sprite to the element it's called on.

```css
.prepend-sprite-icon( <@x>, <@y>, <@width>, <@height>[, <@nudge-left>[, <@nudge-top>[, <@pad>[, <@sprite-image>[, <@sprite-grid>]]]]] );
```

* `@x`: The x coordinate of the desired image on the grid.
* `@y`: The y coordinate of the desired image on the grid.
* `@width`: Width of the image
* `@height`: Height of the image
* `@nudge-left`: *(Optional)* The value of the `left` property for the icon. Defaults to `0`.
* `@nudge-top`: *(Optional)* The value of the `top` property for the icon. Defaults to `0`.
* `@pad`: *(Optional)* Left-padding (in addition to the width of the icon) to apply to the element. Defaults to `10px`
* `@sprite-image`: *(Optional)* The sprite image to use. Defaults to the globally defined `@sprite-image` value.
* `@sprite-grid`: *(Optional)* The grid size used in the sprite. Defaults to the globally defined `@sprite-grid` value.

```css
/* Usage: */
.example {
	.prepend-sprite-icon( 1, 2, 16px, 32px );
}
/* Example output: */
.example {
	position: relative;
	padding-left: 42px;
}
.example:before {
	position: absolute;
	display: block;
	content: ' ';
	background-image: url('/images/example-sprite.png');
	background-repeat: no-repeat;
	background-position: -50px -100px;
	width: 16px;
	height: 32px;
	top: 0;
	left: 0;
}
```

### .append-sprite-icon()

Appends an icon taken from the sprite after the element it's called on.

```css
.append-sprite-icon( <@x>, <@y>, <@width>, <@height>[, <@nudge-right>[, <@nudge-top>[, <@pad>[, <@sprite-image>[, <@sprite-grid>]]]]] );
```

* `@x`: The x coordinate of the desired image on the grid.
* `@y`: The y coordinate of the desired image on the grid.
* `@width`: Width of the image
* `@height`: Height of the image
* `@nudge-right`: *(Optional)* The value of the `right` property for the icon. Defaults to `0`.
* `@nudge-top`: *(Optional)* The value of the `top` property for the icon. Defaults to `0`.
* `@pad`: *(Optional)* Right-padding (in addition to the width of the icon) to apply to the element. Defaults to `10px`
* `@sprite-image`: *(Optional)* The sprite image to use. Defaults to the globally defined `@sprite-image` value.
* `@sprite-grid`: *(Optional)* The grid size used in the sprite. Defaults to the globally defined `@sprite-grid` value.

```css
/* Usage: */
.example {
	.append-sprite-icon( 1, 2, 16px, 32px );
}
/* Example output: */
.example {
	position: relative;
	padding-right: 42px;
}
.example:after {
	position: absolute;
	display: block;
	content: ' ';
	background-image: url('/images/example-sprite.png');
	background-repeat: no-repeat;
	background-position: -50px -100px;
	width: 16px;
	height: 32px;
	top: 0;
	right: 0;
}
```

### .prepend-sprite-icon-pos()

Adjusts the positioning of a prepended sprite icon.

```css
.prepend-sprite-icon-pos( <@x>, <@y>[, <@nudge-left>[, <@nudge-top>[, <@sprite-grid>]]] );
```

* `@x`: The x coordinate of the desired image on the grid.
* `@y`: The y coordinate of the desired image on the grid.
* `@nudge-left`: *(Optional)* The value of the `left` property for the icon. Defaults to `0`.
* `@nudge-top`: *(Optional)* The value of the `top` property for the icon. Defaults to `0`.
* `@sprite-grid`: *(Optional)* The grid size used in the sprite. Defaults to the globally defined `@sprite-grid` value.

```css
/* Usage: */
.example {
	.prepend-sprite-icon-pos( 1, 2 );
}
/* Example output: */
.example:before {
	background-position: -50px -100px;
}
```

### .append-sprite-icon-pos()

Adjusts the positioning of a appended sprite icon.

```css
.append-sprite-icon-pos( <@x>, <@y>[, <@nudge-right>[, <@nudge-top>[, <@sprite-grid>]]] );
```

* `@x`: The x coordinate of the desired image on the grid.
* `@y`: The y coordinate of the desired image on the grid.
* `@nudge-right`: *(Optional)* The value of the `right` property for the icon. Defaults to `0`.
* `@nudge-top`: *(Optional)* The value of the `top` property for the icon. Defaults to `0`.
* `@sprite-grid`: *(Optional)* The grid size used in the sprite. Defaults to the globally defined `@sprite-grid` value.

```css
/* Usage: */
.example {
	.append-sprite-icon-pos( 1, 2 );
}
/* Example output: */
.example:after {
	background-position: -50px -100px;
}
```

### .prepend-icon-setup()

A [partial mixin](#optimising-output-using-partial-mixins) to generate common properties for prepended icon mixins.

```css
.prepend-icon-setup( [<@width>[, <@height>[, <@nudge-left>[, <@nudge-top>[, <@pad>]]]]] );
```

* `@width`: *(Optional)* Width of the image. Defaults to not being set.
* `@height`: *(Optional)* Height of the image. Defaults to not being set.
* `@nudge-left`: *(Optional)* The value of the `left` property for the icon. Defaults to not being set.
* `@nudge-top`: *(Optional)* The value of the `top` property for the icon. Defaults to not being set.
* `@pad`: *(Optional)* Left-padding (in addition to the width of the icon) to apply to the element. Defaults to `10px`

```css
/* Usage: */
.example {
	.prepend-icon-setup( 32px, 15px );
}
/* Example output: */
.example {
	position: relative;
}
.example:before {
	position: absolute;
	display: block;
	content: ' ';
	top: 0;
	left: 0;
    width: 32px;
    height: 15px;
}
```

### .append-icon-setup()

A [partial mixin](#optimising-output-using-partial-mixins) to generate common properties for appended icon mixins.

```css
.append-icon-setup( [<@width>[, <@height>[, <@nudge-right>[, <@nudge-top>[, <@pad>]]]]] );
```

* `@width`: *(Optional)* Width of the image. Defaults to not being set.
* `@height`: *(Optional)* Height of the image. Defaults to not being set.
* `@nudge-right`: *(Optional)* The value of the `right` property for the icon. Defaults to not being set.
* `@nudge-top`: *(Optional)* The value of the `top` property for the icon. Defaults to not being set.
* `@pad`: *(Optional)* Right-padding (in addition to the width of the icon) to apply to the element. Defaults to `10px`

```css
/* Usage: */
.example {
	.append-icon-setup( 32px, 15px );
}
/* Example output: */
.example {
	position: relative;
}
.example:after {
	position: absolute;
	display: block;
	content: ' ';
	top: 0;
	right: 0;
    width: 32px;
    height: 15px;
}
```

### .prepend-icon-image()

A [partial mixin](#optimising-output-using-partial-mixins) to generate image-specific properties for prepended icon mixins. Likely to be used in combination with the `.prepend-icon-setup()` mixin above.

```css
.prepend-icon-image( <@icon-image>[, <@width>[, <@height>[, <@nudge-left>[, <@nudge-top>[, <@pad>]]]]] );
```

* `@icon-image`: URL or data URI of an image to use for the prepended icon
* `@width`: *(Optional)* Width of the image. Defaults to not being set.
* `@height`: *(Optional)* Height of the image. Defaults to not being set.
* `@nudge-left`: *(Optional)* The value of the `left` property for the icon. Defaults to not being set.
* `@nudge-top`: *(Optional)* The value of the `top` property for the icon. Defaults to not being set.
* `@pad`: *(Optional)* Left-padding (in addition to the width of the icon) to apply to the element. Defaults to `10px`

```css
/* Usage: */
.example {
	.prepend-icon-image( 'img/icon.png', 12px, 12px );
}
/* Example output: */
.example {
	padding-left: 22px;
}
.example:before {
  background: url('img/icon.png') no-repeat 0 0;
  width: 12px;
  height: 12px;
}
```

### .append-icon-image()

A [partial mixin](#optimising-output-using-partial-mixins) to generate image-specific properties for appended icon mixins. Likely to be used in combination with the `.append-icon-setup()` mixin above.

```css
.append-icon-image( <@icon-image>[, <@width>[, <@height>[, <@nudge-right>[, <@nudge-top>[, <@pad>]]]]] );
```

* `@icon-image`: URL or data URI of an image to use for the prepended icon
* `@width`: *(Optional)* Width of the image. Defaults to not being set.
* `@height`: *(Optional)* Height of the image. Defaults to not being set.
* `@nudge-right`: *(Optional)* The value of the `right` property for the icon. Defaults to not being set.
* `@nudge-top`: *(Optional)* The value of the `top` property for the icon. Defaults to not being set.
* `@pad`: *(Optional)* Right-padding (in addition to the width of the icon) to apply to the element. Defaults to `10px`

```css
/* Usage: */
.example {
	.append-icon-image( 'img/icon.png', 12px, 12px );
}
/* Example output: */
.example {
	padding-right: 22px;
}
.example:after {
  background: url('img/icon.png') no-repeat 0 0;
  width: 12px;
  height: 12px;
}
```

Arrows
-------

The arrow mixins give the ability to create pure CSS 'arrows' for use in different contexts.

* [.arrow()](#arrow)
* [.arrowbox()](#arrowbox)

### .arrow()

Turns an empty element into an arrow. This works best when applied to pseudo-elements so that you don't need to creating junk elements in the DOM.

```css
.arrow( <@direction>, <@size>, <@bg-color> );
```

* `@direction`: Direction that the arrow should point in. Possible values: `up`, `down`, `left`, `right`.
* `@size`: Size (in pixels) of the arrow.
* `@bg-color`: The color of the arrow.

```css
/* Usage: */
.example:before {
	.arrow(right, 10px, #F00);
}
/* Example output: */
.example:before {
    width: 0;
    height: 0;
    border: 10px solid transparent;
    border-left-color: #F00;
}
```

### .arrowbox()

Adds an arrow pointer to a box, for use as a tooltip (or similar).

```css
.arrowbox( <@direction>, <@size>, <@bg-color>[, <@offset>] );
```

* `@direction`: Direction that the arrow should point in. Possible values: `up`, `down`, `left`, `right`.
* `@size`: Size (in pixels) of the arrow.
* `@bg-color`: The background-color of the arrow and the box.
* `@offset`: *(Optional)* Position (in pixels) for where the arrow should be positioned along the side of the box. Defaults to the center.

```css
.arrowbox( <@direction>, <@size>, <@bg-color>, <@border-width>, <@border-color>[, <@offset>] );
```

* `@direction`: Direction that the arrow should point in. Possible values: `up`, `down`, `left`, `right`.
* `@size`: Size (in pixels) of the arrow.
* `@bg-color`: The background-color of the arrow and the box.
* `@border-width`: The border width for the arrow and the box.
* `@border-color`: The border color for the arrow and the box.
* `@offset`: *(Optional)* Position (in pixels) for where the arrow should be positioned along the side of the box. Defaults to the center.


```css
/* Usage: */
.example {
	.arrowbox(down, 10px, #000);
}
.example2 {
	.arrowbox(right, 10px, #000, 2px, #F00);
}
/* Example output: */
.example {
	width: 100px;
	height: 100px;
	position: relative;
	background: #000;
	margin-bottom: 20px;
}
.example:after,
.example:before {
	top: 100%;
	border: solid transparent;
	content: " ";
	height: 0;
	width: 0;
	position: absolute;
	pointer-events: none;
}
.example:after {
	border-color: transparent;
	border-top-color: #000;
	border-width: 10px;
	left: 50%;
	margin-left: -10px;
}

.example2 {
	width: 100px;
	height: 100px;
	position: relative;
	background: #000;
	border: 2px solid #F00;
	margin-bottom: 20px;
}
.example2:after,
.example2:before {
	left: 100%;
	border: solid transparent;
	content: " ";
	height: 0;
	width: 0;
	position: absolute;
	pointer-events: none;
}
.example2:after {
	border-color: transparent;
	border-left-color: #000;
	border-width: 8px;
	top: 50%;
	margin-top: -8px;
}
.example2:before {
	border-color: transparent;
	border-left-color: #F00;
	border-width: 11px;
	top: 50%;
	margin-top: -11px;
}

```


Grids
-------

The ClearLess grid system is a straightforward, fluid grid system that supports either floated columns or columns created using `display: inline-block;`. It supports infinite levels of nested columns.

Gutters are applied using the margin-right property. The columns that represent the last in a row at any time need to have the `.end-column()` (for floated grids) or `.inline-end-column()` (for inline-block grids) applied to them to stop them dropping down (although there are also shortcuts for doing this in the column and inline-column mixins themselves). There are obviously other ways to address this 'last column gutter' issue - but we've opted for simplicity and to closest match how many people would code this when doing so in 'straight' css.

Groups of columns need to be wrapped in a parent element with the appropriate `.column-wrapper()` or `.inline-column-wrapper()` mixin applied to them.

* [@total-columns](#setting-total-columns)
* [@column-width](#setting-column-width)
* [@gutter-width](#setting-gutter-width)
* [.column-wrapper()](#column-wrapper)
* [.inline-column-wrapper()](#inline-column-wrapper)
* [.column()](#column)
* [.inline-column()](#inline-column)
* [.end-column()](#end-column)
* [.inline-end-column()](#inline-end-column)
* [.span()](#span)
* [.pre-pad()](#pre-pad)
* [.post-pad()](#post-pad)
* [.pre-push()](#pre-push)
* [.post-push()](#post-push)
* [.post-push-end()](#post-push-end)

### SETTING: @total-columns

```css
@total-columns: 12;
```

The total number of columns for the top-level grid.

### SETTING: @column-width

```css
@column-width: 60px;
```

The width of a column, in pixels. It should be noted that the pixel value is never actually used itself - instead it will be converted to a percentage value. If you have flat visuals you can take this value straight from your visual, whatever width they are fixed at.

### SETTING: @gutter-width

```css
@gutter-width: 20px;
```

The width of a gutter, in pixels. It should be noted that the pixel value is never actually used itself - instead it will be converted to a percentage value. If you have flat visuals you can take this value straight from your visual, whatever width they are fixed at.


### .column-wrapper()

Apply to the parent element of the grid columns for **floated grids**. This (by design) *does not* apply any float clearing to the columns - you will likely want to use the `.clearfix()` mixin (or `overflow:hidden;` or whatever you're perferred float clearing methid is!) to account for this.

```css
.column-wrapper();
```

```css
/* Usage: */
.example {
	.column-wrapper();
}
/* Example output: */
.example {
	width: 100%;
}
```

### .inline-column-wrapper()

Apply to the parent element of the grid columns for **inline-block grids**.

```css
.inline-column-wrapper();
```

```css
/* Usage: */
.example {
	.column-wrapper();
}
/* Example output: */
.example {
	letter-spacing: -0.31em;
	word-spacing: -0.43em;
}
.ie7 .example {
	letter-spacing: normal;
}
```

### .column()

When supplied with no arguments, this mixin just sets up the necessary shared styles to make an element into a **floated** column. The `.span()` mixin should then be used to apply the correct width accordingly.

When supplied with a column count, this mixin effectively combines both of the above steps into one - simpler but may not result in the most optimised CSS, depending on the situation.

```css
.column();
```

```css
.column( <@span>[, <@parent-grid-units>[, <@end-column>]] );
```

* `@span`: Number of grid columns to span.
* `@parent-grid-units`: *(Optional)* For nested grids, the number of columns the parent element spans needs to be added here.
* `@end-column`: *(Optional)* Set to `true` if this column is the last one in a row.

```css
.column( <@span>[, <@end-column>] );
```

* `@span`: *(Optional)* Number of grid columns to span.
* `@end-column`: *(Optional)* Set to `true` if this column is the last one in a row.

```css
/* Usage: */
.example {
	.column();
}
.example2 {
	.column(2);
}
.example3 {
	.column(2,true);
}
/* Example output: */
.example {
	float: left;
	margin-right: 1.5873015873015872%;
}
.example2 {
    float: left;
    width: 11.11111111111111%;
    margin-right: 1.5873015873015872%;
}
.example3 {
    float: left;
    width: 11.11111111111111%;
}
```

### .inline-column()

When supplied with no arguments, this mixin just sets up the necessary shared styles to make an element into a **inline-block** column. The `.span()` mixin should then be used to apply widths and margins accordingly.

When supplied with a column count, this mixin effectively combines both of the above steps into one - simpler but may not result in the most optimised CSS, depending on the situation.

```css
.inline-column();
```

```css
.inline-column( <@span>[, <@parent-grid-units>[, <@end-column>]] );
```

* `@span`: Number of grid columns to span.
* `@parent-grid-units`: *(Optional)* For nested grids, the number of columns the parent element spans needs to be added here.
* `@end-column`: *(Optional)* Set to `true` if this column is the last one in a row.

```css
.inline-column( <@span>[, <@end-column>] );
```

* `@span`: *(Optional)* Number of grid columns to span.
* `@end-column`: *(Optional)* Set to `true` if this column is the last one in a row.

```css
/* Usage: */
.example {
	.inline-column();
}
.example2 {
	.inline-column(2);
}
.example3 {
	.inline-column(2,true);
}
/* Example output: */
.example {
	display: inline-block;
	vertical-align: top;
	letter-spacing: normal;
	word-spacing: normal;
	margin-right: 1.5873015873015872%;
}
.ie7 .example {
	display: inline;
	zoom: 1;
}
.example2 {
	display: inline-block;
	vertical-align: top;
	letter-spacing: normal;
	word-spacing: normal;
	width: 11.11111111111111%;
	margin-right: 1.5873015873015872%;
}
.ie7 .example2 {
	display: inline;
	zoom: 1;
}
.example3 {
	display: inline-block;
	vertical-align: top;
	letter-spacing: normal;
	word-spacing: normal;
	width: 11.11111111111111%;
}
.ie7 .example3 {
	display: inline;
	zoom: 1;
}
```

### .end-column()

Should be applied to the last column in a row for **floated columns**. Typically this will equate to the `:last-child` column, but when dropping columns for RWD this is not always the case.

```css
.end-column();
```

```css
/* Usage: */
.example {
	.end-column();
}
/* Example output: */
.example {
	margin-right: 0;
	float: right;
}
```

### .inline-end-column()

Should be applied to the last column in a row for **inline-block columns**. Typically this will equate to the `:last-child` column, but when dropping columns for RWD this is not always the case.

```css
.inline-end-column();
```

```css
/* Usage: */
.example {
	.inline-end-column();
}
/* Example output: */
.example {
	margin-right: 0;
}
```

### .span()

A [partial mixin](#optimising-output-using-partial-mixins) for generating the width (and sometimes margin-right) property for columns (both floated and inline-block).

```css
.span( <@span>[, <@parent-grid-units>] );
```

* `@span`: Number of grid columns to span.
* `@parent-grid-units`: *(Optional)* For nested grids, the number of columns the parent element spans needs to be added here.

```css
/* Usage: */
.example {
	.span(2);
}
/* Example output: */
.example {
	width: 11.11111111111111%;
}
```

### .pre-pad()

Adds the specified number of columns' worth of padding to the the left of the element. 

```css
.pre-pad( <@span>[, <@parent-grid-units>] );
```

* `@span`: Number of grid columns' worth of `padding-left` to add.
* `@parent-grid-units`: *(Optional)* For nested grids, the number of columns the parent element spans needs to be added here.

```css
/* Usage: */
.example {
	.pre-pad(2);
}
/* Example output: */
.example {
	padding-left: 12.698412698412698%;
}
```

### .post-pad()

Adds the specified number of columns' worth of padding to the the right of the element. 

```css
.post-pad( <@span>[, <@parent-grid-units>] );
```

* `@span`: Number of grid columns' worth of `padding-right` to add.
* `@parent-grid-units`: *(Optional)* For nested grids, the number of columns the parent element spans needs to be added here.

```css
/* Usage: */
.example {
	.post-pad(2);
}
/* Example output: */
.example {
	padding-right: 12.698412698412698%;
}
```

### .pre-push()

Adds the specified number of columns' worth of margin to the the left of the element. 

```css
.pre-push( <@span>[, <@parent-grid-units>] );
```

* `@span`: Number of grid columns' worth of `margin-left` to add.
* `@parent-grid-units`: *(Optional)* For nested grids, the number of columns the parent element spans needs to be added here.

```css
/* Usage: */
.example {
	.pre-push(2);
}
/* Example output: */
.example {
	margin-left: 12.698412698412698%;
}
```

### .post-push()

Adds the specified number of columns' worth of margin to the the right of the element. 

```css
.post-push( <@span>[, <@parent-grid-units>] );
```

* `@span`: Number of grid columns' worth of `margin-right` to add.
* `@parent-grid-units`: *(Optional)* For nested grids, the number of columns the parent element spans needs to be added here.

```css
/* Usage: */
.example {
	.post-push(2);
}
/* Example output: */
.example {
	margin-right: 14.285714285714285%;
}
```

### .post-push-end()

Should be used instead of the `.post-push()` mixin above when applied to the last column in a row.

```css
.post-push-end( <@span>[, <@parent-grid-units>] );
```

* `@span`: Number of grid columns' worth of `margin-right` to add.
* `@parent-grid-units`: *(Optional)* For nested grids, the number of columns the parent element spans needs to be added here.

```css
/* Usage: */
.example {
	.post-push-end(2);
}
/* Example output: */
.example {
	margin-right: 12.698412698412698%;
}
```




Some notes on usage and best practices
--------------------------------------

Using a CSS preprocessor can result in pretty bloated generated CSS if you're not careful. Below are a few notes that outline some thoughts on how to avoid this and how ClearLess is structured to give you better tools to optimise your generated CSS.

### On using (Clear)Less responsibly...

Just because there is a mixin for something doesn't mean you *need* to use it! If you have a individual case that would need to override half the the properties outputted by the mixin in order to be styled correctly, then is probably better to roll the solution by hand (or create a new mixin for this use case) rather than to use the mixin and then override it. Fight the bloat!

**All of the examples below above sample output from the mixins.** It's definitely recommended that you familiarise yourself with the output so you can judge whether or not to use the mixin in different situations.

### Optimising output using 'partial' mixins

The library consists mostly of 'full' mixins, which stand alone and give you all the functionality you might expect. However there are also an number of so-called 'partial' mixins for things lke sprites, grids and icons. You may need more than one of these mixins to achieve the desired result - the idea is to split out certain bits of functionality so as to allow for optimisations in your generated CSS.

An example using the [sprite mixins](#sprites):

```css
/* Using 'full' mixins - results in verbose generated CSS */
.social {
	a.twitter {
		.sprite-sized(0, 1, 16px, 16px);
	}
	a.facebook {
		.sprite-sized(0, 2, 16px, 16px);
	}
	a.youtube {
		.sprite-sized(0, 3, 16px, 16px);
	}
}
/* Output - poorly optimised, lots of repitition */
.social a.twitter {
	background-image: url('/images/example-sprite.png');
	background-repeat: no-repeat;
	background-position: 0 -50px;
	width: 16px;
	height: 16px;
}
.social a.facebook {
	background-image: url('/images/example-sprite.png');
	background-repeat: no-repeat;
	background-position: 0 -100px;
	width: 16px;
	height: 16px;
}
.social a.youtube {
	background-image: url('/images/example-sprite.png');
	background-repeat: no-repeat;
	background-position: 0 -150px;
	width: 16px;
	height: 16px;
}

/* Using 'partial' mixins - results in more optimised generated CSS */
.social {
	a {
		.sprite-image();
		.size(16px);
	}
	a.twitter {
		.sprite-pos(0, 1);
	}
	a.facebook {
		.sprite-pos(0, 2);
	}
	a.youtube {
		.sprite-pos(0, 3);
	}
}
/* Output - much better! */
.socal a {
	background-image: url('/images/example-sprite.png');
	background-repeat: no-repeat;
	width: 16px;
	height: 16px;
}
.social a.twitter {
	background-position: 0 -50px;
}
.social a.facebook {
	background-position: 0 -100px;
}
.social a.youtube {
	background-position: 0 -150px;
}
```

As you can see the second example gives an output that is much less verbose and more like what you would write by hand. So use the mixins wisely - for one off styling a full mixin is often appropriate, for applying the same styling to multiple related elements some of the partial mixins are probably better to use.

### Doesn't Gzipping make bloated output a non-issue?

Gzip compression works very well on duplicated strings, so yes - gzipping a file with a lot of duplicated properties will to a large extent offest the additional size/bloat issue.

**However**, It's still best to work to avoid all that duplication in the first place! And never forget that people may be view-sourcing on these files, and trying to learn from them. Regardless of whether you're gzip'ing, you should always strive to make sure your generated CSS is as representative of CSS you'd write by hand as possible/reasonable.

### Mixins or classes?

If you find yourself applying a particular mixin to a lot of element selectors, it's probably worth considering if it would be better to take the 'regular CSS' route and create a separate classname (or classnames) to apply that mixin to. you can then use that class in your HTML instead of repeatedly using the mixin in your Less/CSS. It's generally good to do this in situations where you have an appropriate semantic class name that could encapsulate the mixin's output.

You can of course still use the mixin in other places as needed (where using the class would not be appropriate), but you'll get the advantage of a leaner CSS file (although with the possible disadvantage of more classes in your HTML).

Neither approach is right or wrong - but take the time to consider each particular use case and you'll end up with a better balance between your HTML, your Less and the generated CSS.


Credits
-------

ClearLess is maintained and documented by [Mark Perkins](https://github.com/allmarkedup). For any queries, questions or bug reports you can ping him [on twitter](http://twitter.com/allmarkedup) or open an issue in the Github issue tracker.

Many of the mixins, styles and other parts of this library were shamelessly poached from other open-source projects, including Mark Otto's [Preboot](http://markdotto.com/bootstrap/) and the [HTML5 Boilerplate](http://html5boilerplate.com). Thanks for being awesome!


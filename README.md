# Kubide Sass Architecture & Styleguide

This will be the Sass Architecture used by [Kubide](https://kubide.es/) based on [CSS Guidelines](http://cssguidelin.es/) by [Harry Roberts](https://csswizardry.com/) and [Sass Guidelines](https://sass-guidelin.es/) by [Hugo Giraudel](http://hugogiraudel.com/).

## v0.0.1 [Under Construction]

Copy and pasting from the original repositories to adapt them to Kubide's way or work.

This is a free project that [I maintain](http://ignaciodenuevo.com/) at work.

Now if you feel like contributing or fixing a tiny typo by opening an issue or a pull-request would be appreciated!

> This is an abbreviation of Hugo Giraudel's Sass-Guidelines

# Sass: Syntax & formatting

This styleguide should describe the way we want our code to look at Kubide.

We are right now 4 Front-End developers and our Front-end Lead (so we thought is time to make an architecture to make our work easier).

Same as Sass-Guidelines and CSS-Guidelines we would use the following.

```sass
// Yep
.foo {
  display: block;
  overflow: hidden;
  padding: 0 1em;
}

// Nope
.foo {
    display: block; overflow: hidden;

    padding: 0 1em;
}
```

## Strings

It is highly recommended to force [UTF-8](http://en.wikipedia.org/wiki/UTF-8) encoding in the main stylesheet `@charset` directive. Make sure it is the very first element of the stylesheet and there is no character of any kind before it.

### Quotes

CSS does not require strings to be quoted, not even those containing spaces. (i.e.. `'code'` is equal to `code`).

* color names are treated as colors when unquoted, which can lead to serious issues;
* most syntax highlighters will choke on unquoted strings;
* it helps general readability;
* there is no valid reason not to quote strings.

```sass
// Yep
$direction: 'left';

// Nope
$direction: left;
```
> As per the CSS specifications, the `@charset` directive should be declared in double quotes [to be considered valid](https://www.w3.org/TR/css-syntax-3/#charset-rule). However, Sass takes care of this when compiling to CSS so the authoring has no impact on the final result. You can safely stick to single quotes, even for `@charset`.

### Strings as CSS values

Specific CSS values (identifiers) such as `initial` or `sans-serif` require not to be quoted. Indeed, the declaration `font-family: 'sans-serif'` will silently fail because CSS is expecting an identifier, not a quoted string. Because of this, we do not quote those values.

```sass
// Yep
$font-type: sans-serif;

// Nope
$font-type: 'sans-serif';

// Okay I guess
$font-type: unquote('sans-serif');
```

### Strings containing quotes

If a string contains one or several single quotes, one might consider wrapping the string with double quotes (`"`) instead, in order to avoid escaping characters within the string.

```sass
// Okay
@warn 'You can\'t do that.';

// Okay
@warn "You can't do that.";
```
### URLs

URLs should be quoted as well, for the same reasons as above:

```sass
// Yep
.foo {
  background-image: url('/images/kittens.jpg');
}

// Nope
.foo {
  background-image: url(/images/kittens.jpg);
}
```
## Numbers

In Sass, number is a data type including everything from unitless numbers to lengths, durations, frequencies, angles and so on. This allows calculations to be run on such measures.

### Zeros

```sass
// Yep
.foo {
  padding: 2em;
  opacity: 0.5;
}

// Nope
.foo {
  padding: 2.0em;
  opacity: .5;
}
```
### Units

When dealing with lengths, a `0` value should never ever have a unit.

> Beware, this practice should be limited to lengths only. Having a unitless zero for a time property such as `transition-delay` is not allowed. Theoretically, if a unitless zero is specified for a duration, the declaration is deemed invalid and should be discarded. Not all browsers are that strict, but some are. Long story short: only omit the unit for lengths.

To add a unit to a number, you have to multiply this number by 1 unit.

```sass
$value: 42;

// Yep
$length: $value * 1px;

// Nope
$length: $value + px;
```

To remove the unit of a value, you have to divide it by one unit of its kind.

```sass
$length: 42px;

// Yep
$value: $length / 1px;

// Nope
$value: str-slice($length + unquote(''), 1, 2);
```

### Calculations

**Top-level numeric calculations should always be wrapped in parentheses**

```sass
// Yep
.foo {
  width: (100% / 3);
}

// Nope
.foo {
  width: 100% / 3;
}
```

### Magic numbers

"Magic number" is an [old school programming](http://en.wikipedia.org/wiki/Magic_number_(programming)#Unnamed_numerical_constants) term for *unnamed numerical constant*. Basically, it’s just a random number that happens to *just work*™ yet is not tied to any logical explanation.

```sass
/**
 * 1. Magic number. This value is the lowest I could find to align the top of
 * `.foo` with its parent. Ideally, we should fix it properly.
 */
.foo {
  top: 0.327em; /* 1 */
}
```

On topic, CSS-Tricks has a [terrific article](http://css-tricks.com/magic-numbers-in-css/) about magic numbers in CSS that Hugo encourages you to read.

## Colors

Colors occupy an important place in the CSS language. You can read about how to use colors in Sass here:

* [powerful functions](http://sass-lang.com/documentation/Sass/Script/Functions.html).
* [How to Programmatically Go From One Color to Another](http://thesassway.com/advanced/how-to-programtically-go-from-one-color-to-another-in-sass)
* [Using Sass to Build Color Palettes](http://www.sitepoint.com/using-sass-build-color-palettes/)
* [Dealing with Color Schemes in Sass](http://www.sitepoint.com/dealing-color-schemes-sass/)

### Color formats

In order to make colors as simple as they can be, my advice would be to respect the following order of preference for color formats:

1. [HSL notation](http://en.wikipedia.org/wiki/HSL_and_HSV);
1. [RGB notation](http://en.wikipedia.org/wiki/RGB_color_model);
1. Hexadecimal notation (lowercase and shortened).

CSS color keywords should not be used, unless for rapid prototyping. Indeed, they are English words and some of them do a pretty bad job at describing the color they represent, especially for non-native speakers. On top of that, keywords are not perfectly semantic; for instance `grey` is actually darker than `darkgrey`, and the confusion between `grey` and `gray` can lead to inconsistent usages of this color.

```sass
// Yep
.foo {
  color: hsl(0, 100%, 50%);
}

// Also yep
.foo {
  color: rgb(255, 0, 0);
}

// Meh
.foo {
  color: #f00;
}

// Nope
.foo {
  color: #FF0000;
}

// Nope
.foo {
  color: red;
}
```

When using HSL or RGB notation, always add a single space after a comma (`,`) and no space between parentheses (`(`, `)`) and content.

```sass
// Yep
.foo {
  color: rgba(0, 0, 0, 0.1);
  background: hsl(300, 100%, 100%);
}

// Nope
.foo {
  color: rgba(0,0,0,0.1);
  background: hsl( 300, 100%, 100% );
}
```

### Colors and variables

When using a color more than once, store it in a variable with a meaningful name representing the color.

```sass
$sass-pink: hsl(330, 50%, 60%);
```


### Lightening and darkening colors

To know how it works see the [following link](https://sass-guidelin.es/#lightening-and-darkening-colors).

## Lists

Lists are the Sass equivalent of arrays. A list is a flat data structure (unlike [maps](#maps)) intended to store values of any type (including lists, leading to nested lists).

Lists should respect the following guidelines:

* either inlined or multilines;
* necessarily on multilines if too long to fit on an 80-character line;
* unless used as is for CSS purposes, always comma separated;
* always wrapped in parenthesis;
* trailing comma if multilines, not if inlined.

```sass
// Yep
$font-stack: ('Helvetica', 'Arial', sans-serif);

// Yep
$font-stack: (
  'Helvetica',
  'Arial',
  sans-serif,
);

// Nope
$font-stack: 'Helvetica' 'Arial' sans-serif;

// Nope
$font-stack: 'Helvetica', 'Arial', sans-serif;

// Nope
$font-stack: ('Helvetica', 'Arial', sans-serif,);
```
In [this article](http://hugogiraudel.com/2013/07/15/understanding-sass-lists/), Hugo goes through a lot of tricks and tips to handle and manipulate lists correctly in Sass.


## Maps

With Sass, stylesheet authors can define maps — the Sass term for associative arrays, hashes or even JavaScript objects.

Maps should be written as follows:

* space after the colon (`:`);
* opening brace (`(`) on the same line as the colon (`:`);
* **quoted keys** if they are strings (which represents 99% of the cases);
* each key/value pair on its own new line;
* comma (`,`) at the end of each key/value;
* **trailing comma** (`,`) on last item to make it easier to add, remove or reorder items;
* closing brace (`)`) on its own new line;
* no space or new line between closing brace (`)`) and semi-colon (`;`).

```sass
// Yep
$breakpoints: (
  'small': 767px,
  'medium': 992px,
  'large': 1200px,
);

// Nope
$breakpoints: ( small: 767px, medium: 992px, large: 1200px );
```

Write-ups about Sass maps are many given how longed-for this feature was. Here are 3 that Hugo recommends: [Using Sass Maps](http://www.sitepoint.com/using-sass-maps/), [Extra Map functions in Sass](http://www.sitepoint.com/extra-map-functions-sass/), [Real Sass, Real Maps](http://blog.grayghostvisuals.com/sass/real-sass-real-maps/).

## CSS Ruleset

* related selectors on the same line; unrelated selectors on new lines;
* the opening brace (`{`) spaced from the last selector by a single space;
* each declaration on its own new line;
* a space after the colon (`:`);
* a trailing semi-colon (`;`) at the end of all declarations;
* the closing brace (`}`) on its own new line;
* a new line after the closing brace `}`.

```sass
// Yep
.foo, .foo-bar,
.baz {
  display: block;
  overflow: hidden;
  margin: 0 auto;
}

// Nope
.foo,
.foo-bar, .baz {
    display: block;
    overflow: hidden;
    margin: 0 auto }
```

Adding to those CSS-related guidelines, we want to pay attention to:

* local variables being declared before any declarations, then spaced from declarations by a new line;
* mixin calls with no `@content` coming before any declaration;
* nested selectors always coming after a new line;
* mixin calls with `@content` coming after any nested selector;
* no new line before a closing brace (`}`).

```sass
.foo, .foo-bar,
.baz {
  $length: 42em;

  @include ellipsis;
  @include size($length);
  display: block;
  overflow: hidden;
  margin: 0 auto;

  &:hover {
    color: red;
  }

  @include respond-to('small') {
    overflow: visible;
  }
}
```

## Declaration Sorting

We use to write CSS/Sass alphabetically:

```sass
.foo {
  background: black;
  bottom: 0;
  color: white;
  font-weight: bold;
  font-size: 1.5em;
  height: 100px;
  position: absolute;
  right: 0;
  width: 100px;
}
```

Another way to order selectors is to order them as [Idiomatic CSS](https://github.com/necolas/idiomatic-css#declaration-order) does.

```sass
.selector {
    /* Positioning */
    position: absolute;
    z-index: 10;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;

    /* Display & Box Model */
    display: inline-block;
    overflow: hidden;
    box-sizing: border-box;
    width: 100px;
    height: 100px;
    padding: 10px;
    border: 10px solid #333;
    margin: 10px;

    /* Other */
    background: #000;
    color: #fff;
    font-family: sans-serif;
    font-size: 16px;
    text-align: right;
}
```

## Selector Nesting

We try not to nest more than three selectors deep in Kubide.

```sass
.foo {
  .bar {
    &:hover {
      color: red;
    }
  }
}
```

### Exceptions

For starters, it is allowed and even recommended to nest pseudo-classes and pseudo-elements within the initial selector.

```sass
.foo {
  color: red;

  &:hover {
    color: green;
  }

  &::before {
    content: 'pseudo-element';
  }
}
```

When using component-agnostic state classes such as `.is-active`, it is perfectly fine to nest it under the component’s selector to keep things tidy.

```sass
.foo {
  // …

  &.is-active {
    font-weight: bold;
  }
}
```

# Naming conventions

There are a few things you can name in Sass, and it is important to name them well so the whole code base looks both consistent and easy to read:

* variables;
* functions;
* mixins.


As for many languages, Hugo suggests all-caps snakerized variables when they are constants. Not only is this a very old convention, but it also contrasts well with usual lowercased hyphenated variables.

```sass
// Yep
$CSS_POSITIONS: (top, right, bottom, left, center);

// Nope
$css-positions: (top, right, bottom, left, center);

```

If you really want to play with the ideas of constants in Sass, you should read [this dedicated article](http://www.sitepoint.com/dealing-constants-sass/).

# Commenting

Because of this, it should be heavily commented, especially if you or someone else intend to read and update the code 6 months or 1 year from now.

* the structure and/or role of a file;
* the goal of a ruleset;
* the idea behind a magic number;
* the reason for a CSS declaration;
* the order of CSS declarations;
* the thought process behind a way of doing things.

## Writing comments

```sass
/**
 * Helper class to truncate and add ellipsis to a string too long for it to fit
 * on a single line.
 * 1. Prevent content from wrapping, forcing it on a single line.
 * 2. Add ellipsis at the end of the line.
 */
.ellipsis {
  white-space: nowrap; /* 1 */
  text-overflow: ellipsis; /* 2 */
  overflow: hidden;
}
```

# Architecture

Architecting a CSS project is probably one of the most difficult things you will have to do in a project’s life.

## Components

Components can be anything, as long as they:

* do one thing and one thing only;
* are re-usable and re-used across the project;
* are independent.

For instance, a search form should be treated as a component. It should be reusable, at different positions, on different pages, in various situations. It should not depend on its position in the DOM (footer, sidebar, main content…).

## Component Structure

Ideally, components should exist in their own Sass partial (within the `components/` folder, as is described in the [7-1 pattern](#the-7-1-pattern)), such as `components/_button.scss`. The styles described in each component file should only be concerned with:

* the style of the component itself;
* the style of the component’s variants, modifiers, and/or states;
* the styles of the component’s descendents (i.e. children), if necessary.

## [The 7-1 pattern](https://sass-guidelin.es/#the-7-1-pattern)

```
sass/
|
|– abstracts/
|   |– _variables.scss    # Sass Variables
|   |– _functions.scss    # Sass Functions
|   |– _mixins.scss       # Sass Mixins
|   |– _placeholders.scss # Sass Placeholders
|
|– base/
|   |– _reset.scss        # Reset/normalize
|   |– _typography.scss   # Typography rules
|   …                     # Etc.
|
|– components/
|   |– _buttons.scss      # Buttons
|   |– _carousel.scss     # Carousel
|   |– _cover.scss        # Cover
|   |– _dropdown.scss     # Dropdown
|   …                     # Etc.
|
|– layout/
|   |– _navigation.scss   # Navigation
|   |– _grid.scss         # Grid system
|   |– _header.scss       # Header
|   |– _footer.scss       # Footer
|   |– _sidebar.scss      # Sidebar
|   |– _forms.scss        # Forms
|   …                     # Etc.
|
|– pages/
|   |– _home.scss         # Home specific styles
|   |– _contact.scss      # Contact specific styles
|   …                     # Etc.
|
|– animations/
|   |– _animations.scss     # Animation Layer
|
|– vendors/
|   |– _bootstrap.scss    # Bootstrap
|   |– normalize.scss     # Normalize.css
|   …                     # Etc.
|
– main.scss              # Main Sass file
```


### Base folder

The `base/` folder holds what we might call the boilerplate code for the project.

* `_base.scss`
* `_reset.scss`
* `_typography.scss`

### Layout folder

The `layout/` folder contains everything that takes part in laying out the site or application.


* `_grid.scss`
* `_header.scss`
* `_footer.scss`
* `_sidebar.scss`
* `_forms.scss`
* `_navigation.scss`

### Components folder

For smaller components, there is the `components/` folder.

* `_media.scss`
* `_carousel.scss`
* `_thumbnails.scss`


### Pages folder

If you have page-specific styles, it is better to put them in a `pages/` folder.

* `_home.scss`
* `_contact.scss`

### Animations folder

If you have small or heavy animations you should put them in a `animations/` folder.

* `_animations.scss`

### Abstracts folder

The `abstracts/` folder gathers all Sass tools and helpers used across the project. Every global variable, function, mixin and placeholder should be put in here.

* `_variables.scss`
* `_mixins.scss`
* `_functions.scss`
* `_placeholders.scss`


### Vendors folder

And last but not least, most projects will have a `vendors/` folder containing all the CSS files from external libraries and frameworks – Normalize, Bootstrap, jQueryUI, FancyCarouselSliderjQueryPowered, and so on. Putting those aside in the same folder is a good way to say “Hey, this is not from me, not my code, not my responsibility”.

* `_normalize.scss`
* `_bootstrap.scss`
* `_jquery-ui.scss`

### Main file

The main file (usually labelled `main.scss`) should be the only Sass file from the whole code base not to begin with an underscore. This file should not contain anything but `@import` and comments.

Files should be imported according to the folder they live in, one after the other in the following order:

1. `abstracts/`
1. `vendors/`
1. `base/`
1. `layout/`
1. `components/`
1. `pages/`
1. `themes/`

In order to preserve readability, the main file should respect these guidelines:

* one file per `@import`;
* one `@import` per line;
* no new line between two imports from the same folder;
* a new line after the last import from a folder;
* file extensions and leading underscores omitted.

```sass
@import 'abstracts/variables';
@import 'abstracts/functions';
@import 'abstracts/mixins';
@import 'abstracts/placeholders';

@import 'vendors/bootstrap';
@import 'vendors/jquery-ui';

@import 'base/reset';
@import 'base/typography';

@import 'layout/navigation';
@import 'layout/grid';
@import 'layout/header';
@import 'layout/footer';
@import 'layout/sidebar';
@import 'layout/forms';

@import 'components/buttons';
@import 'components/carousel';
@import 'components/cover';
@import 'components/dropdown';

@import 'pages/home';
@import 'pages/contact';

@import 'animations/animations';
```

## Shame file

There is an interesting concept that has been made popular by [Harry Roberts](http://csswizardry.com), [Dave Rupert](http://daverupert.com) and [Chris Coyier](http://css-tricks.com) that consists of putting all the CSS declarations, hacks and things we are not proud of in a [shame file](http://csswizardry.com/2013/04/shame-css-full-net-interview/).

```sass
/**
 * Nav specificity fix.
 *
 * Someone used an ID in the header code (`#header a {}`) which trumps the
 * nav selectors (`.site-nav a {}`). Use !important to override it until I
 * have time to refactor the header stuff.
 */
.site-nav a {
    color: #BADA55 !important;
}
```

# Responsive Web Design and breakpoints

## Naming breakpoints

Breakpoints should not be named after devices but something more general.

```sass
// Yep
$breakpoints: (
  'medium': (min-width: 800px),
  'large': (min-width: 1000px),
  'huge': (min-width: 1200px),
);

// Nope
$breakpoints: (
  'tablet': (min-width: 800px),
  'computer': (min-width: 1000px),
  'tv': (min-width: 1200px),
);
```

## Breakpoint manager

Once you have named your breakpoints the way you want, you need a way to use them in actual media queries.

Obviously, this is a fairly simplistic breakpoint manager.

* [Sass-MQ](https://github.com/sass-mq/sass-mq)
* [Breakpoint](http://breakpoint-sass.com/)
* [include-media](https://github.com/eduardoboucas/include-media)
* [SitePoint](http://www.sitepoint.com/managing-responsive-breakpoints-sass/)
* [CSS-Tricks](http://css-tricks.com/approaches-media-queries-sass/)

## Media Queries Usage

```sass
.foo {
  color: red;

  @include respond-to('medium') {
    color: blue;
  }
}
```

Leading to the following CSS output:

```sass
.foo {
  color: red;
}

@media (min-width: 800px) {
  .foo {
    color: blue;
  }
}
```

# Variables

A new variable should be created only when all of the following criteria are met:

* the value is repeated at least twice;
* the value is likely to be updated at least once;
* all occurrences of the value are tied to the variable (i.e. not by coincidence).

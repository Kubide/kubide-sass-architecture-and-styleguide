# Kubide Sass Architecture

This will be the Sass Architecture used by Kubide based on [CSS Guidelin](http://cssguidelin.es/) by [Harry Roberts](https://csswizardry.com/) and [Sass Guidelin](https://sass-guidelin.es/) by [Hugo Giraudel](http://hugogiraudel.com/).

## v0.0.1 [Under Construction]

Copy and pasting from the original repositories to adapt them to Kubide's way or work.

This is a free project that [I maintain](http://ignaciodenuevo.com/) at work.

Now if you feel like contributing or fixing a tiny typo by opening an issue or a pull-request would be appreciated!

> This is an abbreviation of Hugo Giraudel's Sass-Guideline

# Sass: Syntax & formatting

This styleguide should describe the way we want our code to look at Kubide.

We are right now 4 Front-End developers and our Front-end Lead (so we thought is time to make an architecture to make our work easier).

Same as Sass-Guideline and CSS-Guideline we would use the following.

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

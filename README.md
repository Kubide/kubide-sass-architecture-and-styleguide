# Kubide Sass Architecture

This will be the Sass Architecture used by Kubide based on [CSS Guidelin](http://cssguidelin.es/) by [Harry Roberts](https://csswizardry.com/) and [Sass Guidelin](https://sass-guidelin.es/) by [Hugo Giraudel](http://hugogiraudel.com/).

## v0.0.1 [Under Construction]

Copy and pasting from the original repositories to adapt them to Kubide's way or work.

This is a free project that [I maintain](http://ignaciodenuevo.com/) at work.

Now if you feel like contributing or fixing a tiny typo by opening an issue or a pull-request would be appreciated!

# Syntax & formatting

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


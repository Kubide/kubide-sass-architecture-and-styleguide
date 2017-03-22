# CSS

> This is an abbreviation of [Harry Robert's CSS-Guidelines](http://cssguidelin.es/)

## Introduction

* keep stylesheets maintainable.
* keep code transparent, sane, and readable.
* keep stylesheets scalable.

## The Importance of a Styleguide

* build and maintain products for a reasonable length of time.
* have developers of differing abilities and specialisms.
* have a number of different developers working on a product at any given time.
* on-board new staff regularly.
* have a number of codebases that developers dip in and out of.

## Syntax and Formatting

At a very high-level, we want

* two (2) space indents, no tabs.
* 80 character wide columns.
* multi-line CSS.
* meaningful use of whitespace.

## 80 Characters Wide

Where possible, limit CSS files’ width to 80 characters.

* the ability to have multiple files open side by side.
* viewing CSS on sites like GitHub, or in terminal windows.
* providing a comfortable line length for comments.

## Titling

Begin every new major section of a CSS project with a title:

```css
/*------------------------------------*\
*  #SECTION-TITLE
\*------------------------------------*/

.selector { }
```

## Anatomy of a Ruleset

* related selectors on the same line; unrelated selectors on new lines.
* a space before our opening brace ({).
* properties and values on the same line.
* a space after our property–value delimiting colon (:).
* each declaration on its own new line.
* the opening brace ({) on the same line as our last selector.
* our first declaration on a new line after our opening brace ({).
* our closing brace (}) on its own new line.
* each declaration indented by two (2) spaces.
* a trailing semi-colon (;) on our last declaration.

## Multi-line CSS

CSS should be written across multiple lines, except in very specific circumstances. There are a number of benefits to this:

* A reduced chance of merge conflicts, because each piece of functionality exists on its own line.
* More ‘truthful’ and reliable diffs, because one line only ever carries one change.

Exceptions to this rule should be fairly apparent, such as similar rulesets that only carry one declaration each, for example:

```css
icon {
  display: inline-block;
  width:  16px;
  height: 16px;
  background-image: url(/img/sprite.svg);
}

.icon--home     { background-position:   0     0  ; }
.icon--person   { background-position: -16px   0  ; }
.icon--files    { background-position:   0   -16px; }
.icon--settings { background-position: -16px -16px; }
```

These types of ruleset benefit from being single-lined because:

* they still conform to the one-reason-to-change-per-line rule.
* they share enough similarities that they don’t need to be read as thoroughly as other rulesets—there is more benefit in being able to scan their selectors, which are of more interest to us in these cases.

## Indenting

As well as indenting individual declarations, indent entire related rulesets to signal their relation to one another, for example:


```css
.foo { }

  .foo__bar { }

    .foo__baz { }

```

## Indenting Sass

```css
.foo {
  color: red;

  .bar {
    color: blue;
  }
}
```

When indenting Sass, we stick to the same two (2) spaces, and we also leave a blank line before and after the nested ruleset.

## Alignment

Attempt to align common and related identical strings in declarations, for example:

```css
.foo {
  -webkit-border-radius: 3px;
     -moz-border-radius: 3px;
          border-radius: 3px;
}

.bar {
  position: absolute;
  top:    0;
  right:  0;
  bottom: 0;
  left:   0;
  margin-right: -10px;
  margin-left:  -10px;
  padding-right: 10px;
  padding-left:  10px;
}
```

## Meaningful Whitespace

* One (1) empty line between closely related rulesets.
* Two (2) empty lines between loosely related rulesets.
* Five (5) empty lines between entirely new sections.

```css
/*------------------------------------*\
  #FOO
\*------------------------------------*/

.foo { }

  .foo__bar { }


.foo--baz { }





/*------------------------------------*\
  #BAR
\*------------------------------------*/

.bar { }

  .bar__baz { }

  .bar__foo { }
```

## HTML

Always quote attributes, even if they would work without.

This is ok:

```html
<div class="box">
```

This is better:

```html
<div class="box">
```

When writing multiple values in a class attribute, separate them with two spaces, thus:

```html
<div class="foo  bar">
```

You can denote thematic breaks in content with five (2) empty lines, for example:

```html
<header class="page-head">
  ...
</header>


<main class="page-content">
  ...
</main>


<footer class="page-foot">
  ...
</footer>
```

Separate independent but loosely related snippets of markup with a single empty line, for example:

```html
<ul class="primary-nav">

  <li class="primary-nav__item">
    <a href="/" class="primary-nav__link">Home</a>
  </li>

  <li class="primary-nav__item  primary-nav__trigger">
    <a href="/about" class="primary-nav__link">About</a>

    <ul class="primary-nav__sub-nav">
      <li><a href="/about/products">Products</a></li>
      <li><a href="/about/company">Company</a></li>
    </ul>

  </li>

  <li class="primary-nav__item">
    <a href="/contact" class="primary-nav__link">Contact</a>
  </li>

</ul>
```

## Commenting

As CSS is something of a declarative language that doesn’t really leave much of a paper-trail, it is often hard to discern—from looking at the CSS alone—

* whether some CSS relies on other code elsewhere.
* what effect changing some code will have elsewhere.
* where else some CSS might be used.
* what styles something might inherit (intentionally or otherwise).
* what styles something might pass on (intentionally or otherwise).
* where the author intended a piece of CSS to be used.

As a rule, you should comment anything that isn’t immediately obvious from the code alone. That is to say, there is no need to tell someone that `color: red;` will make something red, but if you’re using `overflow: hidden;` to clear floats—as opposed to clipping an element’s overflow—this is probably something worth documenting.

For large comments that document entire sections or components, we use a DocBlock-esque multi-line comment which adheres to our 80 column width.

```css
/**
 * The site’s main page-head can have two different states:
 *
 * 1) Regular page-head with no backgrounds or extra treatments; it just
 *    contains the logo and nav.
 * 2) A masthead that has a fluid-height (becoming fixed after a certain point)
 *    which has a large background image, and some supporting text.
 *
 * The regular page-head is incredibly simple, but the masthead version has some
 * slightly intermingled dependency with the wrapper that lives inside it.
 */
 ```
 
This level of detail should be the norm for all non-trivial code—descriptions of states.

```css
/**
 * Extend `.btn {}` in _components.buttons.scss.
 */

.btn { }
```

### Low-level

Oftentimes we want to comment on specific declarations (i.e. lines) in a ruleset.

```css
/**
 * Large site headers act more like mastheads. They have a faux-fluid-height
 * which is controlled by the wrapping element inside it.
 *
 * 1. Mastheads will typically have dark backgrounds, so we need to make sure
 *    the contrast is okay. This value is subject to change as the background
 *    image changes.
 * 2. We need to delegate a lot of the masthead’s layout to its wrapper element
 *    rather than the masthead itself: it is to this wrapper that most things
 *    are positioned.
 * 3. The wrapper needs positioning context for us to lay our nav and masthead
 *    text in.
 * 4. Faux-fluid-height technique: simply create the illusion of fluid height by
 *    creating space via a percentage padding, and then position everything over
 *    the top of that. This percentage gives us a 16:9 ratio.
 * 5. When the viewport is at 758px wide, our 16:9 ratio means that the masthead
 *    is currently rendered at 480px high. Let’s…
 * 6. …seamlessly snip off the fluid feature at this height, and…
 * 7. …fix the height at 480px. This means that we should see no jumps in height
 *    as the masthead moves from fluid to fixed. This actual value takes into
 *    account the padding and the top border on the header itself.
 */

.page-head--masthead {
  margin-bottom: 0;
  background: url(/img/css/masthead.jpg) center center #2e2620;
  @include vendor(background-size, cover);
  color: $color-masthead; /* [1] */
  border-top-color: $color-masthead;
  border-bottom-width: 0;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1) inset;

  @include media-query(lap-and-up) {
    background-image: url(/img/css/masthead-medium.jpg);
  }

  @include media-query(desk) {
    background-image: url(/img/css/masthead-large.jpg);
  }

  > .wrapper { /* [2] */
    position: relative; /* [3] */
    padding-top: 56.25%; /* [4] */

    @media screen and (min-width: 758px) { /* [5] */
      padding-top: 0; /* [6] */
      height: $header-max-height - double($spacing-unit) - $header-border-width; /* [7] */
    }

  }

}
```


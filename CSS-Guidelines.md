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

## Naming Conventions

A good naming convention will tell you and your team

* what type of thing a class does.
* where a class can be used.
* what (else) a class might be related to.

The naming convention Harry follows is very simple: hyphen (-) delimited strings, with BEM-like naming for more complex pieces of code.

### Hyphen Delimited

All strings in classes are delimited with a hyphen (-), like so:

```css
.page-head { }

.sub-content { }
```

### BEM (Block - Element - Modifier)

BEM splits components’ classes into three groups:

[See BEM official documentation](https://en.bem.info/)

* Block: The sole root of the component.
* Element: A component part of the Block.
* Modifier: A variant or extension of the Block.

To take an analogy (note, not an example):

```css
.person { }
.person__head { }
.person--tall { }
```

Elements are delimited with two (2) underscores `(__)`, and Modifiers are delimited by two (2) hyphens `(--)`.

#### Modifying Elements

You can have variants of Elements, and these can be denoted in a number of ways depending on how and why they are being modified.

```css
.person__eye--blue { }
```

If using Sass, we would likely write this like so:

```sass
.person { }

  .person__face {

    .person--handsome & { }

  }

.person--handsome { }
```

Note that we do not nest a new instance of `.person__face {}` inside of `.person--handsome {};` instead, we make use of Sass’ parent selectors to prepend `.person--handsome` onto the existing `.person__face {}` selector. This means that all of our `.person__face {}`-related rules exist in once place, and aren’t spread throughout the file. This is general good practice when dealing with nested code: keep all of your context (e.g. all `.person__face {}` code) encapsulated in one location.

[See naming conventions in HTML](http://cssguidelin.es/#naming-conventions-in-html) for more info.

## JavaScript Hooks

As a rule, it is unwise to bind your CSS and your JS onto the same class in your HTML. This is because doing so means you can’t have (or remove) one without (removing) the other. It is much cleaner, much more transparent, and much more maintainable to bind your JS onto specific classes.

Typically, these are classes that are prepended with `js-`, for example:

```html
<input type="submit" class="btn  js-btn" value="Follow" />
```

## `data-*` Attributes

A common practice is to use `data-*` attributes as JS hooks, but this is incorrect. `data-*` attributes, as per the spec, are used to store custom data private to the page or application (emphasis mine). `data-*` attributes are designed to store data, not be bound to.

# CSS Selectors

Selectors and their maintainability and scalability is one of the most critical things about writing CSS.

## Selector Intent

Do not style HTML tags directly to, for example style a navbar as follows:

```css
header ul { }
```

This selector’s intent is to style any ul inside any header element, whereas our intent was to style the site’s main navigation. 

Instead create a class:

```css
.site-nav { }
```

CSS cannot be encapsulated, it is inherently leaky, but we can mitigate some of these effects by not writing such globally-operating selectors: **your selectors should be as explicit and well reasoned as your reason for wanting to select something.**

## Reusability

We want the option to be able to move, recycle, duplicate, and syndicate components across our projects.

To this end, we make heavy use of classes. IDs, as well as being hugely over-specific. Everything you choose, from the type of selector to its name, should lend itself toward being reused.

## Location Independence

Our components’ styling should not be reliant upon where we place them—they should remain entirely location independent.

```css
.promo a { }
```

It will greedily style any and every link inside of a .promo to look like a button—it is also pretty wasteful as a result of being so locationally dependent: we can’t reuse that button with its correct styling outside of .promo because it is explicitly tied to that location. A far better selector would have been:

```css
.btn { }
```

## Portability

Reducing, or, ideally, removing, location dependence means that we can move components around our markup more freely, but how about improving our ability to move classes around components?

```css
input.btn { }
```

This is a qualified selector; the leading `input` ties this ruleset to only being able to work on `input` elements. By omitting this qualification, we allow ourselves to reuse the `.btn` class on any element we choose, like an `a`, for example, or a `button`.

Of course, there are times when you may want to legitimately qualify a selector—you might need to apply some very specific styling to a particular element when it carries a certain class, for example:

```css
/**
 * Embolden and colour any element with a class of `.error`.
 */
.error {
  color: red;
  font-weight: bold;
}

/**
 * If the element is a `div`, also give it some box-like styling.
 */
div.error {
  padding: 10px;
  border: 1px solid;
}
```
This is one example where a qualified selector might be justifiable, but I would still recommend an approach more like:

```css
/**
 * Text-level errors.
 */
.error-text {
  color: red;
  font-weight: bold;
}

/**
 * Elements that contain errors.
 */
.error-box {
  padding: 10px;
  border: 1px solid;
}
```

## Naming

Instead of a class like `.site-nav`, choose something like `.primary-nav`; rather than `.footer-links`, favour a class like `.sub-links`.

That is to say, we should use sensible names—classes like `.border` or `.red` are never advisable—but we should avoid using classes which describe the exact nature of the content and/or its use cases. **Using a class name to describe content is redundant because content describes itself.**


```css
/**
 * Runs the risk of becoming out of date; not very maintainable.
 */
.blue {
  color: blue;
}

/**
 * Depends on location in order to be rendered properly.
 */
.header span {
  color: blue;
}

/**
 * Too specific; limits our ability to reuse.
 */
.header-color {
  color: blue;
}

/**
 * Nicely abstracted, very portable, doesn’t risk becoming out of date.
 */
.highlight-color {
  color: blue;
}

```

Instead of .home-page-panel, choose .masthead; instead of `.site-nav`, favour `.primary-nav`; instead of `.btn-login`, opt for `.btn-primary`.

[See naming UI components](http://cssguidelin.es/#naming-ui-components)

## Selector Performance

Generally speaking, the longer a selector is (i.e. the more component parts) the slower it is, for example:

```css
body.home div.header ul { }

```

…is a far less efficient selector than:

```css
.primary-nav { }
```

This is because browsers read CSS selectors **right-to-left**. A browser will read the first selector as:

* find all `ul` elements in the DOM.
* now check if they live anywhere inside an element with a class of `.header`.
* next check that `.header` class exists on a `div` element.
* now check that that all lives anywhere inside any elements with a class of `.home`.
* finally, check that `.home` exists on a `body` element.

The second, in contrast, is simply a case of the browser reading

* find all the elements with a class of .primary-nav.

## The Key Selector

Because browsers read selectors **right-to-left**, the rightmost selector is often critical in defining a selector’s performance: this is called the key selector `* {}`.

This is a very, very expensive selector, and should most likely be avoided or rewritten.

## General Rules

Your selectors are fundamental to writing good CSS. To very briefly sum up the above sections:

* **Select what you want explicitly**, rather than relying on circumstance or coincidence. Good Selector Intent will rein in the reach and leak of your styles.
* **Write selectors for reusability**, so that you can work more efficiently and reduce waste and repetition.
* **Do not nest selectors unnecessarily**, because this will increase specificity and affect where else you can use your styles.
* **Do not qualify selectors unnecessarily**, as this will impact the number of different elements you can apply styles to.
* **Keep selectors as short as possible**, in order to keep specificity down and performance up.

# Specificity

As we’ve seen, CSS isn’t the most friendly of languages: globally operating, very leaky, dependent on location, hard to encapsulate, based on inheritance… But! None of that even comes close to the horrors of specificity.

```css
#content table { }

/**
 * Uh oh! My styles get overwritten by `#content table {}`.
 */
.my-new-table { }

```

Specificity can, among other things,

* limit your ability to extend and manipulate a codebase.
* interrupt and undo CSS’ cascading, inheriting nature.
* cause avoidable verbosity in your project.
* prevent things from working as expected when moved into different environments.
* lead to serious developer frustration.

Simple changes to the way we work include, but are not limited to,

* not using IDs in your CSS.
* not nesting selectors.
* not qualifying classes.
* not chaining selectors.

## IDs in CSS

If we want to keep specificity low, which we do, we have one really quick-win, simple, easy-to-follow rule that we can employ to help us: avoid using IDs in CSS.

In fact, to highlight the severity of this difference, see how [one thousand chained classes cannot override the specificity of a single ID](http://jsfiddle.net/csswizardry/0yb7rque/).

## Nesting

When we talk about nesting, we don’t necessarily mean preprocessor nesting, like so:

```css
.foo {

  .bar { }

}
```

We’re actually talking about descendant or child selectors.

```css
/**
 * An element with a class of `.bar` anywhere inside an element with a class of
 * `.foo`.
 */
.foo .bar { }


/**
 * An element with a class of `.module-title` directly inside an element with a
 * class of `.module`.
 */
.module > .module-title { }


/**
 * Any `li` element anywhere inside a `ul` element anywhere inside a `nav`
 * element
 */
nav ul li { }
```

As a rule, **if a selector will work without it being nested then do not nest it.**

## Scope

One possible advantage of nesting—which, unfortunately, does not outweigh the disadvantages of increased specificity—is that it provides us with a namespace of sorts. A selector like `.widget .title` scopes the styling of `.title` to an element that only exists inside of an element carrying a class of `.widget`.

### !important

!important is not a good practice by default, but there are exceptions like helpers or utility classes in CSS.

```css
.one-half {
  width: 50% !important;
}

.hidden {
  display: none !important;
}
```

Here we proactively apply `!important` to ensure that these styles always win. This is correct use of `!important` to guarantee that these trumps always work, and don’t accidentally get overridden by something else more specific.

**Only use `!important` proactively, not reactively.**

## Hacking Specificity

[See hacking specificity](http://cssguidelin.es/#hacking-specificity)

# Architectural Principles

In this section, we’ll take a look at some of these design patterns and paradigms, and how we can use them to reduce code—and increase code reuse—in our CSS projects.

## High-level Overview

At a very high-level, your architecture should help you

* provide a consistent and sane environment.
* accommodate change.
* grow and scale your codebase.
* promote reuse and efficiency.
* increase productivity.

## Object-orientation

Object-orientation is a programming paradigm that breaks larger programs up into smaller, in(ter)dependent objects that all have their own roles and responsibilities. From Wikipedia:

> Object-oriented programming (OOP) is a programming paradigm that represents the concept of ‘objects’ […] which are usually instances of classes, [and] are used to interact with one another to design applications and computer programs.

Skin is a layer that we (optionally) add to our structure in order to give objects and abstractions a specific look-and-feel. Let’s look at an example:

```css
/**
 * A simple, design-free button object. Extend this object with a `.btn--*` skin
 * class.
 */
.btn {
  display: inline-block;
  padding: 1em 2em;
  vertical-align: middle;
}


/**
 * Positive buttons’ skin. Extends `.btn`.
 */
.btn--positive {
  background-color: green;
  color: white;
}

/**
 * Negative buttons’ skin. Extends `.btn`.
 */
.btn--negative {
  background-color: red;
  color: white;
}
```

Whenever you are building a UI component, try and see if you can break it into two parts: one for structural styles (paddings, layout, etc.) and another for skin (colours, typefaces, etc.)

## The Single Responsibility Principle

The single responsibility principle is a paradigm that, very loosely, states that all pieces of code (in our case, classes) should focus on doing one thing and one thing only. More formally:

> …the single responsibility principle states that every context (class, function, variable, etc.) should have a single responsibility, and that responsibility should be entirely encapsulated by the context.

```css
.box {
  display: block;
  padding: 10px;
}


.message {
  border-style: solid;
  border-width: 1px 0;
  font-weight: bold;
}

.message--error {
  background-color: #fee;
  color: #f00;
}

.message--success {
  background-color: #efe;
  color: #0f0;
}
```

 Let’s take some example CSS that does not adhere to the single responsibility principle:

```css
.error-message {
  display: block;
  padding: 10px;
  border-top: 1px solid #f00;
  border-bottom: 1px solid #f00;
  background-color: #fee;
  color: #f00;
  font-weight: bold;
}

.success-message {
  display: block;
  padding: 10px;
  border-top: 1px solid #0f0;
  border-bottom: 1px solid #0f0;
  background-color: #efe;
  color: #0f0;
  font-weight: bold;
}
```

## The Open/Closed Principle

The open/closed principle, in my opinion, is rather poorly named. It is poorly named because 50% of the vital information is omitted from its title. The open/closed principle states that

> [s]oftware entities (classes, modules, functions, etc.) should be open for extension, but closed for modification.

Let’s take an example:

```css
.box {
  display: block;
  padding: 10px;
}

.box--large {
  padding: 20px;
}
```

An incorrect way of achieving the same might look like this:

```css
.box {
  display: block;
  padding: 10px;
}

.content .box {
  padding: 20px;
}
```

A selector like .content .box {} is potentially troublesome because

* it forces all `.box` components into that style when placed inside of `.content`, which means the modification is dictated to developers, whereas developers should be allowed to opt into changes explicitly;
* the `.box` style is now unpredictable to developers; the single responsibility no longer exists because nesting the selector produces a forced caveat.

Remember: **open for extension; closed for modification.**

## DRY

DRY, which stands for Don’t Repeat Repeat Yourself, is a micro-principle used in software development which aims to keep the repetition of key information to a minimum. Its formal definition is that

> [e]very piece of knowledge must have a single, unambiguous, authoritative representation within a system.

```css
.btn {
  display: inline-block;
  padding: 1em 2em;
  font-weight: bold;
}

[...]

.page-title {
  font-size: 3rem;
  line-height: 1.4;
  font-weight: bold;
}

[...]

  .user-profile__title {
    font-size: 1.2rem;
    line-height: 1.5;
    font-weight: bold;
  }
```

From the above code, we can reasonably deduce that the `font-weight: bold`; declaration appears three times purely coincidentally. To try and create an abstraction, mixin, or `@extend` directive to cater for this repetition would be overkill, and would tie these three rulesets together based purely on circumstance.

In short, only DRY code that is actually, thematically related. Do not try to reduce purely coincidental repetition: **duplication is better than the wrong abstraction.**

## Composition over Inheritance

This principle suggests that larger systems should be composed from much smaller, individual parts, rather than inheriting behaviour from a much larger, monolithic object.

## The Separation of Concerns

The separation of concerns is a principle which, at first, sounds a lot like the single responsibility principle. The separation of concerns states that code should be broken up

> into distinct sections, such that each section addresses a separate concern. A concern is a set of information that affects the code of a computer program. […] A program that embodies SoC well is called a modular program.

The separation of concerns increases reusability and confidence whilst reducing dependency.

### Misconceptions

There are, I feel, a number of unfortunate misconceptions surrounding the separation of concerns when applied to HTML and CSS. They all seem to revolve around some format of:

> Using classes for CSS in your markup breaks the separation of concerns.

In short: having classes in your markup does not violate the separation of concerns. Classes merely act as an API to link two separate concerns together. The simplest way to separate concerns is to write well formed HTML and well formed CSS, and link to two together with sensible, judicious use of classes.

# Tâmia Stylus Style Guide

Based on [edX Stylus & CSS Style Guide](https://github.com/edx/ux-pattern-library/wiki/Styleguide:-Stylus-&-CSS).

### Stylus (in CSS syntax)

The majority of our CSS is processed from Stylus, our chosen pre-processor.

We prefer [CSS syntax](https://learnboost.github.io/stylus/docs/css-style.html) over the syntax with significant whitespace when writing pre-processed CSS.

## Syntax & Formatting

In general, styling should adhere to:

* tab (set to 4 spaces) indents, no spaces;
* ideally, 120-characters wide lines;
* meaningful use of whitespace (breaking up sections / different definitions, legibility of comments, etc.);
* one selector at line;
* a space before any opening brace (`{`);
* no new line before a closing brace (``}``);
* properties and values on the same line;
* no space between a property and a value (`color:#f00`);
* limit use of shorthand declarations to instances where you must explicitly set all the available values.
* detailed comments to help readers;

```scss
// Good
.foo,
.bar,
.baz {
    display:block;
    overflow:hidden;
    padding-top:0;
    padding-right:1rem;
}

// Bad
.foo, .bar, .baz {
    display:block;
}

// Bad
.foo 
{ display: block;
    overflow: hidden;
    padding: 0 1em inherit inherit; }

// Bad
.foo {
    display: block; overflow: hidden;

    padding: 0 1rem;
}

// Bad
.foo {
    display: block; overflow: hidden; padding: 0 1rem;
}
```

### Stylus specifically

Adding to those CSS-related guidelines, we want to pay attention to:

* local variables being declared before any declarations, then spaced from declarations by a new line; prepend name with `_`;
* `@extend`s and mixin calls with parenthesis (`name()`) should be before any other declarations;
* nested selectors always coming after a new line;
* treat mixin calls with content (`+name() {}`) as normal nested selectors;

```scss
.foo, 
.foo-bar,
.baz {
    _length = 50rem;

    @extend .ellipsis;
    size(_length);
    display:block;
    overflow:hidden;
    margin:0 auto;

    +link-all-states() {
        color:black;
    }
    
    &:hover, 
    &:focus {
        color:red;
    }

    &.is-disabled {
        opacity:.5;
    }
}
```

### Strings in Stylus

Strings should always be wrapped with double quotes in Stylus. Besides consistency with other languages, including CSS' cousin JavaScript, there are several reasons for this choice:

* color names are treated as colors when unquoted, which can lead to serious issues;
* most syntax highlighters will choke on unquoted strings;
* it helps general readability (within editors and syntax-sensitive;

```scss
// Good
simple_font = "Open Sans", "Helvetica", "Arial", sans-serif;

// Bad
simple_font = 'Open Sans', 'Helvetica', 'Arial', sans-serif;

// Bad
simple_font = Open Sans, Helvetica, Arial, sans-serif;
```

URLs should be quoted as well, for the same reasons as above:

```scss
// Good
.foo {
    background-image:url("/images/logo-edx.jpg");
}

// Bad
.foo {
    background-image:url(/images/logo-edx.jpg);
}
```

### Numbers

Numbers should not display trailing zeros and leading zeros before a decimal value less than one.

```scss
// Good
.foo {
    opacity:.5;
    padding:2rem;
}

// Bad
.foo {
    opacity:0.5;
    padding:2.0rem;
}
```

When dealing with lengths, a 0 value should never ever have a unit.

```scss
// Good
_length = 0;

// Bad
_length = 0rem;
```

### Calculations

Top-level numeric calculations should always be wrapped in parentheses. Not only does this requirement dramatically improve readability, it also prevents some edge cases by forcing Stylus to evaluate the contents of the parentheses when creating complex values or using variables.

```scss
// Good
.foo {
    width:(100% / 3);
}

// Bad
.foo {
    width:100% / 3;
}
```

### Imperfect and Magic Numbers

“Magic number” is an old school programming term for unnamed numerical constant. Basically, it’s just a random number that happens to just work™ yet is not tied to any logical explanation.

Needless to say magic numbers are a plague and should be avoided at all costs. When you cannot manage to find a reasonable explanation for why a number works, add an extensive comment explaining how you got there and why you think it works. Admitting you don’t know why something works is still more helpful to the next developer than them having to figure out what’s going on from scratch.

```scss
// Magic Number: This value is the lowest I could find to align the top of `.foo` with its parent. Ideally, we should fix it properly.
.foo {
    top:.327rem;
}
```

### Units of Measurement

We prefer using pixels in general for sizing. But sometimes percents or ems/rems are better.

Use `spacer` (which is 10px by default) variable if possible:

```scss
side-padding:2*spacer;
```

When setting line-heights of our type, we define numbers that are unitless.

### Colors

In order to make colors as simple as they can be, my advice would be to respect the following order of preference for color formats:

1. Hexadecimal notation. Lowercase and shortened when possible.
2. HSLA notation;

When using HSLA notation, always add a single space after a comma (,) and no space between parentheses ((, )) and content.

When needing to communicate transparency or alpha within a color, please use the `hsla()` formatting or the `rgba()` Stylus function.

```scss
// Good
.foo {
    color:#f00;
    background:hsla(360, 100%, 50%, .5);
}

// Good
.foo {
    color:rgba(#f00, .5);
}

// Bad
.foo {
    color:rgba(0,0,0,0.1);
    background:hsl( 300, 100%, 100% );
}

// Bad
.foo {
    color:#BAD;
    background:#ff0000;
}
```

#### Colors and Variables

When using a color more than once, store it in a variable with a meaningful name representing the color (with the proper level of theming application applied):

```scss
// Good
accent_color = #c0ffee;

// Bad
stylus_pink = blue;
light_blue = hsla(58%, 73%, 76%, 1.0);
```

### Property and Declaration Order

Stylus/CSS properties should be ordered like this:

1. Positioning: position, top, right, bottom, left, display, float, clear, z-index, overflow, etc.
2. Layout: width, height, margin, padding, border, etc.
3. Decoration: color, background, opacity, etc.
4. Typography: font, line-height, list-style, outline, text-align, etc.
5. Everything else.

Note again that Stylus extends mixins (only with parenthesis) along with variables should be defined before these properties.

Example: 

```scss
.foo {
    _size = 100px;
    @extend .bar;
    foo-bar();
    position:absolute;
    overflow:hidden;
    right:0;
    bottom:0;
    width:_size;
    height:_size;
    color:base_color;
    background:bg_color;
    font-weight:bold;
    font-size:19px;
}
```

### Selector/Rule Nesting

One particular feature Stylus provides that is being overly misused by many developers is selector nesting. Selector nesting offers a way for stylesheet authors to compute long selectors by nesting shorter selectors within each others.

For instance, the following Stylus nesting:
```scss
// Bad
.foo {
    .foo__bar {
        &:hover {
            color:red;
        }
    }
}
```

…will generate this CSS:

```css
.foo .foo__bar:hover {
    color:#f00;
}
```

The problem with selector nesting is that it ultimately makes code more difficult to read. One has to mentally compute the resulting selector out of the indentation levels; it is not always quite obvious what the CSS will end up being.

This statement becomes truer as selectors get longer and references to the current selector (&) more frequent. At some point, the risk of losing track and not being able to understand what's going on anymore is so high that it is not worth it.

Also, extra selectors within a rule can cause havok with a compiled stylesheets cascade and specificity causing either 1) later rules to be trumped by previous ones, and thus incorrectly rendered styles or 2) redundant, bloated later rules to correct for specificity.

To prevent such a situation, **we avoid selector nesting as much as possible**. When having to nest, **one should never nest more than three levels**. However, there are obviously a few exceptions to this rule:

* it is allowed and even recommended to nest pseudo-classes and pseudo-elements within the initial selector;
* when using component-agnostic state classes such as `.is-active`, it is perfectly fine to nest it under the component's selector to keep things tidy;
* styling an element because it happens to be contained within another specific element.

You should never repeat a block name:

```scss
// Good
.block {
    &__elem {
        color:red;
    }
    &_mod &__elem {
        color:blue;
    }
}

// Bad
.block {
    &__elem {
        color:red;
        .block_mod & {
            color:blue;
        }
    }
}
```

### Inline + In-Document Styles

We strive to maintain proper separation of content and design, and therefore **highly discourage the use of inline `style="…"` attributes as well as `<style></style>` elements** that contain rules on a per document or page level.

### Inline Documentation

Styling can be tricky and to some of our earlier principles, we strive to be clear in explaining why we did something so that the next person or the person four years later can follow why or how we were doing something. This means commenting on things like:

* the structure and/or role of a file;
* the goal of a ruleset;
* the idea behind a magic number;
* the reason for a CSS declaration;
* the thought process behind a way of doing things.

#### Stylus Comments (for development)

We start every mixin, function and extend with a block of documentation explaining:

* what the following piece of code does;
* what arguments it accepts (if any);
* an example for how to use it;
* We write them all in single-line Stylus comments so that they do not get compiled into the generated CSS, allowing us to have documentation without worrying about bloating our code with unnecessary comments.

Example comment block from a mixin partial:

```scss
// background-image with Retina variant.
//
// path - Image file path.
// x - background-position x (default: 0).
// y - background-position y (default: 0).
//
// Retina image file should be named path@2x.png.
image(path, x=0, y=0) {
    if retinafy {
        background:url(path) x y;

        +retina() {
            _ext = extname(path);
            _hdpath = dirname(path) + "/" + basename(path, _ext) + "@2x" + ext;
            background:url(_hdpath) x y;
            background-size:image-size(pathjoin(images_relative_root, path));
        }
    } else {
        background:url(path) x y;
    }
}
```

When commenting on a particular section or line of Stylus/CSS code, use Stylus inline comments instead of a block. This makes the comment invisible in the output, even in expanded mode during development.

```scss
// Visited link color (only for underline links because image backgrounds don’t work on :visited links).
visited_color ?= #a963b8
```

## File Organization

We leverage the following directory structure:

```
[application-name]
├── styles
|   └── blocks                       // blocks
|       └── block1.styl
|       └── block2.styl
|       ...
|   └── index.styl                   // global settings, entry point
|   └── styles.styl                  // common and misc styles
```

### File Naming

Files should be named in all lowercase with `-` separating legible words (e.g. `main-rtl.styl`)

### Vendor Files

When possible, we do not:

* edit a vendor file directly or change a vendor’s source code;
* change a vendor file’s name.

When possible store vendor files in Bower or npm.

### Main Styles Files

The main file (usually labelled `index.styl`) should not contain anything but global variables, `@import`s and comments:

```scss
/* Author: Artem Sapegin, http://sapegin.me, 2013 */

content_max_width = 1024px;
sticky_footer_height = 24px;
header_height = 55px;
spacer = 10px;
bg_color = #fff;
base_color = #1a1a1a;
light_color = #ccc;
light_hover_color = #fff;
link_style = "underline";
link_color = #1978c8;
visited_color = #8c5ad2;
hover_color = #1694ec;
photo_bg_color = #1a1a1a;
site_domain = "birdwatcher.ru";
content_font = 19px
normal_font = 16px
small_font = 13px

@import "../bower_components/social-likes/social-likes_flat.css";
@import "../build/fotorama.css";
@import "../build/fonts/icons";
@import "tamia";
@import "modules/rouble";
@import "modules/switcher";
@import "modules/modal";
@import "modules/form";
@import "modules/table";
@import "modules/media";
@import "modules/list";
@import "modules/richtypo";
@import "styles";

@import "blocks/*";
```

## UI Naming Conventions
Naming conventions in CSS are hugely useful in making your code more strict, more transparent, and more informative.

A good naming convention tells us:

* what type of thing a class does;
* where a class can be used;
* what (else) a class might be related to.

### Naming Syntax

**TODO: link to OPOR**

### Naming Elements

Name an element something that is sensible, but somewhat ambiguous: aim for high reusability. For example, instead of a class like ``.site-nav``, choose something like ``.primary-nav``; rather than ``.footer-links``, favour a class like ``.sub-links``.

The first name of each of the two examples above is tied to a very specific use case: they can only be used as the site’s navigation or the footer’s links respectively. By using slightly more ambiguous names (such as those in the second of each two examples), we can increase our ability to reuse these components in different circumstances.

```scss
// Good
// Nicely abstracted, very portable, doesn’t risk becoming out of date.
.highlight-color {
    color:orange;
}

// Bad
// Runs the risk of becoming out of date; not very maintainable.
.blue {
    color:blue;
}

// Bad
// Too specific; limits our ability to reuse.
.header-color {
    color:blue;
}
```

### Layout

When we create layouts for our applications, we:

* always follow the existing grid system and configuration of the application;
* define layout separate from other style rules;
* adhere to existing responsive breakpoints (as well as strategically update these) for larger layout/grid system needs/issues.

### Responsive Breakpoints

Media queries should not be tied to specific devices. Media queries should take care of a range of screen sizes, until the design breaks and the next media query takes over. For the same reasons, breakpoints should not be named after devices but something more general.

At this point, any naming convention that makes crystal clear that a design is not intimately tied to a specific device type will do the trick, as long as it gives a sense of the range covered.

```scss
// Good
breakpoint_small = 600px;
breakpoint_medium = 1024px;
breakpoint_large = 1280px;

// Bad
breakpoint_iphone = 480px;
breakpoint_ipad = 1024px;
breakpoint_desktop = 1280px;
```

#### Referencing Breakpoints

Use [Tâmia](http://tamiadev.github.io/tamia/docs.html#toc15) mixins:

```scss
.about-splash {
    &__img {
        ...
    }

    +below(breakpoint_medium) {
        &__img {
            ...
        }
    }
    +below(breakpoint_small) {
        &__img {
            ...
        }
    }
}
```

### Naming Stylus Elements

There are a few things you can name in Stylus, and it is important to name them well so the whole code base looks both consistent and easy to read.

Variables: **lowercase underscore-delimited, and above all meaningful**.

Mixins: **lowercase hyphen-delimited, and above all meaningful**.

```scss
content_max_width = 1024px;

sprite-size(img)
    width: sprite-width(img)
    height: sprite-height(img)
```

### The Single Responsibility Principle

The single responsibility principle is a paradigm that, very loosely, states that all pieces of code (in our case, classes) should focus on doing one thing and one thing only.

What this means for us is that our CSS should be composed of a series of much smaller classes that focus on providing very specific and limited functionality.

```scss
// Good
.message {
    display:block;
    padding:spacer;
    border-style:solid;
    border-width:1px 0;
    font-weight:bold;

    &_error {
        background-color:#fee;
        color:#f00;
    }

    &_success {
        background-color:#efe;
        color:#0f0;
    }
}

// Bad
.error-message {
    display:block;
    padding:spacer;
    border-top:1px solid #f00;
    border-bottom:1px solid #f00;
    background-color:#fee;
    color:#f00;
    font-weight:bold;
}

.success-message {
    display:block;
    padding:spacer;
    border-top:1px solid #0f0;
    border-bottom:1px solid #0f0;
    background-color:#efe;
    color:#0f0;
    font-weight:bold;
}
```

## Style/Display Hacks

### Using !important

The !important modifier (example: `margin: { 0 !important }`) does have a place in CSS projects, but only if used sparingly and proactively.

Proactive use of `!important` is when it is used before you’ve encountered any specificity problems; when it is used as a guarantee rather than as a fix. For example:

```css
.half {
    width:50% !important;
}

.is-hidden {
    display:none !important;
}
```

These two helper, or utility, classes are very specific in their intentions: we would only use them if we wanted something to be rendered at 50% width or not rendered at all. If we didn’t want this behaviour, we would not use these classes, therefore whenever we do use them we will definitely want them to win.

Here we proactively apply `!important` to ensure that these styles always win. This is correct use of `!important` to guarantee that these trumps always work, and don’t accidentally get overridden by something else more specific.

Incorrect, reactive use of `!important` is when it is used to combat specificity problems after the fact: applying `!important` to declarations because of poorly architected CSS.

### Feature Detection

Use Modernizr classes to provide fallbacks for older browsers when necessary:

```scss
.checkbox {
    &__input
        &:before {
            background-image:embedurl("check.svg");
            ...
        }
        .no-svg &:before {
            content: "✓"
            ...
        }
    }
}
```

## Git Etiquette

Never check production-ready files into a codebase’s repository including:

* Stylus sourcemaps;
* compiled CSS files;
* local editor/project management files.

All of these files should be added to either your local global .gitignore file or to project's .gitignore file.

Compiled CSS should be compressed as much as possible. 

### Sources

The following were great sources of inspiration, guidance, and debate:

* Original [edX Stylus & CSS Style Guide](https://github.com/edx/ux-pattern-library/wiki/Styleguide:-Stylus-&-CSS)
* http://cssguidelin.es/
* http://sass-guidelin.es/
* http://codeguide.co/
* https://github.com/styleguide/css

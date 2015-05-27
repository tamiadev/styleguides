## General

Welcome to the edX CSS/Sass Styleguide. Before continuing, you should have a general understanding of specificity, the cascading principle of Cascading Style Sheets, the [SCSS](http://sass-lang.com/) syntax, and knowledge of Sass/SCSS compilation.

At the end of the day, edX's styles should be kept as simple and understandable as it can be. CSS is a simple language. Sass, our pre-processor of choice and being intended to write CSS, should not get much more complex than regular CSS. The [KISS principle](http://en.wikipedia.org/wiki/KISS_principle) (Keep It Simple Stupid) is key here and may even take precedence over the [DRY](http://en.wikipedia.org/wiki/Don%27t_repeat_yourself) principle (Don't Repeat Yourself) in some circumstances.

### Sources
The following were great sources of inspiration, guidance, and debate:

* http://cssguidelin.es/
* http://sass-guidelin.es/
* http://codeguide.co/
* https://github.com/styleguide/css

- - -

## Background

For more context and related details, please see the following companion documentation:

* [General Front End Styleguide](https://github.com/edx/ux-pattern-library/wiki/Styleguide:-General)
* [HTML Styleguide](https://github.com/edx/ux-pattern-library/wiki/Styleguide:-HTML)
* [Accessibility Styleguide](https://github.com/edx/ux-pattern-library/wiki/Styleguide:-Accessibility)
* [edX Right to Left Best Practices](https://github.com/edx/edx-platform/wiki/RTL-UI-Best-Practices)

### Sass (in SCSS syntax)
The majority of our CSS is processed from Sass, our chosen pre-processor.

We prefer [SCSS syntax over the original Sass syntax](http://thesassway.com/editorial/sass-vs-scss-which-syntax-is-better) when writing pre-processed CSS.Thus, all files should have a ``.scss`` extension.

When possible and supported, we prefer [LibSass](https://github.com/sass/libsass) over RubySass for the sake of edX standards/platform language support and for the speed/efficiency with local development workflows (Node.js-based).

- - -

## Syntax & Formatting
In general, edX styling should adhere to:

* four (4) spaces indents, no tabs;
* ideally, 100-characters wide lines;
* properly written multi-line CSS rules (one rule per line);
* meaningful use of whitespace (breaking up sections/ different definitions, legibility of comments, etc.);
* multiple selectors display related selectors on the same line; unrelated selectors on new lines;
* a space before any opening brace (``{``);
* properties and values on the same line;
* limit use of shorthand declarations to instances where you must explicitly set all the available values.
* detailed comments to help readers;


```scss
// Example: Yep
.foo {
    display: block;
    overflow: hidden;
    padding-top: 0;
    padding-right: 1rem;
}

// Example: Nope
.foo 
{ display: block;
    overflow: hidden;
    padding: 0 1em inherit inherit; }

// Example: Nope
.foo {
    display: block; overflow: hidden;

    padding: 0 1rem;
}

// Example: Nope
.foo {
    display: block; overflow: hidden; padding: 0 1rem;
}
```

### Sass specifically
Adding to those CSS-related guidelines, we want to pay attention to:

* local variables being declared before any declarations, then spaced from declarations by a new line;
* mixin calls with no ``@content`` coming before any declaration;
* nested selectors always coming after a new line;
* mixin calls with ``@content`` coming after any nested selector;
* no new line before a closing brace (``}``);

Example:

```scss
.foo, 
.foo-bar,
.baz {
    $length: 50rem;

    @include ellipsis;
    @include size($length);
    display: block;
    overflow: hidden;
    margin: 0 auto;
    
    // STATE: hover and focus
    &:hover, 
    &:focus {
        color: red;
    }

    &.is-disabled {
        opacity: 0.5;
    }

    @include media($mobile) {
        @include span-columns(3);
    }
}
```

### Strings in Sass
Strings should always be wrapped with double quotes in Sass (single being easier to type than double on qwerty keyboards). Besides consistency with other languages, including CSS' cousin JavaScript, there are several reasons for this choice:

* color names are treated as colors when unquoted, which can lead to serious issues;
* most syntax highlighters will choke on unquoted strings;
* it helps general readability (within editors and syntax-sensitive;

```scss
// Example: Yep
$font-stack: "Open Sans", "Helvetica", "Arial", sans-serif;

// Example: Nope
$font-stack: 'Open Sans', 'Helvetica', 'Arial', sans-serif;

// Example: Nope
$font-stack: Open Sans, Helvetica, Arial, sans-serif;
```

URLs should be quoted as well, for the same reasons as above:

```scss
// Example: Yep
.foo {
    background-image: url("/images/logo-edx.jpg");
}

// Example: Nope
.foo {
    background-image: url(/images/logo-edx.jpg);
}
```

### Numbers
Numbers should display leading zeros before a decimal value less than one. Never display trailing zeros.

```scss
// Example: Yep
.foo {
    padding: 2rem;
    opacity: 0.5;
}

// Example: Nope
.foo {
    padding: 2.0rem;
    opacity: .5;
}
```

When dealing with lengths, a 0 value should never ever have a unit.

```scss
// Example: Yep
$length: 0;

// Example: Nope
$length: 0rem;
```

### Calculations
Top-level numeric calculations should always be wrapped in parentheses. Not only does this requirement dramatically improve readability, it also prevents some edge cases by forcing Sass to evaluate the contents of the parentheses when creating complex values or using variables.

```scss
// Example: Yep
.foo {
    width: (100% / 3);
}

// Example: Nope
.foo {
    width: 100% / 3;
}
```

### Imperfect and Magic Numbers
"Magic number" is an old school programming term for unnamed numerical constant. Basically, it's just a random number that happens to just work™ yet is not tied to any logical explanation.

Needless to say magic numbers are a plague and should be avoided at all costs. When you cannot manage to find a reasonable explanation for why a number works, add an extensive comment explaining how you got there and why you think it works. Admitting you don't know why something works is still more helpful to the next developer than them having to figure out what's going on from scratch.

```scss
// Magic Number: This value is the lowest I could find to align the top of `.foo` with its parent. Ideally, we should fix it properly.
.foo {
    top: 0.327rem;
}
```

### Units of Measurement
We prefer using relative units (rems or percentage-based) in general for sizing. Thinking in pixel-based dimensions is easier to plan out, so we leverage some utilities to convert pixel-based values/scales to a relative scale:

* [Bourbon's Pixel to Rems Mixin](http://bourbon.io/docs/#px-to-rem)

#### Fonts
When sizing type, we prefer to use rems as our unit of measurement. Rems allow us to scale type size while still scoping the base of the scale to a relative point in our UI (read more about [rems](http://snook.ca/archives/html_and_css/font-size-with-rem) in [type-sizing](http://css-tricks.com/rems-ems/)). 

When we use rems, we always set a base scale (usually 62.5% on the body DOM element, which converts a base 16px browser default size to ~10px for easier font-sizing properties with a scale based on 10).

When we define the ``font-weight`` property with **numeric values**. Also, these values should match up with the font-weights formally supported by the typefaces being used (e.g Open Sans supports weights of 300, 400, 600, 800).

To make things easier, Bourbon has a nifty function that automatically does this for you:
```css
$something: rem(16); // 16 here is the size in pixels
```

#### Capitalization
If you need to capitalize text, say in buttons or in navigation, do so using CSS rather than in the markup. For example:
```html
<!-- Example: Yep -->
<span class="capitalize">I'm in all caps!</span>
```

```css
.capitalize {
    text-transform: uppercase;
}
```

```html
<!-- Example: Nope -->
<span>FOR MORE INFORMATION, SEE OUR FAQS!</span>
```

#### Line Heights
When setting line-heights of our type, we define numbers that are unitless. This, [borrowed from Eric Meyer](http://meyerweb.com/eric/thoughts/2006/02/08/unitless-line-heights/), allows the unitless value to be used by the current element and by descendent elements as a scaling factor.

```scss
// Example: Yep
.foo {
    font-size: 1.4rem;
    line-height: 1.48;
}

// Example: Nope
.foo {
    font-size: 1.4rem;
    line-height: 1.48rem;
}

// Example: Nope
.foo {
    font-size: 1.2rem;
    line-height: 1em;
}

// Example: Nope
.foo {
    font-size: 2.4rem;
    line-height: 32px;
}
```

#### Layout/Grid-based
With grids that control our layouts, we prefer percentage-based measurements (alongside pixel or feature-based media-queries for responsiveness).

Some utilities we leverage include:

* [Grid Calculator](http://gridcalculator.dk/) - for grid definition
* [Neat](http://neat.bourbon.io/) - for grid implementation/management


### Colors
In order to make colors as simple as they can be, my advice would be to respect the following order of preference for color formats:

1. [RGB notation](http://en.wikipedia.org/wiki/RGB_color_model);
2. Hexadecimal notation. Preferably lowercase and shortened when possible.
3. [CSS color keywords](http://www.w3.org/TR/css3-color/#svg-color);

When using RGB notation, always add a single space after a comma (,) and no space between parentheses ((, )) and content.

When needing to communicate transparency or alpha within a color, please use the ``rgba()`` formatting or the ``rgba()`` Sass mixin/function.

```scss
// Example: Yep
.foo {
    color: rgba(0, 0, 0, 0.75);
    background: rgb(255, 255, 255);
}

// Example: Nope
.foo {
    color: rgba(0,0,0,0.1);
    background: hsl( 300, 100%, 100% );
}
```

#### Colors and Variables
When using a color more than once, store it in a variable with a meaningful name representing the color (with the proper level of theming application applied):

```scss
// Example: Yep
$action-primary-color: rgb(0, 180, 255);

// Example: Nope
$sass-pink: blue;
$light-blue: hsla(58%, 73%, 76%, 1.0);
```

When using Sass to calculate derivative color values, also include the actual value in an inline comment next to the calculation syntax.

```scss

$action-color: rgb(25, 25, 25);

...

$action-primary-color: $action-color;
$action-secondary-color: tint($action-color,10%);    // rgb(50, 50, 50)
$action-tertiary-color: shade($action-color, 10%);   // rgb(0, 0, 0)
```

For common or global color values, please either provide a description of the color's hue/value in either A) the Sass variable name where appropriate or B) in an inline comment when the color is defined.

```scss
// Example: Yep
$edx-blue: rgb(0, 162, 227);

// Example: Yep
$branding-color-secondary: rgb(183, 37, 103); // edX Pink
```

### Alignment
Attempt to align common and related identical strings in declarations, such as variable values, for example:

```scss
// example timing variables
$tmg-s3:  3.0s;
$tmg-s2:  2.0s;
$tmg-s1:  1.0s;
$tmg-avg: 0.75s;
$tmg-f1:  0.50s;
$tmg-f2:  0.25s;
$tmg-f3:  0.125s;
```

**Tip: If using Sublime Text 2/3, the [Abacus Package](https://packagecontrol.io/packages/Abacus), makes short work of this.**

### Property and Declaration Order
Sass/CSS properties should be ordered in [Concentric CSS](https://github.com/brandon-rhodes/Concentric-CSS) order, where properties are defined based on moving from the outside to the inside of an element's box model.

Note again that Sass includes and extends along with variables should be defined before these properties

Example: 

```scss
.foo {
    $foo-color: rgb(178, 6, 16);

    @include margin(0 20rem);
    @include clearfix();
    
    @extend %bar;

    position: absolute;
    right: 0;
    bottom: 0;
    width: 10rem;
    height: 10rem;
    background: black;
    overflow: hidden;
    color: $foo-color;
    font-weight: 400;
    font-size: 1.5rem;
}
```

**Tip: If using Sublime Text 2/3, the [CSScomb](https://packagecontrol.io/packages/CSScomb%20Alpha%20Sort), makes short work of this.**

### Selector/Rule Nesting
One particular feature Sass provides that is being overly misused by many developers is selector nesting. Selector nesting offers a way for stylesheet authors to compute long selectors by nesting shorter selectors within each others.

For instance, the following Sass nesting:
```scss
.foo {
    .bar {
        &:hover {
            color: red;
        }
    }
}
```
... will generate this CSS:

```css
.foo {
    &.bar {
      color: red;
    }
}
```

The problem with selector nesting is that it ultimately makes code more difficult to read. One has to mentally compute the resulting selector out of the indentation levels; it is not always quite obvious what the CSS will end up being.

This statement becomes truer as selectors get longer and references to the current selector (&) more frequent. At some point, the risk of losing track and not being able to understand what's going on anymore is so high that it is not worth it.

Also, extra selectors within a rule can cause havok with a compiled stylesheets cascade and specificity causing either 1) later rules to be trumped by previous ones, and thus incorrectly rendered styles or 2) redundant, bloated later rules to correct for specificity.

To prevent such a situation, **we avoid selector nesting as much as possible**. When having to nest, **one should never nest more than three levels**. However, there are obviously a few exceptions to this rule:

* it is allowed and even recommended to nest pseudo-classes and pseudo-elements within the initial selector;
*  when using component-agnostic state classes such as ``.is-active``, it is perfectly fine to nest it under the component's selector to keep things tidy.
* styling an element because it happens to be contained within another specific element


### Inline + In-Document Styles
We strive to maintain proper separation of content and design, and therefore **highly discourage the use of inline style=”…” attributes as well as ```<style></style>``` elements** that contain rules on a per document or page level.

### Inline Documentation
Styling can be tricky and to some of our earlier principles, we strive to be clear in explaining why we did something so that the next person or the person 4 years later can follow why or how we were doing something. This means commenting on things like:

* the structure and/or role of a file;
* the goal of a ruleset;
* the idea behind a magic number;
* the reason for a CSS declaration;
* the order of CSS declarations;
* the thought process behind a way of doing things.

#### Sass Comments (for development)
We start every mixin, function and extend with a block of documentation explaining:

* what the following piece of code does
* what arguments it accepts (if any)
* an example for how to use it
* We write them all in single-line Sass comments so that they do not get compiled into the generated CSS, allowing us to have documentation without worrying about bloating our code with unnecessary comments.

Example comment block from a mixin partial: 
```scss
//
// Prints out the width and height of an element.
//
// $width - String or number representation of the element width
// $height - String or number representation of the element height
// false if it is the same as the width
//
// Examples
// @include size(1rem);
// @include size(1rem, 3rem);
//

@mixin size($width, $height: false) {
    width: $width;
    height: if($height, $height, $width);
}
```

When commenting on a particular section or line of Sass/CSS code, use Sass inline comments instead of a block. This makes the comment invisible in the output, even in expanded mode during development.

```scss
// Add current module to the list of imported modules.
// `!global` flag is required so it actually updates the global variable.
$imported-modules: append($imported-modules, $module) !global;
```

Sass comments that point out a particular context, such as a use-case or state of an element should be prefixed with ``CASE:`` or ``STATE:`` respectively.

Example:
```scss
.foo {
    color: blue;
  
    // STATE: hover and focus
    &:hover, 
    &:focus {
      color: teal;
    }
  
    // STATE: is disabled
    &.is-disabled {
      @extend %disabled;
    }
  
    // CASE: has associated actions
    &.has-actions {
      display: block;
    }
}
```

#### CSS Comments (for production)
While generally not needed or desired, there may be a case where you want comments to render in production-ready CSS. For that, please use an inline comment before the described Sass/CSS properties affected.

Example comment block from a mixin partial: 
```css
.foo {
    /* <gollum>We makes it's position fixed, my precious</gollum> */ 
    position: fixed;
    ...
}
```

#### Table of Contents
A table of contents of major sections of a Sass file should be included and updated. The only exceptions to this rule are:

* Sass Compilation files (e.g. main.scss, app.scss)
* Vendor Sass/CSS files (which should retain their original syntax for maintenance/updating purposes)

A table of contents for each Sass file/partial should contain:

* App Name + Sass file/partial name;
* General summary of the scope of the file;
* Sections of rules/styling (with an anchor for easy finding in the file)
* Summary of sections with brief descriptions

Example Table of Contents:
```scss

// ------------------------------
// edX LMS: Actions

// About: Sass partial for styling all of the LMS's actions (buttons, links, form actions, etc.). Further customization per view or UI element (e.g. header, primary nav) should be done in those specific files/partials.

// #SETTINGS
// Global:                    scoped variables and config.

// #TOOLS
// Mixins:                    Useful mixins.
// Functions:                 Useful functions.
// Animations/Transitions:    Canned motion/stateful changing.

// #BASE
// Reset:                     Basic reset of button/action elements.
// Visual Styles:             Presentation styles/types for actions.

// #COMPONENTS
// Primary:                   Primary actions.
// Secondary:                 Secondary actions.
// Tertiary:                  Less important actions.
// Inline:                    Inline actions.
// Action Groups:             groups of actions together.
// Action + Multiple Select:  dropdown actions.

// #CONTEXTS
// Messages:                  Actions in messages and alerts.
// States:                    Disabled, loading, processing, etc.
// Buttons:                   Button elements.
// ------------------------------

// ------------------------------
// #SETTINGS
// ------------------------------
// sections content goes here...

// ------------------------------
// #TOOLS
// ------------------------------
// sections content goes here...

// ------------------------------
// #BASE
// ------------------------------
// sections content goes here...

// ------------------------------
// #COMPONENTS
// ------------------------------
// sections content goes here...

// ------------------------------
// #CONTEXTS
// ------------------------------
// sections content goes here...

```

Begin every new major section of a CSS project with a title:

#### Global Sass Elements Documentation
Every variable, function, mixin and placeholder that is intended to be reused all over the codebase should be documented as part of the global API using [SassDoc](http://sassdoc.com/).

Exmaple: 
```scss
/// Vertical rhythm baseline used all over the code base.
/// @type Length
$vertical-rhythm-baseline: 1.5rem;
```

- - -

## File Organization
We adhere to the [7-1 Pattern](http://sass-guidelin.es/#the-7-1-pattern) when organizing our applications' Sass files. This means, we leverage the following directory structure:

```
[application-name]
├── static
|   └── vendor                       // vendor packages
|   └── vendor-extensions            // customizations to vendor packages
|   └── css
|   |   └── main.css                 // compiled css
|   └── sass
|   |   └── base                     // set up, and reset
|   |   └── components               // atomic, elemental, and molecular-level UI
|   |   └── layouts                  // grid, canned layouts, layout-based rules
|   |   └── utilities                // variables, helpers, shame + developer-based overrides
|   |   └── views                    // page/view specifics
|   |   └── main.scss                // main application compile
...
└── themes                           // canned/formal themes for application 
|   └── [theme-name]                 // individual theme
|   |   └── static
|   |   |   └── sass
|   |   |   |   └── base.scss        // base overrides/additions
|   |   |   |   └── components.scss  // components overrides/additions
|   |   |   |   └── layouts.scss     // layouts overrides/additions
|   |   |   |   └── views.scss       // views overrides/additions
|   |   |   |   └── overrides.scss   // ultimate overrides/additions
|   |   |   |   └── utilities.scss   // utilities overrides/additions
|   |   |   ...
```

### UX Pattern Library Files + Application Files
When using the edX UX Pattern Library alongside an application the Pattern Library files should be:

1. compiled before any overrides or extensions (on a grouping/directory level)
2. centrally managed per instance or application

Here's an example:

```
[edx-pl]
├── static
|   └── vendor                       // vendor packages
|   └── vendor-extensions            // customizations to vendor packages
|   └── css
|   |   └── main.css                 // compiled css
|   └── sass
|   |   └── base                     // set up, and reset
|   |   └── components               // atomic, elemental, and molecular-level UI
|   |   └── layouts                  // grid, canned layouts, layout-based rules
|   |   └── utilities                // variables, helpers, shame + developer-based overrides
|   |   └── views                    // page/view specifics
[application-name]
├── static
|   └── vendor                       // vendor packages
|   └── vendor-extensions            // customizations to vendor packages
|   └── css
|   |   └── main.css                 // compiled css
|   └── sass
|   |   └── base                     // set up, and reset
|   |   └── components               // atomic, elemental, and molecular-level UI
|   |   └── layouts                  // grid, canned layouts, layout-based rules
|   |   └── utilities                // variables, helpers, shame + developer-based overrides
|   |   └── views                    // page/view specifics
|   |   └── main.scss                // main application compile
...
└── themes                           // canned/formal themes for application 
|   └── [theme-name]                 // individual theme
|   |   └── static
|   |   |   └── sass
|   |   |   |   └── base.scss        // base overrides/additions
|   |   |   |   └── components.scss  // components overrides/additions
|   |   |   |   └── layouts.scss     // layouts overrides/additions
|   |   |   |   └── views.scss       // views overrides/additions
|   |   |   |   └── overrides.scss   // ultimate overrides/additions
|   |   |   |   └── utilities.scss   // utilities overrides/additions
|   |   |   ...
```

See the [main style compile section]() for an example of how the edX UX Pattern Library is referenced during a Sass compilation.

### File Naming
We adhere to the following conventions when naming:

* files should be named in all lowercase with ``-`` separating legible words (e.g. ``main-rtl.scss``)
* Sass partial files (ones to never be compiled on their own) are prefixed with an underscore (e.g. ``_modal.scss``) for native Sass compilation parsing and for a visual cue that the file is not standalone.
* Main Sass compilation files
* Vendor files that need to be given the ``.scss`` file format (for compilation purposes), should preserve their original file names for easier maintenance and updating

### Vendor Files
When using vendor files within our CSS/Sass, we follow [our general vendor rules](https://github.com/edx/ux-pattern-library/wiki/Styleguide:-General#vendor-files).

##### Known/Chosen Vendor Files
We use the following Front End Development centric utilities and vendors to help with our Sass/CSS:

* [Bourbon](http://bourbon.io/) ([docs](http://bourbon.io/docs/)) - A set of utilities for writing SCSS
* [Neat](http://neat.bourbon.io/) ([docs](http://thoughtbot.github.io/neat-docs/latest/)) - A responsive-friendly grid framework based upon Bourbon.
* [normalize](http://necolas.github.io/normalize.css/) - A common and basic CSS reset that makes cross-browser/agent rendering more consistent.

#### Vendor CSS + Sass Compilation
While some vendor CSS files are best left out to a separate asset/reference call when loading a page (through another ``<link>`` element), any vendor Sass/CSS files that our Sass compiled application CSS will benefit from should be included in the compilation. A good example of this is [normalize](http://necolas.github.io/normalize.css/), where both future rules in our styling (both vendor-extensions and everything else) may be dependending on that vendor code being parsed by the browser and being placed in the cascade already. When this is the case, we **do not edit the file, but do edit the file's extension and name to become a Sass partial**, so ``normalize.css`` becomes ``_normalize.scss``.

### Theming
Basic and controlled levels of theming (core settings such as colors, typography, etc.) can be set on a per-theme level using the ``_variables.scss`` partial in a configured themes ``static/themes/[theme-name]/sass/`` directory.

More advanced Sass/CSS overrides to particular parts of the styling/compilation can occur in specific partials (which are always listed at the bottom of each compilation section) in a configured themes ``static/themes/[theme-name]/sass/`` directory. 

**NOTE:** writing custom overrides for Open edX Sass/CSS is welcome, but not directly supported within the Open edX theming service currently.

### Main Style Compile
The main file (usually labelled ``main.scss``) should be the only Sass file from the whole code base not to begin with an underscore. This file should not contain anything but ``@import`` and comments.

Example ``main.scss`` compile: 

```scss
// ------------------------------
// edX [application-name]: Main Style Compile

// About: Sass compile for the edX [application-name]. This does not contain styles for other edX products/experiences (e.g. account/onboarding).

// ------------------------------
// #VENDOR
// ------------------------------
@import '../vendor/bourbon/bourbon';
@import '../vendor/neat/neat';
@import '../vendor/jquery-ui';
@import '../vendor/bi-app/bi-app-ltr';    // LTR support
@import '../vendor-extensions/jquery-ui';
...

// ------------------------------
// #UTILITIES
// ------------------------------
@import 'utilities/variables';
@import 'utilities/functions';
@import 'utilities/mixins';
@import 'utilities/helpers';
...

@import 'themes/[theme-name]/utilities.scss';

// ------------------------------
// #BASE
// ------------------------------
@import 'base/reset';
@import 'base/typography';
...
@import 'themes/[theme-name]/static/sass/base.scss';

// ------------------------------
// #COMPONENTS
// ------------------------------
@import 'components/buttons';
@import 'components/carousel';
@import 'components/cover';
@import 'components/dropdown';
...
@import 'themes/[theme-name]/static/sass/components.scss';

// ------------------------------
// #LAYOUTS
// ------------------------------
@import 'layouts/grid';
@import 'layouts/navigation';
@import 'layouts/header';
@import 'layouts/footer';
@import 'layouts/sidebar';
@import 'layouts/forms';
...
@import 'themes/[theme-name]/static/sass/layouts.scss';

// ------------------------------
// #VIEWS
// ------------------------------
@import 'views/admin';
@import 'views/home';
@import 'views/login';
@import 'views/register';
...
@import 'themes/[theme-name]/static/sass/views.scss';

// ------------------------------
// #SHAME
// ------------------------------
@import 'utilities/developers';
@import 'utilities/shame';
@import 'themes/[theme-name]/static/sass/overrides.scss';
```

#### Compiling Sass using edX UX Pattern Library
Example Application ``main.scss`` compile (using edX UX Pattern Library): 

```scss
// ------------------------------
// edX [application-name]: Main Style Compile

// About: Sass compile for the edX [application-name]. This does not contain styles for other edX products/experiences (e.g. account/onboarding).

// ------------------------------
// #UTILITIES
// ------------------------------
@import '{path to edX UX PL}/edx-pl/sass/utilities/variables';
@import '{path to edX UX PL}/edx-pl/sass/utilities/functions';
@import '{path to edX UX PL}/edx-pl/sass/utilities/mixins';
@import '{path to edX UX PL}/edx-pl/sass/utilities/helpers';

@import 'utilities.scss';

// ------------------------------
// #VENDOR
// ------------------------------
// specific vendor resources for [application-name]

// ------------------------------
// #BASE
// ------------------------------
@import '{path to edX UX PL}/edx-pl/sass/base/normalize';
@import '{path to edX UX PL}/edx-pl/sass/base/reset';
@import '{path to edX UX PL}/edx-pl/sass/base/typography';

@import 'base.scss';

// ------------------------------
// #COMPONENTS
// ------------------------------
@import '{path to edX UX PL}/edx-pl/sass/components/colors';
@import '{path to edX UX PL}/edx-pl/sass/components/buttons';
@import '{path to edX UX PL}/edx-pl/sass/components/headings';
@import '{path to edX UX PL}/edx-pl/sass/components/copy';
...
@import 'components.scss';

// ------------------------------
// #LAYOUTS
// ------------------------------
@import '{path to edX UX PL}/layouts/grid';
@import '{path to edX UX PL}/layouts/navigation';
@import '{path to edX UX PL}/layouts/header';
@import '{path to edX UX PL}/layouts/footer';
@import '{path to edX UX PL}/layouts/sidebar';
@import '{path to edX UX PL}/layouts/forms';
...
@import 'layouts';

// ------------------------------
// #VIEWS
// ------------------------------
@import 'views';

// ------------------------------
// #SHAME
// ------------------------------
@import 'overrides';
```

- - -

## UI Naming Conventions
Naming conventions in CSS are hugely useful in making your code more strict, more transparent, and more informative.

A good naming convention tells us:

* what type of thing a class does;
* where a class can be used;
* what (else) a class might be related to.

### Naming Syntax
The naming convention we follow is based off of hyphen (-) delimited strings:

```scss
// Examples: Yep
.navigation-primary { ... }

.has-errors { ... }

// Examples: Nope
.navigationPrimary { ... }

.has__errors { ... }
```

For larger, more interrelated pieces of UI that require a number of classes, we use a [BEM](http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/)-like naming convention in philosophy, but not in execution (i.e. no underscores or double-hyphens). This means when naming elements via class atrributes, we separate our classes into three groups:

* The sole root/purpose of the component;
* A child/part of the component;
* A variant or extension of the component.

Example of the syntax using the human anatomy: 

```scss
.person { ... }

.person-head { ... }

.person-foot { ... }

.person-foot-left { ... }
.person-foot-right { ... }
```

#### Over Extension
We do not condone extending this naming method too far into the UI as it creates bloat and fragility. Instead, think of each element/component using this method as its own walled room. It should be configurable within a house, but have all of its details defined by itself.

Here's that human anatomy example from above extended too far:

```scss
// Example: Yep
.hat { ... }

.hat-blue { ... }

...

.person { ... }

.person-head {
    ...

    .hat {
        ...
    }

    .hat-blue {
        ...
    }
}

// Example: Nope
.person { ... }

.person-head-hat { ... }

.person-head-hat-blue { ... }
```

This approach can be used for larger elements within a view or page, including elements like:

* Global elements like an application's header/footer;
* Layout/Grid configuration;
* UI messages;
* Navigation.

This approach is also used on consistent and specific smaller components that are used throughout the application, such as:

* Button/control types;
* Form layouts and fields;
* repeatable UI elements within a layout.


```scss
// Example: Yep
.header { ... }

.header-logo { ... }

.header-tagline { ... }

...

.header-edx { ... }

.header-edx-logo { ... }

.header-edx-tagline { ... }

// Example: Nope
.header {
    
    .header-logo { ... }

    .header-tagline { ... }
}

// Example: Nope
.header {
    
    img { ... }
}

.headerTagline { ... }

```

```scss
// Example: Yep
.course { ... }

.course-title { ... }

.course-image { ... }

.course-description { ... }

...

.listing-courses { ... }

.listing-courses {
    
    // specific within listing-centric styling of a course element
    .course { ... }

    .course-description { ... }
}
```

This approach allow us to manage specificity, nesting, and styling-scent (letting someone not intimately familiar with your markup to follow what's being styled).

**Note:** Since we follow [SMACSS](https://smacss.com/)'s stateful class conventions, any classes formatted this way should not consider state. Those state-focused classes will be added separately.

```scss
// Example: Yep
.course { ... }

.course {

    // STATE: is-loading
    &.is-loading { ... }
    
    // CASE: is not available
    &.is-unavailable { ... }

    // CASE: has reprequisites
    &.has-prereqs { ... }
}

// Example: Nope
.course.is-loading { ... }

// Example: Nope
.course-is-unavailable { ... }
```

### Naming Elements
Name an element something that is sensible, but somewhat ambiguous: aim for high reusability. For example, instead of a class like ``.site-nav``, choose something like ``.primary-nav``; rather than ``.footer-links``, favour a class like ``.sub-links``.

The first name of each of the two examples above is tied to a very specific use case: they can only be used as the site’s navigation or the footer’s links respectively. By using slightly more ambiguous names (such as those in the second of each two examples), we can increase our ability to reuse these components in different circumstances.

```scss
// Example: Yep
// Nicely abstracted, very portable, doesn’t risk becoming out of date.
.highlight-color {
    color: rgb(37, 184, 90);
}

// Example: Nope
// Runs the risk of becoming out of date; not very maintainable.
.blue {
    color: blue;
}

// Example: Nope
// Depends on location in order to be rendered properly and does not maintain homogenous specificity.
.header span {
    color: blue;
}

// Example: Nope
// Too specific; limits our ability to reuse.
.header-color {
    color: blue;
}
```

### Behavior-based Classes
!TODO: add rules.

### Layout
When we create layouts for our applications, we:

* always follow the existing grid system and configuration of the application
* define layout separate from other style rules
* adhere to existing responsive breakpoints (as well as strategically update these) for larger layout/grid system needs/issues

#### Archteype Layouts
Based on the views planned in each application, we create grid-based layouts that mirror the alignment of major sections/elements in each view, this may mean creating the following:

* simple layouts (1-column layout, empty layout);
* detail layouts (multi-column layouts);
* unique layouts (homepage, login, etc.)

#### View-based Layouts
When possible, we should apply archetype layouts to each view using a class on the ``<body>`` element, such as:

```HTML
<!-- Example: Yep -->
<!DOCTYPE html>
  <html>
    <head>
        ...
    </head>
    <body class="view-course-detail layout-detail">
        ...
    </body>
</html>
```

**Note**: When using or designing a layout, we adhere to the [The Separation of Concerns](http://cssguidelin.es/#the-separation-of-concerns) UI principle and markup/styles for the layout should be independent of the content within the layout.

#### Right to Left (RTL) Support
We are using [Bi-App Sass](http://anasnakawa.github.io/bi-app-sass/) to help reverse the layout of our UI for languages that read right to left. The primary css properties that are involved in flipping are ``float``, ``margin``, ``padding``, ``text-align``, ``positioning``, ``border-left`` and ``border-right``. When using these properties, we confirm if these properties need to be flipped for right-to-left languages. If so, use the bi-app mixins instead of the regular property: value syntax. For instructions on usage, check the [bi-app readme](https://github.com/anasnakawa/bi-app-sass/blob/master/README.md).

In addition to referencing the Bi-App Sass methods, we must import the correct Bi-App direction based on either RTL or LTR support in our main application compilation file with the proper suffix attached to the file:

Example ``main-ltr.scss`` compile: 

```scss
// ------------------------------
// edX [application-name]: Main Style Compile - LTR

// About: Sass compile for the edX [application-name],with Left to Right support. This does not contain styles for other edX products/experiences (e.g. account/onboarding). This should come before any vendor extensions or any application-centric Sass.

// ------------------------------
// #VENDOR
// ------------------------------
@import '../vendor/bourbon/bourbon';
@import '../vendor/neat/neat';
@import '../vendor/jquery-ui';
@import '../vendor/bi-app/bi-app-ltr';    // LTR support
...
@import '../vendor-extensions/jquery-ui';
...

// ------------------------------
// #UTILITIES
// ------------------------------
...
```

Example ``main-rtl.scss`` compile: 

```scss
// ------------------------------
// edX [application-name]: Main Style Compile - RTL

// About: Sass compile for the edX [application-name],with Right to Left support. This does not contain styles for other edX products/experiences (e.g. account/onboarding). This should come before any vendor extensions or any application-centric Sass.

// ------------------------------
// #VENDOR
// ------------------------------
@import '../vendor/bourbon/bourbon';
@import '../vendor/neat/neat';
@import '../vendor/jquery-ui';
@import '../vendor/bi-app/bi-app-rtl';    // RTL support
...
@import '../vendor-extensions/jquery-ui';
...

// ------------------------------
// #UTILITIES
// ------------------------------
...
```

### Responsive Breakpoints
Media queries should not be tied to specific devices. Media queries should take care of a range of screen sizes, until the design breaks and the next media query takes over. For the same reasons, breakpoints should not be named after devices but something more general.

At this point, any naming convention that makes crystal clear that a design is not intimately tied to a specific device type will do the trick, as long as it gives a sense of the range covered.

```scss
// Example: Yep
$breakpoints: (
    'medium':   (min-width: 800px),
    'large':    (min-width: 1000px),
    'huge':     (min-width: 1200px),
);

// Example: Nope
$breakpoints: (
    'mobile-phone': (min-width: 480px),
    'tablet':       (min-width: 800px),
    'computer':     (min-width: 1000px),
    'tv':           (min-width: 1200px),
);
```
#### Referencing Breakpoints
We lean on [Neat](http://neat.bourbon.io/) to help in [formatting our media queries](http://thoughtbot.github.io/neat-docs/latest/#media) and referencing our breakpoints.

```scss
// Example: using responsive breakpoints in _layouts.scss partial
.layout-multi-1+3 {
    @include outer-container();
    
    .layout-item-1 {
        // CASE: baseline/small
        @include fill-parent();

        // CASE: medium 
        @include media($breakpoints, medium) {
            @include span-columns(1);
        } 

        // CASE: large 
        @include media($breakpoints, large) {
            @include span-columns(3);
        }  
    }

    .layout-item-3 {
        // CASE: baseline/small
        @include fill-parent();
        @include omega();

        // CASE: medium 
        @include media($breakpoints, medium) {
            @include span-columns(3);
        } 

        // CASE: large 
        @include media($breakpoints, large) {
            @include span-columns(9);
        }  
    }
}
```

```scss
// Example: using responsive breakpoints in particular smaller UI element (course listing)
.course-actions {
    
    .action {
        // CASE: baseline/small
        display: block;
        margin-bottom: ($baseline);

        // CASE: medium 
        @include media($breakpoints, medium) {
            display: inline-block;
            vertical-align: middle;
            margin-top: 0;
            margin-right: ($baseline/2);
        }
    }

    .action-enroll {
        ...
    }

    .action-unenroll {
        ...
    }
}
```


### Sass Elements
There are a few things you can name in Sass, and it is important to name them well so the whole code base looks both consistent and easy to read:

* variables;
* functions;
* mixins.

Sass placeholders are deliberately omitted from this list since they can be considered as regular CSS selectors, thus following the same naming pattern as classes.

Regarding variables, functions and mixins, we stick to something very similar to CSS: **lowercase hyphen-delimited, and above all meaningful**.

```scss
$vertical-rhythm-baseline: 1.5rem;

@mixin size($width, $height: $width) {
    // ...
}

@function opposite-direction($direction) {
    // ...
}
```

#### Range of Variables
Please note that its important to not create too broad of range when defining variables.

```scss
// Example: Nope
$blue: rgb(0, 159, 230);
$blue-l1: tint($blue,20%);
$blue-l2: tint($blue,40%);
$blue-l3: tint($blue,60%);
$blue-l4: tint($blue,80%);
$blue-l5: tint($blue,90%);
$blue-d1: shade($blue,20%);
$blue-d2: shade($blue,40%);
$blue-d3: shade($blue,60%);
$blue-d4: shade($blue,80%);
$blue-s1: saturate($blue,15%);
$blue-s2: saturate($blue,30%);
$blue-s3: saturate($blue,45%);
$blue-u1: desaturate($blue,15%);
$blue-u2: desaturate($blue,30%);
$blue-u3: desaturate($blue,45%);
$blue-t0: rgba($blue, 0.125);
$blue-t1: rgba($blue, 0.25);
$blue-t2: rgba($blue, 0.50);
$blue-t3: rgba($blue, 0.75);
```

#### Elements that are Constants
If you happen to be a framework developer or library writer, you might find yourself dealing with variables that are not meant to be updated in any circumstances: constants. 

We suggest all-caps variables when they are constants. Not only is this a very old convention, but it also contrasts well with usual lowercased variables.

```scss
// Example: Yep
$CSS-POSITIONS: top, right, bottom, left, center;

// Example: Nope
$css-positions: top, right, bottom, left, center;
```

### Namespacing
In certain cases, scoping your rules with a namespace will avoid collisions or inherited style rules from elsewhere. Currently within edX, the following situations could benefit from namespacing:

* customizations (in a separate file) to vendor styling
* third-party contributions (UI additions, plug-ins, etc.)
* shared elements cross edX applications (e.g. xmodules, xblocks, course content)

For instance, if you're working on the Open Response Assessment (ORA) project, you could consider using a ora- namespace for specific ORA elements.

```scss
$ora-configuration: ( ... );

@function ora-status($ora-complete) {
    // ...
}
```

- - -

## UI Principles

### Object-Orientation
Object-orientation is a programming paradigm that breaks larger programs up into smaller, in(ter)dependent objects that all have their own roles and responsibilities. When applied to CSS, we call it object-oriented CSS, or OOCSS. OOCSS deals with the separation of UIs into structure and skin: breaking UI components into their underlying structural forms, and layering their cosmetic forms on separately. 

Example: 
```scss
// A simple, design-free button object. Extend this object with a `.button-*` skin class.
.button {
    display: inline-block;
    padding: 1rem 2rem;
    vertical-align: middle;
}

// button: confirm (extends .button)
.button-confirm {
    background-color: green;
    color: white;
}

// button: delete (extends .button)
.button-delete {
    background-color: red;
  color: white;
}
```

```HTML
<button class="button  button-delete">Delete</button>
```

We prefer the multiple-class approach over using a Sass ``@extend``: using multiple classes in your markup, as opposed to wrapping the classes up into one using a preprocessor:

* gives us a better paper-trail in our markup, and allows you to see quickly and explicitly which classes are acting on a piece of HTML;
* allows for greater composition in that classes are not tightly bound to other styles in your CSS.

Whenever we are building a UI component, we prefer breaking it into two parts: one for structural styles (paddings, layout, etc.) and another for skin (colours, typefaces, etc.).

### The Single Responsibility Principle
The single responsibility principle is a paradigm that, very loosely, states that all pieces of code (in our case, classes) should focus on doing one thing and one thing only.

What this means for us is that our CSS should be composed of a series of much smaller classes that focus on providing very specific and limited functionality.

```scss
// Example: Yep
.box {
  display: block;
  padding: 1rem;
}

.message {
  border-style: solid;
  border-width: .1rem 0;
  font-weight: bold;
}

.message-error {
  background-color: #fee;
  color: #f00;
}

.message-success {
  background-color: #efe;
  color: #0f0;
}

// Example: Nope
.error-message {
  display: block;
  padding: 10rem;
  border-top: .1rem solid #f00;
  border-bottom: .1rem solid #f00;
  background-color: #fee;
  color: #f00;
  font-weight: bold;
}

.success-message {
  display: block;
  padding: 1rem;
  border-top: .1rem solid #0f0;
  border-bottom: .1rem solid #0f0;
  background-color: #efe;
  color: #0f0;
  font-weight: bold;
}

```

- - -

## Selectors
Your selectors are fundamental to writing good CSS. We strive to adhere to the following principles when constructing our selectors:

* **Select what you want explicitly**, rather than relying on circumstance or coincidence. Good Selector Intent will rein in the reach and leak of your styles.
* **Write selectors for reusability**, so that you can work more efficiently and reduce waste and repetition.
* **Do not nest selectors unnecessarily**, because this will increase specificity and affect where else you can use your styles.
* **Do not qualify selectors unnecessarily**, as this will impact the number of different elements you can apply styles to.
* **Keep selectors as short as possible**, in order to keep specificity down and performance up.
* **Keep specificity (see below) as homogenous as possible** when choosing selectors

- - -

## Specificity
Specificity can, among other things:

* limit your ability to extend and manipulate a codebase;
* interrupt and undo CSS’ cascading, inheriting nature;
* cause avoidable verbosity in your project;
* prevent things from working as expected when moved into different environments;
* lead to serious developer frustration.

All of these issues are greatly magnified when working on a larger project with a number of developers contributing code. **We try to make sure there isn’t a lot of variance between selectors in our codebase, and that all selectors strive for as low a specificity as possible.**

We try to not:

* use IDs in our Sass/CSS;
* nest selectors when possible in our Sass;
* qualify classes in our selectors;
* chain selectors in our Sass/CSS.

- - -

## Style/Display Hacks

### Using !important
The !important modifier (example: ``margin: { 0 !important }``) does have a place in CSS projects, but only if used sparingly and proactively.

Proactive use of !important is when it is used before you’ve encountered any specificity problems; when it is used as a guarantee rather than as a fix. For example:

```css
.one-half {
    width: 50% !important;
}

.hidden {
    display: none !important;
}
```

These two helper, or utility, classes are very specific in their intentions: we would only use them if we wanted something to be rendered at 50% width or not rendered at all. If we didn’t want this behaviour, we would not use these classes, therefore whenever we do use them we will definitely want them to win.

Here we proactively apply !important to ensure that these styles always win. This is correct use of !important to guarantee that these trumps always work, and don’t accidentally get overridden by something else more specific.

Incorrect, reactive use of !important is when it is used to combat specificity problems after the fact: applying !important to declarations because of poorly architected CSS. For example, let’s imagine we have this HTML:

```HTML
<div class="content">
    <h2 class="heading-sub">...</h2>
</div>
```

…and this CSS:

```CSS
.content h2 {
    font-size: 2rem;
}

.heading-sub {
    font-size: 1.5em !important;
}
```

For the sake of specificity, preferred alternatives to the reactive use of !important (when refactoring or proper styling isn't an option) are: 

1. to place a rule within the ``_shame.scss`` Sass partial of an edX app. This places the rule at the end of the Sass's compilation order, and isolates/organizes rules that need to be addressed in the near future for others.

2. to chain the selector (preferrably in the ``_shame.scss`` file mentioned above) with a double of the original selector.

Example of double-chaining: 

```scss
.notes-copy.notes-copy { ... }
```

### Browser Specific Styles
We encourage troubleshooting and building code that will work in all browsers without special modifications.

While we do not have to support less than modern browsers (including < IE9) within our apps. If for some reason you do need to target older browsers, we recommend doing so with:

* with an eye towards [progressive enhancement](http://blog.teamtreehouse.com/progressive-enhancement-past-present-future);
* conditional IE comments for IE-specific display issues/adjustments (see below).

#### Targeting less than modern versions of IE
If using IE comments, class the html tag with the appropriate version of IE and scope/include any rules in the ``utilities/_shame.scss``.

##### HTML
```html
<!--[if IE 8]><html class="no-js lt-ie10 lt-ie9" lang=""><![endif]-->
<!--[if IE 9]><html class="no-js lt-ie10" lang=""><![endif]-->
<!--[if gt IE 9]><!--><html class="no-js" lang="en"><!--<![endif]-->

```

##### CSS
```css
// general styling
.foo {
  
}
// scoped to IE9 and below
.lt-ie10 .foo {
  
} 
```

- - -

## Git Etiquette
Never check production-ready files into a codebase's repository including:

* Sass sourcemaps
* compiled CSS files
* local editor/project management files

All of these files should be added to either your local global .gitignore file or better yet the edX project's .gitignore file.

Compiled CSS should be compressed as much as possible. 
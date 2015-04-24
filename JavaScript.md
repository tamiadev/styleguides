# Tâmia JavaScript Style Guide

Covers EcmaScript 6. Based on [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript).

## Table of Contents

  1. [References](#references)
  1. [Objects](#objects)
  1. [Arrays](#arrays)
  1. [Destructuring](#destructuring)
  1. [Strings](#strings)
  1. [Functions](#functions)
  1. [Arrow Functions](#arrow-functions)
  1. [Constructors](#constructors)
  1. [Modules](#modules)
  1. [Iterators and Generators](#iterators-and-generators)
  1. [Properties](#properties)
  1. [Variables](#variables)
  1. [Hoisting](#hoisting)
  1. [Comparison Operators & Equality](#comparison-operators--equality)
  1. [Blocks](#blocks)
  1. [Comments](#comments)
  1. [Whitespace](#whitespace)
  1. [Commas](#commas)
  1. [Semicolons](#semicolons)
  1. [Type Casting & Coercion](#type-casting--coercion)
  1. [Naming Conventions](#naming-conventions)
  1. [Accessors](#accessors)
  1. [Events](#events)
  1. [jQuery](#jquery)
  1. [ECMAScript 5 Compatibility](#ecmascript-5-compatibility)
  1. [Resources](#resources)
  1. [Contributors](#contributors)
  1. [License](#license)

## References

  - Use `const` for all of your references; avoid using `var`.

  > Why? This ensures that you can't reassign your references (mutation), which can lead to bugs and difficult to comprehend code.

    ```javascript
    // bad
    var a = 1;
    var b = 2;

    // good
    const a = 1;
    const b = 2;
    ```

  - If you must mutate references, use `let` instead of `var`.

  > Why? `let` is block-scoped rather than function-scoped like `var`.

    ```javascript
    // bad
    var count = 1;
    if (true) {
      count += 1;
    }

    // good, use the let.
    let count = 1;
    if (true) {
      count += 1;
    }
    ```

  - Note that both `let` and `const` are block-scoped.

    ```javascript
    // const and let only exist in the blocks they are defined in.
    {
      let a = 1;
      const b = 1;
    }
    console.log(a); // ReferenceError
    console.log(b); // ReferenceError
    ```


## Objects

  - Use the literal syntax for object creation.

    ```javascript
    // bad
    const item = new Object();

    // good
    const item = {};
    ```

  - Don't use [reserved words](http://es5.github.io/#x7.6.1) as keys. It won't work in IE8. [More info](https://github.com/airbnb/javascript/issues/61).

    ```javascript
    // bad
    const superman = {
      default: { clark: 'kent' },
      private: true
    };

    // good
    const superman = {
      defaults: { clark: 'kent' },
      hidden: true
    };
    ```

  - Use readable synonyms in place of reserved words.

    ```javascript
    // bad
    const superman = {
      class: 'alien'
    };

    // bad
    const superman = {
      klass: 'alien'
    };

    // good
    const superman = {
      type: 'alien'
    };
    ```

  <a name="es6-computed-properties"></a>
  - Use computed property names when creating objects with dynamic property names.

  > Why? They allow you to define all the properties of an object in one place.

    ```javascript

    function getKey(k) {
      return `a key named ${k}`;
    }

    // bad
    const obj = {
      id: 5,
      name: 'San Francisco'
    };
    obj[getKey('enabled')] = true;

    // good
    const obj = {
      id: 5,
      name: 'San Francisco',
      [getKey('enabled')]: true
    };
    ```

  <a name="es6-object-shorthand"></a>
  - Use object method shorthand.

    ```javascript
    // bad
    const atom = {
      value: 1,

      addValue: function (value) {
        return atom.value + value;
      }
    };

    // good
    const atom = {
      value: 1,

      addValue(value) {
        return atom.value + value;
      }
    };
    ```

  <a name="es6-object-concise"></a>
  - Use property value shorthand.

  > Why? It is shorter to write and descriptive.

    ```javascript
    const lukeSkywalker = 'Luke Skywalker';

    // bad
    const obj = {
      lukeSkywalker: lukeSkywalker
    };

    // good
    const obj = {
      lukeSkywalker
    };
    ```

  - Group your shorthand properties at the beginning of your object declaration.

  > Why? It's easier to tell which properties are using the shorthand.

    ```javascript
    const anakinSkywalker = 'Anakin Skywalker';
    const lukeSkywalker = 'Luke Skywalker';

    // bad
    const obj = {
      episodeOne: 1,
      twoJedisWalkIntoACantina: 2,
      lukeSkywalker,
      episodeThree: 3,
      mayTheFourth: 4,
      anakinSkywalker
    };

    // good
    const obj = {
      lukeSkywalker,
      anakinSkywalker,
      episodeOne: 1,
      twoJedisWalkIntoACantina: 2,
      episodeThree: 3,
      mayTheFourth: 4
    };
    ```


## Arrays

  - Use the literal syntax for array creation.

    ```javascript
    // bad
    const items = new Array();

    // good
    const items = [];
    ```

  - If you don't know array length use Array#push.

    ```javascript
    const someStack = [];


    // bad
    someStack[someStack.length] = 'abracadabra';

    // good
    someStack.push('abracadabra');
    ```

  <a name="es6-array-spreads"></a>
  - Use array spreads `...` to copy arrays.

    ```javascript
    // bad
    const len = items.length;
    const itemsCopy = [];
    let i;

    for (i = 0; i < len; i++) {
      itemsCopy[i] = items[i];
    }

    // good
    const itemsCopy = [...items];
    ```

  - To convert an array-like object to an array, use Array#from.

    ```javascript
    const foo = document.querySelectorAll('.foo');
    const nodes = Array.from(foo);
    ```

    **Note:** Requires [polyfill](http://babeljs.io/docs/usage/polyfill/) in Babel.


## Destructuring

  - Use object destructuring when accessing and using multiple properties of an object.

  > Why? Destructuring saves you from creating temporary references for those properties.

    ```javascript
    // bad
    function getFullName(user) {
      const firstName = user.firstName;
      const lastName = user.lastName;

      return `${firstName} ${lastName}`;
    }

    // good
    function getFullName(obj) {
      const { firstName, lastName } = obj;
      return `${firstName} ${lastName}`;
    }

    // best
    function getFullName({ firstName, lastName }) {
      return `${firstName} ${lastName}`;
    }
    ```

  - Use array destructuring.

    ```javascript
    const arr = [1, 2, 3, 4];

    // bad
    const first = arr[0];
    const second = arr[1];

    // good
    const [first, second] = arr;
    ```

  - Use object destructuring for multiple return values, not array destructuring.

  > Why? You can add new properties over time or change the order of things without breaking call sites.

    ```javascript
    // bad
    function processInput(input) {
      // then a miracle occurs
      return [left, right, top, bottom];
    }

    // the caller needs to think about the order of return data
    const [left, __, top] = processInput(input);

    // good
    function processInput(input) {
      // then a miracle occurs
      return { left, right, top, bottom };
    }

    // the caller selects only the data they need
    const { left, right } = processInput(input);
    ```



## Strings

  - Use single quotes `''` for strings.

    ```javascript
    // bad
    const name = "Bob Parr";

    // good
    const name = 'Bob Parr';
    ```

  - Strings longer than 120 characters should be written across multiple lines using string concatenation.
  - **Note:** If overused, long strings with concatenation could impact performance. [jsPerf](http://jsperf.com/ya-string-concat) & [Discussion](https://github.com/airbnb/javascript/issues/40).

    ```javascript
    // bad
    const errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';

    // bad
    const errorMessage = 'This is a super long error that was thrown because \
    of Batman. When you stop to think about how Batman had anything to do \
    with this, you would get nowhere \
    fast.';

    // good
    const errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about ' +
      'how Batman had anything to do with this, you would get nowhere fast.';
    ```

  <a name="es6-template-literals"></a>
  - When programmatically building up strings, use template strings instead of concatenation.

  > Why? Template strings give you a readable, concise syntax with proper newlines and string interpolation features.

    ```javascript
    // bad
    function sayHi(name) {
      return 'How are you, ' + name + '?';
    }

    // bad
    function sayHi(name) {
      return ['How are you, ', name, '?'].join();
    }

    // good
    function sayHi(name) {
      return `How are you, ${name}?`;
    }
    ```


## Functions

  - Use function declarations instead of function expressions.

  > Why? Function declarations are named, so they're easier to identify in call stacks. Also, the whole body of a function declaration is hoisted, whereas only the reference of a function expression is hoisted. This rule makes it possible to always use [Arrow Functions](#arrow-functions) in place of function expressions.

    ```javascript
    // bad
    const foo = function () {
    };

    // good
    function foo() {
    }
    ```

  - Function expressions:

    ```javascript
    // immediately-invoked function expression (IIFE)
    (() => {
      console.log('Welcome to the Internet. Please follow me.');
    })();
    ```

  - Never declare a function in a non-function block (if, while, etc). Assign the function to a variable instead. Browsers will allow you to do it, but they all interpret it differently, which is bad news bears.
  - **Note:** ECMA-262 defines a `block` as a list of statements. A function declaration is not a statement. [Read ECMA-262's note on this issue](http://www.ecma-international.org/publications/files/ECMA-ST/Ecma-262.pdf#page=97).

    ```javascript
    // bad
    if (currentUser) {
      function test() {
        console.log('Nope.');
      }
    }

    // good
    let test;
    if (currentUser) {
      test = () => {
        console.log('Yup.');
      };
    }
    ```

  - Never name a parameter `arguments`, this will take precedence over the `arguments` object that is given to every function scope.

    ```javascript
    // bad
    function nope(name, options, arguments) {
      // ...stuff...
    }

    // good
    function yup(name, options, args) {
      // ...stuff...
    }
    ```

  <a name="es6-rest"></a>
  - Never use `arguments`, opt to use rest syntax `...` instead.

  > Why? `...` is explicit about which arguments you want pulled. Plus rest arguments are a real Array and not Array-like like `arguments`.

    ```javascript
    // bad
    function concatenateAll() {
      const args = Array.prototype.slice.call(arguments);
      return args.join('');
    }

    // good
    function concatenateAll(...args) {
      return args.join('');
    }
    ```

  <a name="es6-default-parameters"></a>
  - Use default parameter syntax rather than mutating function arguments.

    ```javascript
    // really bad
    function handleThings(opts) {
      // No! We shouldn't mutate function arguments.
      // Double bad: if opts is falsy it'll be set to an object which may
      // be what you want but it can introduce subtle bugs.
      opts = opts || {};
      // ...
    }

    // still bad
    function handleThings(opts) {
      if (opts === void 0) {
        opts = {};
      }
      // ...
    }

    // good
    function handleThings(opts = {}) {
      // ...
    }
    ```


## Arrow Functions

  - When you must use function expressions (as when passing an anonymous function), use arrow function notation.

  > Why? It creates a version of the function that executes in the context of `this`, which is usually what you want, and is a more concise syntax.

  > Why not? If you have a fairly complicated function, you might move that logic out into its own function declaration.

    ```javascript
    // bad
    [1, 2, 3].map(function (x) {
      return x * x;
    });

    // good
    [1, 2, 3].map((x) => {
      return x * x;
    });
    ```

  - If the function body fits on one line, feel free to omit the braces and use implicit return. Otherwise, add the braces and use a `return` statement.

  > Why? Syntactic sugar. It reads well when multiple functions are chained together.

  > Why not? If you plan on returning an object.

    ```javascript
    // good
    [1, 2, 3].map((x) => x * x);

    // good
    [1, 2, 3].map((x) => {
      return { number: x };
    });
    ```

  - Always use parentheses around the arguments. Omitting the parentheses makes the functions less readable and only works for single arguments.

  > Why? These declarations read better with parentheses. They are also required when you have multiple parameters so this enforces consistency.

    ```javascript
    // bad
    [1, 2, 3].map(x => x * x);

    // good
    [1, 2, 3].map((x) => x * x);
    ```


## Constructors

  - Always use `class`. Avoid manipulating `prototype` directly.

  > Why? `class` syntax is more concise and easier to reason about.

    ```javascript
    // bad
    function Queue(contents = []) {
      this._queue = [...contents];
    }
    Queue.prototype.pop = function() {
      const value = this._queue[0];
      this._queue.splice(0, 1);
      return value;
    }


    // good
    class Queue {
      constructor(contents = []) {
        this._queue = [...contents];
      }
      pop() {
        const value = this._queue[0];
        this._queue.splice(0, 1);
        return value;
      }
    }
    ```

  - Use `extends` for inheritance.

  > Why? It is a built-in way to inherit prototype functionality without breaking `instanceof`.

    ```javascript
    // bad
    const inherits = require('inherits');
    function PeekableQueue(contents) {
      Queue.apply(this, contents);
    }
    inherits(PeekableQueue, Queue);
    PeekableQueue.prototype.peek = function() {
      return this._queue[0];
    }

    // good
    class PeekableQueue extends Queue {
      peek() {
        return this._queue[0];
      }
    }
    ```

  - Methods can return `this` to help with method chaining.

    ```javascript
    // bad
    Jedi.prototype.jump = function() {
      this.jumping = true;
      return true;
    };

    Jedi.prototype.setHeight = function(height) {
      this.height = height;
    };

    const luke = new Jedi();
    luke.jump(); // => true
    luke.setHeight(20); // => undefined

    // good
    class Jedi {
      jump() {
        this.jumping = true;
        return this;
      }

      setHeight(height) {
        this.height = height;
        return this;
      }
    }

    const luke = new Jedi();

    luke.jump()
      .setHeight(20);
    ```

  - It's okay to write a custom toString() method, just make sure it works successfully and causes no side effects.

    ```javascript
    class Jedi {
      contructor(options = {}) {
        this.name = options.name || 'no name';
      }

      getName() {
        return this.name;
      }

      toString() {
        return `Jedi - ${this.getName()}`;
      }
    }
    ```


## Modules

  - Always use modules (`import`/`export`) over a non-standard module system. You can always transpile to your preferred module system.

  > Why? Modules are the future, let's start using the future now.

    ```javascript
    // bad
    const TamiaStyleGuide = require('./TamiaStyleGuide');
    module.exports = TamiaStyleGuide.es6;

    // ok
    import TamiaStyleGuide from './TamiaStyleGuide';
    export default TamiaStyleGuide.es6;

    // best
    import { es6 } from './TamiaStyleGuide';
    export default es6;
    ```

  - Do not use wildcard imports.

  > Why? This makes sure you have a single default export.

    ```javascript
    // bad
    import * as TamiaStyleGuide from './TamiaStyleGuide';

    // good
    import TamiaStyleGuide from './TamiaStyleGuide';
    ```

  - And do not export directly from an import.

  > Why? Although the one-liner is concise, having one clear way to import and one clear way to export makes things consistent.

    ```javascript
    // bad
    // filename es6.js
    export { es6 as default } from './TamiaStyleGuide';

    // good
    // filename es6.js
    import { es6 } from './TamiaStyleGuide';
    export default es6;
    ```


## Iterators and Generators

  - Don't use iterators. Prefer JavaScript's higher-order functions like `map()` and `reduce()` instead of loops like `for-of`.

  > Why? This enforces our immutable rule. Dealing with pure functions that return values is easier to reason about than side-effects.

    ```javascript
    const numbers = [1, 2, 3, 4, 5];

    // bad
    let sum = 0;
    for (let num of numbers) {
      sum += num;
    }
    sum === 15;

    // good
    let sum = 0;
    numbers.forEach((num) => sum += num);
    sum === 15;

    // best (use the functional force)
    const sum = numbers.reduce((total, num) => total + num, 0);
    sum === 15;
    ```

  - Don't use generators.

  > Why? They don't transpile well to ES5.


## Properties

  - Use dot notation when accessing properties.

    ```javascript
    const luke = {
      jedi: true,
      age: 28
    };

    // bad
    const isJedi = luke['jedi'];

    // good
    const isJedi = luke.jedi;
    ```

  - Use subscript notation `[]` when accessing properties with a variable.

    ```javascript
    const luke = {
      jedi: true,
      age: 28
    };

    function getProp(prop) {
      return luke[prop];
    }

    const isJedi = getProp('jedi');
    ```


## Variables

  - Always use `const` to declare variables. Not doing so will result in global variables. We want to avoid polluting the global namespace. Captain Planet warned us of that.

    ```javascript
    // bad
    superPower = new SuperPower();

    // good
    const superPower = new SuperPower();
    ```

  - Use one `const` declaration per variable.

    > Why? It's easier to add new variable declarations this way, and you never have to worry about swapping out a `;` for a `,` or introducing punctuation-only diffs.

    ```javascript
    // bad
    const items = getItems(),
        goSportsTeam = true,
        dragonball = 'z';

    // bad
    // (compare to above, and try to spot the mistake)
    const items = getItems(),
        goSportsTeam = true;
        dragonball = 'z';

    // good
    const items = getItems();
    const goSportsTeam = true;
    const dragonball = 'z';
    ```

  - Group all your `const`s and then group all your `let`s.

  > Why? This is helpful when later on you might need to assign a variable depending on one of the previous assigned variables.

    ```javascript
    // bad
    let i, len, dragonball,
        items = getItems(),
        goSportsTeam = true;

    // bad
    let i;
    const items = getItems();
    let dragonball;
    const goSportsTeam = true;
    let len;

    // good
    const goSportsTeam = true;
    const items = getItems();
    let dragonball;
    let i;
    let length;
    ```

  - Assign variables where you need them, but place them in a reasonable place.

  > Why? `let` and `const` are block scoped and not function scoped.

    ```javascript
    // good
    function() {
      test();
      console.log('doing stuff..');

      //..other stuff..

      const name = getName();

      if (name === 'test') {
        return false;
      }

      return name;
    }

    // bad
    function() {
      const name = getName();

      if (!arguments.length) {
        return false;
      }

      return true;
    }

    // good
    function() {
      if (!arguments.length) {
        return false;
      }

      const name = getName();

      return true;
    }
    ```


## Hoisting

  - `var` declarations get hoisted to the top of their scope, their assignment does not. `const` and `let` declarations are blessed with a new concept called [Temporal Dead Zones (TDZ)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let#Temporal_dead_zone_and_errors_with_let). It's important to know why [typeof is no longer safe](http://es-discourse.com/t/why-typeof-is-no-longer-safe/15).

    ```javascript
    // we know this wouldn't work (assuming there
    // is no notDefined global variable)
    function example() {
      console.log(notDefined); // => throws a ReferenceError
    }

    // creating a variable declaration after you
    // reference the variable will work due to
    // variable hoisting. Note: the assignment
    // value of `true` is not hoisted.
    function example() {
      console.log(declaredButNotAssigned); // => undefined
      var declaredButNotAssigned = true;
    }

    // The interpreter is hoisting the variable
    // declaration to the top of the scope,
    // which means our example could be rewritten as:
    function example() {
      let declaredButNotAssigned;
      console.log(declaredButNotAssigned); // => undefined
      declaredButNotAssigned = true;
    }

    // using const and let
    function example() {
      console.log(declaredButNotAssigned); // => throws a ReferenceError
      console.log(typeof declaredButNotAssigned); // => throws a ReferenceError
      const declaredButNotAssigned = true;
    }
    ```

  - Anonymous function expressions hoist their variable name, but not the function assignment.

    ```javascript
    function example() {
      console.log(anonymous); // => undefined

      anonymous(); // => TypeError anonymous is not a function

      var anonymous = function() {
        console.log('anonymous function expression');
      };
    }
    ```

  - Named function expressions hoist the variable name, not the function name or the function body.

    ```javascript
    function example() {
      console.log(named); // => undefined

      named(); // => TypeError named is not a function

      superPower(); // => ReferenceError superPower is not defined

      var named = function superPower() {
        console.log('Flying');
      };
    }

    // the same is true when the function name
    // is the same as the variable name.
    function example() {
      console.log(named); // => undefined

      named(); // => TypeError named is not a function

      var named = function named() {
        console.log('named');
      }
    }
    ```

  - Function declarations hoist their name and the function body.

    ```javascript
    function example() {
      superPower(); // => Flying

      function superPower() {
        console.log('Flying');
      }
    }
    ```

  - For more information refer to [JavaScript Scoping & Hoisting](http://www.adequatelygood.com/2010/2/JavaScript-Scoping-and-Hoisting) by [Ben Cherry](http://www.adequatelygood.com/).


## Comparison Operators & Equality

  - Use `===` and `!==` over `==` and `!=`.
  - Comparison operators are evaluated using coercion with the `ToBoolean` method and always follow these simple rules:

    + **Objects** evaluate to **true**
    + **Undefined** evaluates to **false**
    + **Null** evaluates to **false**
    + **Booleans** evaluate to **the value of the boolean**
    + **Numbers** evaluate to **false** if **+0, -0, or NaN**, otherwise **true**
    + **Strings** evaluate to **false** if an empty string `''`, otherwise **true**

    ```javascript
    if ([0]) {
      // true
      // An array is an object, objects evaluate to true
    }
    ```

  - Use shortcuts.

    ```javascript
    // bad
    if (name !== '') {
      // ...stuff...
    }

    // good
    if (name) {
      // ...stuff...
    }

    // bad
    if (collection.length > 0) {
      // ...stuff...
    }

    // good
    if (collection.length) {
      // ...stuff...
    }
    ```

  - For more information see [Truth Equality and JavaScript](http://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/#more-2108) by Angus Croll.


## Blocks

  - Use braces with all multi-line blocks.

    ```javascript
    // bad
    if (test)
      return false;

    // bad
    if (test) return false;

    // bad
    function() { return false; }

    // good
    if (test) {
      return false;
    }

    // good
    function() {
      return false;
    }
    ```

  - If you're using multi-line blocks with `if` and `else`, put `else` on a separate line after the `if` block's closing brace.

    ```javascript
    // bad
    if (test) {
      thing1();
      thing2();
    } else {
      thing3();
    }

    // good
    if (test) {
      thing1();
      thing2();
    }
    else {
      thing3();
    }
    ```


## Comments

  - Use `/** ... */` for multiline comments. Include a description, specify types and values for all parameters and return values.

    ```javascript
    // bad
    // make() returns a new element
    // based on the passed in tag name
    //
    // @param {String} tag
    // @return {Element} element
    function make(tag) {

      // ...stuff...

      return element;
    }

    // good
    /**
     * Returns a new element based on the passed in tag name.
     *
     * @param {String} tag
     * @return {Element} element
     */
    function make(tag) {

      // ...stuff...

      return element;
    }
    ```

  - Use `//` for single line comments. Place single line comments on a newline above the subject of the comment. Put an empty line before the comment.

    ```javascript
    // bad
    const active = true; // is current tab

    // better
    const active = true;  // Is current tab

    // good
    // Is current tab
    const active = true;

    // bad
    function getType() {
      console.log('fetching type...');
      // set the default type to 'no type'
      const type = this._type || 'no type';

      return type;
    }

    // good
    function getType() {
      console.log('fetching type...');

      // Set the default type to 'no type'
      const type = this._type || 'no type';

      return type;
    }
    ```

  - Prefixing your comments with `FIXME` or `TODO` (`@todo`) helps other developers quickly understand if you're pointing out a problem that needs to be revisited, or if you're suggesting a solution to the problem that needs to be implemented. These are different than regular comments because they are actionable. The actions are `FIXME -- need to figure this out` or `TODO -- need to implement`.

  - Use `// FIXME:` to annotate problems.

    ```javascript
    function Calculator() {

      // FIXME: shouldn't use a global here
      total = 0;

      return this;
    }
    ```

  - Use `// TODO:` to annotate solutions to problems.

    ```javascript
    function Calculator() {

      // TODO: total should be configurable by an options param
      this.total = 0;

      return this;
    }
    ```


## Whitespace

  - Use soft tabs set to 4 spaces.

    ```javascript
    // bad
    function() {
    ∙∙const name;
    }

    // bad
    function() {
    ∙const name;
    }

    // good
    function() {
    ∙∙∙∙const name;
    }
    ```

  - Place 1 space before the leading brace.

    ```javascript
    // bad
    function test(){
      console.log('test');
    }

    // good
    function test() {
      console.log('test');
    }

    // bad
    dog.set('attr',{
      age: '1 year',
      breed: 'Bernese Mountain Dog'
    });

    // good
    dog.set('attr', {
      age: '1 year',
      breed: 'Bernese Mountain Dog'
    });
    ```

  - Set off operators with spaces.

    ```javascript
    // bad
    const x=y+5;

    // good
    const x = y + 5;
    ```

  - End files with a single newline character.

    ```javascript
    // bad
    (function(global) {
      // ...stuff...
    })(this);
    ```

    ```javascript
    // bad
    (function(global) {
      // ...stuff...
    })(this);↵
    ↵
    ```

    ```javascript
    // good
    (function(global) {
      // ...stuff...
    })(this);↵
    ```

  - Use indentation when making long method chains. Use a leading dot, which
    emphasizes that the line is a method call, not a new statement.

    ```javascript
    // bad
    $('#items').find('.selected').highlight().end().find('.open').updateCount();

    // bad
    $('#items').
      find('selected').
        highlight().
        end().
      find('.open').
        updateCount();

    // good
    $('#items')
      .find('.selected')
        .highlight()
        .end()
      .find('.open')
        .updateCount();

    // bad
    const leds = stage.selectAll('.led').data(data).enter().append('svg:svg').class('led', true)
        .attr('width',  (radius + margin) * 2).append('svg:g')
        .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
        .call(tron.led);

    // good
    const leds = stage.selectAll('.led')
        .data(data)
      .enter().append('svg:svg')
        .class('led', true)
        .attr('width', (radius + margin) * 2)
      .append('svg:g')
        .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
        .call(tron.led);
    ```

  - Leave a blank line after blocks and before the next statement

    ```javascript
    // bad
    if (foo) {
      return bar;
    }
    return baz;

    // good
    if (foo) {
      return bar;
    }

    return baz;

    // bad
    const obj = {
      foo: function() {
      },
      bar: function() {
      }
    };
    return obj;

    // good
    const obj = {
      foo: function() {
      },

      bar: function() {
      }
    };

    return obj;
    ```


## Commas

  - Leading commas: **Nope.**

    ```javascript
    // bad
    const story = [
        once
      , upon
      , aTime
    ];

    // good
    const story = [
      once,
      upon,
      aTime
    ];

    // bad
    const hero = {
        firstName: 'Bob'
      , lastName: 'Parr'
      , heroName: 'Mr. Incredible'
      , superPower: 'strength'
    };

    // good
    const hero = {
      firstName: 'Bob',
      lastName: 'Parr',
      heroName: 'Mr. Incredible',
      superPower: 'strength'
    };
    ```

  - Additional trailing comma: **Nope.** This can cause problems with IE6/7 and IE9 if it's in quirksmode. Also, in some implementations of ES3 would add length to an array if it had an additional trailing comma. This was clarified in ES5 ([source](http://es5.github.io/#D)):

  > Edition 5 clarifies the fact that a trailing comma at the end of an ArrayInitialiser does not add to the length of the array. This is not a semantic change from Edition 3 but some implementations may have previously misinterpreted this.

    ```javascript
    // bad
    const hero = {
      firstName: 'Kevin',
      lastName: 'Flynn',
    };

    const heroes = [
      'Batman',
      'Superman',
    ];

    // good
    const hero = {
      firstName: 'Kevin',
      lastName: 'Flynn'
    };

    const heroes = [
      'Batman',
      'Superman'
    ];
    ```


## Semicolons

  - **Yup.**

    ```javascript
    // bad
    (function() {
      const name = 'Skywalker'
      return name
    })()

    // good
    (() => {
      const name = 'Skywalker';
      return name;
    })();

    // good (guards against the function becoming an argument when two files with IIFEs are concatenated)
    ;(() => {
      const name = 'Skywalker';
      return name;
    })();
    ```

    [Read more](http://stackoverflow.com/a/7365214/1712802).


## Type Casting & Coercion

  - Perform type coercion at the beginning of the statement.
  - Strings:

    ```javascript
    // => this.reviewScore = 9;

    // bad
    const totalScore = this.reviewScore + '';

    // good
    const totalScore = String(this.reviewScore);
    ```

  - Use `parseInt` for Numbers and always with a radix for type casting.

    ```javascript
    const inputValue = '4';

    // bad
    const val = new Number(inputValue);

    // bad
    const val = +inputValue;

    // bad
    const val = inputValue >> 0;

    // bad
    const val = parseInt(inputValue);

    // good
    const val = Number(inputValue);

    // good
    const val = parseInt(inputValue, 10);
    ```

  - If for whatever reason you are doing something wild and `parseInt` is your bottleneck and need to use Bitshift for [performance reasons](http://jsperf.com/coercion-vs-casting/3), leave a comment explaining why and what you're doing.

    ```javascript
    // good
    /**
     * parseInt was the reason my code was slow.
     * Bitshifting the String to coerce it to a
     * Number made it a lot faster.
     */
    const val = inputValue >> 0;
    ```

  - **Note:** Be careful when using bitshift operations. Numbers are represented as [64-bit values](http://es5.github.io/#x4.3.19), but Bitshift operations always return a 32-bit integer ([source](http://es5.github.io/#x11.7)). Bitshift can lead to unexpected behavior for integer values larger than 32 bits. [Discussion](https://github.com/airbnb/javascript/issues/109). Largest signed 32-bit Int is 2,147,483,647:

    ```javascript
    2147483647 >> 0 //=> 2147483647
    2147483648 >> 0 //=> -2147483648
    2147483649 >> 0 //=> -2147483647
    ```

  - Booleans:

    ```javascript
    const age = 0;

    // bad
    const hasAge = new Boolean(age);

    // good
    const hasAge = Boolean(age);

    // good
    const hasAge = !!age;
    ```


## Naming Conventions

  - Avoid single letter names. Be descriptive with your naming.

    ```javascript
    // bad
    function q() {
      // ...stuff...
    }

    // good
    function query() {
      // ..stuff..
    }
    ```

  - Use camelCase when naming objects, functions, and instances.

    ```javascript
    // bad
    const OBJEcttsssss = {};
    const this_is_my_object = {};
    function c() {}
    const u = new user({
      name: 'Bob Parr'
    });

    // good
    const thisIsMyObject = {};
    function thisIsMyFunction() {}
    const user = new User({
      name: 'Bob Parr'
    });
    ```

  - Use PascalCase when naming constructors or classes.

    ```javascript
    // bad
    function user(options) {
      this.name = options.name;
    }

    const bad = new user({
      name: 'nope'
    });

    // good
    class User {
      constructor(options) {
        this.name = options.name;
      }
    }

    const good = new User({
      name: 'yup'
    });
    ```

  - Use a leading underscore `_` when naming private properties.

    ```javascript
    // bad
    this.__firstName__ = 'Panda';
    this.firstName_ = 'Panda';

    // good
    this._firstName = 'Panda';
    ```

  - Don't save references to `this`. Use arrow functions or Function.bind.

    ```javascript
    // bad
    function foo() {
      const self = this;
      return function() {
        console.log(self);
      };
    }

    // bad
    function foo() {
      const that = this;
      return function() {
        console.log(that);
      };
    }

    // good
    function foo() {
      return () => {
        console.log(this);
      };
    }
    ```

  - If your file exports a single class, your filename should be exactly the name of the class.

    ```javascript
    // file contents
    class CheckBox {
      // ...
    }
    export default CheckBox;

    // in some other file
    // bad
    import CheckBox from './checkBox';

    // bad
    import CheckBox from './check_box';

    // good
    import CheckBox from './CheckBox';
    ```

  - Use camelCase when you export-default a function. Your filename should be identical to your function's name.

    ```javascript
    function makeStyleGuide() {
    }

    export default makeStyleGuide;
    ```

  - Use PascalCase when you export a singleton / function library / bare object.

    ```javascript
    const TamiaStyleGuide = {
      es6: {
      }
    };

    export default TamiaStyleGuide;
    ```


## Accessors

  - Accessor functions for properties are not required.
  - If you do make accessor functions use getVal() and setVal('hello').

    ```javascript
    // bad
    dragon.age();

    // good
    dragon.getAge();

    // bad
    dragon.age(25);

    // good
    dragon.setAge(25);
    ```

  - If the property is a boolean, use isVal() or hasVal().

    ```javascript
    // bad
    if (!dragon.age()) {
      return false;
    }

    // good
    if (!dragon.hasAge()) {
      return false;
    }
    ```

  - It's okay to create get() and set() functions, but be consistent.

    ```javascript
    class Jedi {
      constructor(options = {}) {
        const lightsaber = options.lightsaber || 'blue';
        this.set('lightsaber', lightsaber);
      }

      set(key, val) {
        this[key] = val;
      }

      get(key) {
        return this[key];
      }
    }
    ```


## Events

  - When attaching data payloads to events (whether DOM events or something more proprietary like Backbone events), pass a hash instead of a raw value. This allows a subsequent contributor to add more data to the event payload without finding and updating every handler for the event. For example, instead of:

    ```javascript
    // bad
    $(this).trigger('listingUpdated', listing.id);

    ...

    $(this).on('listingUpdated', function(e, listingId) {
      // do something with listingId
    });
    ```

    prefer:

    ```javascript
    // good
    $(this).trigger('listingUpdated', { listingId : listing.id });

    ...

    $(this).on('listingUpdated', function(e, data) {
      // do something with data.listingId
    });
    ```

  

## jQuery

  - Do *not* prefix jQuery object variables with a `$`.

    ```javascript
    // bad
    const $sidebar = $('.sidebar');

    // good
    const sidebar = $('.sidebar');
    ```

  - Cache jQuery lookups.

    ```javascript
    // bad
    function setSidebar() {
      $('.sidebar').hide();

      // ...stuff...

      $('.sidebar').css({
        'background-color': 'pink'
      });
    }

    // good
    function setSidebar() {
      const sidebar = $('.sidebar');
      sidebar.hide();

      // ...stuff...

      sidebar.css({
        'background-color': 'pink'
      });
    }
    ```

  - For DOM queries use js-classes `$('.js-sidebar')`.
  - Use `find` with scoped jQuery object queries.

    ```javascript
    // bad
    $('ul', '.sidebar').hide();

    // bad
    $('.sidebar').find('ul').hide();

    // bad
    $('.sidebar ul').hide();

    // good
    $('.js-sidebar').hide();

    // good
    sidebar.find('.js-item').hide();
    ```


## ECMAScript 5 Compatibility

  - Refer to [Kangax](https://twitter.com/kangax/)'s ES5 [compatibility table](http://kangax.github.com/es5-compat-table/).


## Resources

See [a big list or resources](https://github.com/airbnb/javascript#resources) in an Airbnb JavaScript Style Guide.


## Contributors

  - Airbnb JavaScript Style Guide’s [contributors](https://github.com/airbnb/javascript/graphs/contributors)
  - [Artem Sapegin](http://sapegin.me)


## License

The MIT License, see the included [License.md](License.md) file.

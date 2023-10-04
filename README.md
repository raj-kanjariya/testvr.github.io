# 1. Introduction
This document serves as the **complete** definition of MySegmenter Technologies Inc. coding guidelines, standards for source code in the JavaScript programming language.

This document addresses various aspects beyond mere formatting aesthetics, such as indentation and spacing. It also covers conventions and coding standards, including style and best practices. However, the primary focus of this document is on the unequivocal rules that are universally applicable, and avoids giving advice that isn't clearly enforceable (whether by human or tool). This guide draws inspiration from various organizations' documents that outline code conventions for the JavaScript programming language.

The code examples in this document are not considered normative. In other words, while they follow the MySegmenter Technologies style, they do not necessarily demonstrate the only stylish way to present code. You should not feel obligated to strictly follow the optional formatting choices made in these examples as strict rules.

# 2. Source file
## 2.1 File name
File names must be all lowercase and may include underscores (_) or dashes (-), but no additional punctuation. Follow the convention that your project uses. Filenames’ extension must be .js.

If a file contains a `class`, then file should exactly match the name of the class it contains with .js extension.

## 2.2 Special character

### 2.2.1 Whitespace characters
Aside from the line terminator sequence, the ASCII horizontal space character (0x20) is the only whitespace character that appears anywhere in a source file. This implies that.
  1. All other whitespace characters in string literals are escaped, and
  2. Tab characters are not used for indentation.

### 2.2.2 Special escape sequences
For any character that has a special escape sequence (`\'`, `\"`, `\\`, `\b`, `\f`, `\n`, `\r`, `\t`, `\v`), that sequence is used rather than the corresponding numeric escape (e.g `\x0a`, `\u000a`, or `\u{a}`). Legacy octal escapes are never used.

### 2.2.3 Non-ASCII characters
For the remaining non-ASCII characters, either the actual Unicode character (e.g. `∞`) or the equivalent hex or Unicode escape (e.g. `\u221e`) is used, depending only on which makes the code easier to read and understand.

_Tip: In the Unicode escape case, and occasionally even when actual Unicode characters are used, an explanatory comment can be very helpful._

Example:
```
/* Best: perfectly clear even without a comment. */
const units = 'μs';

/* Allowed: but unnecessary as μ is a printable character. */
const units = '\u03bcs'; // 'μs'

/* Poor: the reader has no idea what character this is. */
const units = '\u03bcs';
```
## 2.3 Effective use of <script> tag in HTML for JavaScript
JavaScript programs should be stored in and delivered as .js files.

JavaScript code should not be embedded in HTML files unless the code is specific to a single session. Code in HTML adds significantly to pageweight with no opportunity for mitigation by caching and compression.

`<script src=filename.js>` tag should be placed as late in the body as possible. This reduces the effects of delays imposed by script loading on other page components. There is no need to use the language or type attributes. It is the server, not the script tag, that determines the MIME type.

# 3. Source file structure
(add more context)
1 License or copyright information
1. JSDoc file overview
1. ES module import statements
1. require and requireType statements
1. The file’s implementation

## 3.1. License or copyright information, if present
If a file contains license or copyright information, it should be included in this section.

## 3.2. JSDoc file overview
See [7.5. Top/file-level comments]( #top/file-level comments) for formatting rules.

## 3.3. ES module import statements
### 3.3.1. Imports
Import statements must not be line wrapped and are therefore an exception to the 80-column limit.
#### 3.3.1.1. Import paths
ES module files must use the `import` statement to import other ES module files.

Example:
```
// Importing a default export from another ES module file
import defaultExport from './anotherModule.js';

// Importing multiple named exports from another ES module file
import { export1, export2 } from './anotherModule.js';

// Importing all named exports from another ES module file
import * as allExports from './anotherModule.js';
```

#### 3.3.1.2. File extensions in import paths
The inclusion of the `.js` file extension in import paths is mandatory and should not be omitted.

Preferred:
```
import '../directory/file.js';
```

Disallowed:
```
import '../directory/file';
```

#### 3.3.1.3. Importing the same file multiple times
Do not import the same file multiple times. This can make it hard to determine the aggregate imports of a file.

Example:
```
// Imports have the same path, but since it doesn't align it can be hard to see.
import { short } from './long/path/to/a/file.js';
import { aLongNameThatBreaksAlignment } from './long/path/to/a/file.js';
```

#### 3.3.1.4. Naming imports
##### 3.3.1.4.1. Naming module imports
Module import names (`import * as name`) should be in `lowerCamelCase` and derived from the imported file names.

Example:
```
import * as fileOne from '../file-one.js';
import * as fileTwo from '../file_two.js';
import * as fileThree from '../filethree.js';

import * as libString from './lib/string.js';
import * as math from './math/math.js';
import * as vectorMath from './vector/math.js';
```

##### 3.3.1.4.2. Naming default imports
Default import names are derived from the imported file name and follow the rules in [Rules by identifier type] (add link).

##### 3.3.1.4.3. Best practices for named imports
* Keep the same name: When you import symbols (variables, functions, objects, etc.) using the `import {name}` syntax, you should generally keep the imported name the same as the original name in the module you are importing from. For example, if you import something named Cat, you should keep it as Cat in your code.
* Avoid aliasing: Avoid aliasing imports using the `import {SomeThing as SomeOtherThing}` syntax. In other words, don't rename imported symbols unless it's necessary to avoid naming conflicts.
* Prefer module imports: If there are naming conflicts or you need to rename imports, it recommends using a module import (`import *)` or renaming the exports in the original module itself. This way, you can avoid renaming individual imports and keep the naming consistent.

However, if you need to rename imports, you should use the original module's file name or path in the resulting alias.

Example:
```
import { Cat as BigCat } from './biganimals.js';
import { Cat as DomesticatedCat } from './domesticatedanimals.js';
```

### 3.3.2. Exports
* Exported symbols: Symbols are only exported from a module if they are intended to be used outside of that module.
* Module-local symbols: Symbols that are not intended to be used outside of the module are considered module-local.  Non-exported module-local symbols are not explicitly marked as @private nor do their names start with an underscore.
* No prescribed ordering: There is no specific or required order in which you should list exported symbols and module-local symbols within your module.

When writing code, always use named exports. You can apply the `export` keyword to a declaration, or use the `export {name};` syntax.

# 4. Formatting
_block-like construct_ refers to the body of a class, function, method, or brace-delimited block of code. Note that, [Array literals](link here) and [Object literals](link here), any array or object literal may optionally be treated as if it were a block-like construct.

_Tip: Use `clang-format`. The JavaScript community has invested effort to make sure clang-format "does the right thing" on JavaScript files. `clang-format` has integration with several popular editors._

## 4.1. Braces

### 4.1.1. Braces are used for all control structures.
Braces are required for all control structures (i.e. `if`, `else`, `for`, `do`, `while`, as well as any others), even if the body contains only a single statement. The first statement of a non-empty block must begin on its own line.

Disallowed:
```
if (someVeryLongCondition())
    doSomething();

for (let i = 0; i < foo.length; i++) bar(foo[i]);
```

Exception:

A simple if statement that can fit entirely on a single line with no wrapping (and that doesn’t have an else) may be kept on a single line with no braces when it improves readability. This is the only case in which a control structure may omit braces and newlines.
```
if (shortCondition()) foo();
```

### 4.1.2. Nonempty blocks: K&R style
Braces follow the [Kernighan and Ritchie style] (http://syque.com/cstyle/ch6.7.htm) for nonempty blocks and block-like constructs:
  * No line break before the opening brace.
  * Line break after the opening brace.
  * Line break before the closing brace.
  * Line break after the closing brace if that brace terminates a statement or the body of a function or class statement, or a class method. Specifically, there is no line break after the brace if it is followed by `else`, `catch`, `while`, or a comma, semicolon, or right-parenthesis.

Example:
```
class InnerClass {
    constructor() { }

    /** @param {number} foo */
    method(foo) {
        if (condition(foo)) {
            try {
                // ..
            } catch (err) {
                // ..
            }
        }
    }
}
```

### 4.1.3. Empty blocks: may be concise
An empty block or block-like construct may be closed immediately after it is opened, with no characters, space, or line break in between (i.e. {}), unless it is a part of a multi-block statement (one that directly contains multiple blocks: `if`/`else` or `try`/`catch`/`finally`).

Example:
```
// Empty block construct closed immediately
function doNothing() {}

// Empty multi-block statement doesn’t closed immediately
if (condition) {
  // ..
} else if (otherCondition) {
  // ..
} else {
  // ..
}

```
Disallowed:
```
if (condition) {
    // ..
} else if (otherCondition) {} else {
    // ..
}

try {
    // ..
} catch (e) {}
```

## 4.2. Block indentation: +4 spaces

Each time a new block or block-like construct is opened, the indent increases by four spaces. When the block ends, the indent returns to the previous indent level. The indent level applies to both code and comments throughout the block.

Use of tabs should be avoided because (as of this writing in the 21st Century) there still is not a standard for the placement of tabstops. The use of spaces can produce a larger filesize, but the size is not significant over local networks, and the difference is eliminated by minification.

### 4.2.1. Array literals: optionally "block-like"
Any array literal may optionally be formatted as if it were a “block-like construct.” For example, the following are all valid (not an exhaustive list):

```
const a = [
    0,
    1,
    2,
];

const b =
    [0, 1, 2];
	
const c = [0, 1, 2];

someMethod(foo, [
    0, 1, 2,
], bar);
```

### 4.2.2. Class literals
Class literals (whether declarations or expressions) are indented as blocks. Do not add semicolons after methods, or after the closing brace of a class declaration (statements—such as assignments—that contain class expressions are still terminated with a semicolon). Use the `extends` keyword, but not the `@extends` JSDoc annotation unless the class extends a templatized type.

Example:
```
class Foo {
    constructor() {
        /** @type {number} */
        this.x = 42;
    }

    /** @return {number} */
    method() {
        return this.x;
    }
}
Foo.Empty = class {};
```

```
/** @extends {Foo<string>} */
foo.Bar = class extends Foo {
    /** @override */
    method() {
        return super.method() / 2;
    }
};

/** @interface */
class Frobnicator {
    /** @param {string} message */
    frobnicate(message) {}
}
```

### 4.2.3. Function expressions
When declaring an anonymous function in the list of arguments for a function call, the body of the function is indented four spaces more than the preceding indentation depth.

Example:
```
prefix.something.reallyLongFunctionName('whatever', (a1, a2) => {
    // Indent the function body +4 relative to indentation depth
    // of the 'prefix' statement one line above.
    if (a1.equals(a2)) {
        someOtherLongFunctionName(a1);
    } else {
        andNowForSomethingCompletelyDifferent(a2.parrot);
    }
});

some.reallyLongFunctionCall(arg1, arg2, arg3)
    .thatsWrapped()
    .then((result) => {
        // Indent the function body +4 relative to the indentation depth
        // of the '.then()' call.
        if (result) {
            result.use();
        }
    });
```

### 4.2.4. Switch statements
As with any other block, the contents of a switch block are indented +4. After a switch label, a newline appears, and the indentation level is increased +4, exactly as if a block were being opened. An explicit block may be used if required by lexical scoping. The following switch label returns to the previous indentation level, as if a block had been closed.

A blank line is optional between a `break` and the following case.

Example:
```
switch (animal) {
    case Animal.BANDERSNATCH:
        // ..
        break;

    case Animal.JABBERWOCK:
        // ..
        break;
    default:
        throw new Error('Unknown animal');
}
```

## 4.3.  Statements
### 4.3.1.  One statement per line
Each statement is followed by a line-break.

### 4.3.2. Semicolons are required
Every statement must be terminated with a semicolon. Relying on automatic semicolon insertion is forbidden.

## 4.4. Column limit: 80
JavaScript code has a column limit of 80 characters. Except as noted below, any line that would exceed this limit must be line-wrapped, as explained in [Line-wrapping](link here).

Exceptions:
* custom module (add more context)
* ES module `import` and `export` statements (see [imports](link here) and [exports](link here). (add more context)
* Lines where obeying the column limit is not possible or would hinder discoverability. Examples include:
  * A long URL which should be clickable in source.
  * A shell command intended to be copied-and-pasted.
  * A long string literal which may need to be copied or searched for wholly (e.g., a long file path).

## 4.5. Line-wrapping
_Line wrapping_ is breaking a chunk of code into multiple lines to obey column limit, where the chunk could otherwise legally fit in a single line. There is no comprehensive, deterministic formula showing exactly how to line-wrap in every situation. Very often there are several valid ways to line-wrap the same piece of code.

Note: While the typical reason for line-wrapping is to avoid overflowing the column limit, even code that would in fact fit within the column limit may be line-wrapped at the author's discretion.

### 4.5.1 Where to break
The prime directive of line-wrapping is: prefer to break at a higher syntactic level. 

Preferred:
```
currentEstimate =
    calc(currentEstimate + x * currentEstimate) /
        2.0;
```
Discouraged:
```
currentEstimate = calc(currentEstimate + x *
    currentEstimate) / 2.0;
```

In the preceding example, the syntactic levels from highest to lowest are as follows: assignment, division, function call, parameters, number constant.

Operators are wrapped as follows:
* When a line is broken at an operator the break comes after the symbol.
* This does not apply to the "dot" (`.`), which is not actually an operator.
* A method or constructor name stays attached to the open parenthesis (`(`) that follows it.
* A comma (`,`) stays attached to the token that precedes it.

Note: The primary goal for line wrapping is to have clear code, not necessarily code that fits in the smallest number of lines.

### 4.5.2 Indent continuation lines at least +4 spaces
When line-wrapping, each line after the first (each continuation line) is indented at least +4 from the original line.

When there are multiple continuation lines, indentation may be varied beyond +4 as appropriate. In general, continuation lines at a deeper syntactic level are indented by larger multiples of 4, and two lines use the same indentation level if and only if they begin with syntactically parallel elements.

## 4.6. Whitespace
A single blank line appears:

### 4.6.1. Vertical whitespace
* Between consecutive methods in a class or object literal
  * Exception: A blank line between two consecutive properties definitions in an object literal (with no other code between them) is optional. Such blank lines are used as needed to create logical groupings of fields.
* Within method bodies, sparingly to create logical groupings of statements. Blank lines at the start or end of a function body are not allowed.
* Optionally before the first or after the last method in a class or object literal.
* As required by other sections of this document (Add more context).

Multiple consecutive blank lines are permitted (neither encouraged nor discouraged)

### 4.6.2. Horizontal whitespace
Use of horizontal whitespace depends on location, and falls into three broad categories: _leading_ (at the start of a line), _trailing_ (at the end of a line), and _internal_. Leading whitespace (i.e., indentation) is addressed [here](add link). Trailing whitespace is forbidden.

Beyond where required by the language or other style rules, and apart from literals, comments, and JSDoc, a single internal ASCII space also appears in the following places only.

1. Separating any reserved word (such as `if`, `for`, or `catch`) except for `function` and `super`, from an open parenthesis (`(`) that follows it on that line.
```
if (condition) { // Space separating open parenthesis and reserved word
    // ..
}
```
2. Separating any reserved word (such as `else` or `catch`) from a closing curly brace (`}`) that precedes it on that line.
```
try {
    // ..
} catch (error) { // Space separating closing curly brace and reserved word
    // ..
}
```
3. Before any open curly brace (`{`), with two exceptions:
    * Before an object literal that is the first argument of a function or the first element in an array literal (e.g. `foo({a: [{c: d}]})`).
    * In a template expansion, as it is forbidden by the language (e.g. valid: `ab${1 + 2}cd`, invalid: `xy$ {3}z`).
4. On both sides of any binary or ternary operator.
```
let result = 10 + 5; // Binary operator '+' surrounded by spaces
let isTrue = (x > 5) ? true : false; // Ternary operator '?' and ':' surrounded by spaces
```
5. After the colon (`:`) in an object literal.
```
let person = {
    firstName: "John", // Space after the colon
};
```
6. After a comma (`,`) or semicolon (`;`). Note that spaces are never allowed before these characters.
7. On both sides of the double slash (`//`) that begins an end-of-line comment. Here, multiple spaces are allowed, but not required. (e.g. `let x = 10;  // This is an end-of-line comment with spaces on both sides
8. After an open-block comment character and on both sides of close characters (e.g. for short-form type declarations, casts, and parameter name comments:
	```
	//Type Declaration Comment:
	this.foo = /** @type {number} */ (bar);
	
	//Function Parameter Name Comment:
	function example(/** string */ foo) {}

	//Parameter Comment Within a Function Call:
	baz(/* buzz= */ true);
	```
### 4.6.3. Horizontal alignment: discouraged
_Horizontal alignment_ is the practice of adding a variable number of additional spaces in your code with the goal of making certain tokens appear directly below certain other tokens on previous lines. This practice is permitted, but it is generally discouraged.

Here is an example without alignment, followed by one with alignment. Both are allowed, but the latter is discouraged:
```
// without alignment
{
  tiny: 42, // this is great
  longer: 435, // this too
};
// with alignment
{
  tiny:   42, // this is discouraged
  longer: 435,
};
```
### 4.6.4. Function arguments
Prefer to put all function arguments on the same line as the function name. If doing so would exceed the 80-column limit, the arguments must be line-wrapped in a readable way. To save space, you may wrap as close to 80 as possible, or put each argument on its own line to enhance readability. Indentation should be four spaces. Aligning to the parenthesis is allowed, but discouraged.

Below are the most common patterns for argument wrapping:
```
// Arguments start on a new line, indented four spaces. Preferred when the
// arguments don't fit on the same line with the function name (or the keyword
// "function") but fit entirely on the second line. Works with very long
// function names, survives renaming without reindenting, low on space.
doSomething(
    descriptiveArgumentOne, descriptiveArgumentTwo, descriptiveArgumentThree) {
    // ..
}

// If the argument list is longer, wrap at 80. Uses less vertical space,
// but violates the rectangle rule and is thus not recommended.
doSomething(veryDescriptiveArgumentNumberOne, veryDescriptiveArgumentTwo,
    tableModelEventHandlerProxy, artichokeDescriptorAdapterIterator) {
    // ..
}

// Four-space, one argument per line.  Works with long function names,
// survives renaming, and emphasizes each argument.
doSomething(
    veryDescriptiveArgumentNumberOne,
    veryDescriptiveArgumentTwo,
    tableModelEventHandlerProxy,
    artichokeDescriptorAdapterIterator) {
    // ..
}
```

## 4.7. Grouping parentheses: recommended
Grouping parentheses should be used when the author and reviewer agree that including them makes the code clearer and less prone to misinterpretation. It's not reasonable to assume that every reader of the code has the entire operator precedence table memorized. Therefore, using parentheses for clarity is encouraged.

Example
```
let result = (a + b) * c; // Grouping parentheses used for clarity
```

Do not use unnecessary parentheses around the entire expression following `delete`, `typeof`, `void`, `return`, `throw`, `case`, `in`, `of`, or `yield`. These keywords already have well-defined behavior.

Example:
```
if (typeof (value) === 'string') { // Avoid unnecessary parentheses with typeof
  // ..
}

function throwError() {
  throw (new Error('An error occurred')); // Avoid unnecessary parentheses with throw
}
```

Parentheses are required when performing type casts using JSDoc annotations.

Example:
```
/** @type {!Foo} */ (foo)
```

## 4.8. Chained method calls
When a chain of method calls is too long to fit on one line, there must be one call per line, with the first call on a separate line from the object the methods are called on. If the method changes the context, an extra level of indentation must be used.

Example:
```
elements
    .addClass('foo')
    .children()
    .html('hello')
    .end()
    .appendTo('body');
```

# 5. Language features
## 5.1. Local variable declarations
### 5.1.1. Use `const` and `let`
Declare all local variables with either `const` or `let`. Use const by default, unless a variable needs to be reassigned. The `var` keyword must not be used.

### 5.1.2. One variable per declaration
Every local variable declaration declares only one variable per declaration statement. Declaring multiple variables in a single declaration statement are not used.

Unlike `var`, it is not necessary to declare all variables at the top of a function. Instead, they are to be declared at the point at which they are first used.

Example:
```
// Preferred: Declare each variable separately
let a = 1;
let b = 2;

// Disallowed: Avoid declaring multiple variables in a single statement
let a = 1, b = 2;
```

### 5.1.3. Declared when needed, initialized as soon as possible
Local variables should not be routinely declared at the beginning of their containing block or block-like structure. Instead, they should be declared as close as possible to the point in the code where they are first used (within reasonable limits). This practice is recommended to minimize the scope of local variables, making the code more efficient and easier to understand.

### 5.1.4. Declare types as needed
(add more context)

## 5.2. Array literals
### 5.2.1. Use trailing commas
Include a trailing comma whenever there is a line break between the final element and the closing bracket. This practice helps improve code maintainability by making it easier to add or remove elements in the future without worrying about the last item's comma.

Example:
```
const values = [
    'first value',
    'second value', // comma is added
];
```

### 5.2.2. Avoid `Array` constructor
The constructor is error-prone if arguments are added or removed. Use a literal instead.

Disallowed:
```
const a1 = new Array(x1, x2, x3);
const a2 = new Array(x1, x2);
const a3 = new Array(x1);
const a4 = new Array();
```
This works as expected except for the third case: if x1 is a whole number then a3 is an array of size x1 where all elements are undefined. If x1 is any other number, then an exception will be thrown, and if it is anything else then it will be a single-element array.

Preferred:
```
const a1 = [x1, x2, x3];
const a2 = [x1, x2];
const a3 = [x1];
const a4 = [];
```

Note: Explicitly allocating an array of a given length using `new Array(length)` is allowed when appropriate.

### 5.2.3. Non-numeric properties
Do not define or use non-numeric properties on an array (other than `length`). Use a `Map` (or `Object`) instead.

Example:
```
// Not Recommended: Using an array for non-numeric properties
const person = [];
person['name'] = 'John';
person['age'] = 30;

// Recommended: Using an object for non-numeric properties
const person = {
    name: 'John',
    age: 30,
};
```

### 5.2.4. Destructuring
_Destructuring_ is unpacking multiple values from a single array or iterable. Array literals may be used on the left-hand side of an assignment to perform destructuring. The "rest" element, denoted by `...rest`, can be used to capture any remaining elements into a new array. There should be no space between the `...` and the variable name.

Example:
```
// The values returned by `generateResults()` are destructured into variables a, b, and c,
// and any remaining values are collected into the rest array.
const [a, b, c, ...rest] = generateResults();

// Specific elements from someArray are extracted into variables b and d, 
// while the commas indicate that other elements are skipped.
let [, b, , d] = someArray;
```

Destructuring can also be used for function parameters. Even if a parameter name is required for syntactical reasons, it can be ignored if not needed.  Always specify `[]` as the default value if a destructured array parameter is optional, and provide default values on the left hand side:

Example:
```
/** @param {!Array<number>=} param1 */
function optionalDestructuring([a = 4, b = 2] = []) {}
```

Disallowed:
```
// Assigns default values directly within the parameter list of a function
function badDestructuring([a, b] = [4, 2]) {};
```

_Tip: For (un)packing multiple values into a function’s parameter or return, prefer object destructuring over array destructuring when possible, as it allows naming the individual elements and specifying a different type for each._

Preferred:
```
// Function that returns an object with named properties
function getUserData() {
  return {
    firstName: 'John',
    lastName: 'Doe',
    age: 30,
  };
}

// Destructuring the returned object into named variables with specified types
const { firstName, lastName, age } = getUserData();
```

Discouraged:
```
// Function that returns an array with multiple values
function getUserData() {
  return ['John', 'Doe', 30];
}

// Destructuring the returned array into variables without named properties
const [firstName, lastName, age] = getUserData();
```

### 5.2.5. Spread operator
Array literals may include the spread operator (`...`) to flatten elements out of one or more other iterables. The spread operator should be used instead of more awkward constructs (methods like `concat`, `push`) with `Array.prototype`. There is no space after the `...` operator.

Preferred: Spread Operator
```
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];
const combinedArray = [...arr1, ...arr2]; // Use spread operator to combine arrays
```

Discouraged: `Array.prototype` constructs
```
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];
const combinedArray = arr1.concat(arr2);
```

## 5.3. Object literals
### 5.3.1. Use trailing commas
[See array literal](Add link)

### 5.3.2. Avoid `Object` constructor
While `Object` does not have the same problems as `Array`, it is still disallowed for consistency. Use an object literal (`{}` or `{a: 0, b: 1, c: 2}`) instead.


### 5.3.3. Avoid mix quoted and unquoted keys
Object literals may represent either structs (with unquoted keys and/or symbols) or dicts (with quoted and/or computed keys). Do not mix these key types in a single object literal.

Disallowed:
```
{
    width: 42, // struct-style unquoted key
    'maxWidth': 43, // dict-style quoted key
}
```

This also extends to passing the property name to functions, like `hasOwnProperty`.

Disallowed:
```
if (o.hasOwnProperty('maxWidth')) {
    // ..
}
```
If your code goes through a process like code compilation or minification, the compiler may change variable and function names to make the code more efficient. When this happens, it might rename variables and functions, but it won't rename string literals like `'maxWidth'`. So, if your code relies on the literal string `'maxWidth'`, it can break after compilation because the string remains unchanged.

To avoid this issue, it's recommended to check the property directly without using a string literal:
```
if (o.hasOwnProperty(maxWidth)) {
    // ..
}
```

### 5.3.4. Computed property names

Computed property names (e.g., `{['key' + foo()]: 42}`) are allowed, and are considered dict-style (quoted) keys (i.e., must not be mixed with non-quoted keys) unless the computed property is a [symbol](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol) (e.g., `[Symbol.iterator]`). Enum values may also be used for computed keys, but should not be mixed with non-enum keys in the same literal.

Example: using computed property names:
```
const dynamicKey = 'key';
const obj = {
    [dynamicKey + '1']: 42, // Computed property name, quoted key
};
```

Example: using symbol as a computed Key:
```
const symbolKey = Symbol('mySymbol');
const obj = {
    [symbolKey]: 'value', // Computed property name, not quoted
    regularKey: 'anotherValue', // Regular (non-computed) key
};
```

Example: using enum values for computed keys:
```
const Enum = {
    OPTION_A: 'a',
    OPTION_B: 'b',
};

const obj = {
    [Enum.OPTION_A]: 42, // Computed property name, quoted key (enum value)
    regularKey: 'value', // Regular (non-computed) key
};
```

### 5.3.5. Method shorthand
Methods can be defined on object literals using the method shorthand (`{method() {..}}`) in place of a colon immediately followed by a `function` or arrow function literal.

Example:
```
return {
    stuff: 'candy',
    foo() {
        return this.stuff;  // Returns 'candy'
    },
};
```

Note: `this` in a method shorthand or `function` refers to the object literal itself whereas `this` in an arrow function refers to the scope outside the object literal.

Example:
```
class {
    getObjectLiteral() {
        this.stuff = 'fruit';
        return {
            stuff: 'candy',
            method: () => this.stuff,  // Returns 'fruit'
        };
    }
}
```

### 5.3.6. Shorthand properties
Shorthand properties are allowed on object literals.

Example:
```
const foo = 1;
const bar = 2;
const obj = {
    foo, // Shorthand property for 'foo'
    bar, // Shorthand property for 'bar'
    method() { return this.foo + this.bar; },
};
console.log(obj.method()); // Output: 3
```

### 5.3.7. Destructuring
1. Usage in Assignments:
 * Object destructuring patterns may be used on the left-hand side of an assignment to perform destructuring and unpack multiple values from a single object.

2. Usage in Function Parameters:
 * Keep parameter destructuring simple: When using object destructuring as function parameters, it's recommended to keep it as simple as possible. This means using a single level of unquoted shorthand properties in the destructuring pattern.
 * Avoid deep nesting and computed properties: Deeper levels of nesting and computed properties should be avoided in parameter destructuring.
 * Specify default values: If you want to provide default values for destructured properties, specify them in the left-hand side of the destructured parameter. (`{str = 'some default'} = {}`, rather than `{str} = {str: 'some default'}`)
 * Optional destructured objects: If a destructured object in a function parameter is optional, it should default to an empty object (`{}`).
 * JSDoc for parameter: The JSDoc for the destructured parameter may be given any name (the name is unused but is required by the compiler).
 
Example:
```
/**
 * A function that uses object destructuring in its parameter.
 *
 * @param {string} ordinary - An ordinary parameter.
 * @param {{name: (string|undefined), age: (number|undefined)}=} param1 - Destructured object parameter.
 *     name: The name property.
 *     age: The age property.
 */
function destructured(ordinary, { num, str = 'Unknown' } = {})
```

Disallowed: deep nesting
```
/** @param {{x: {num: (number|undefined), str: (string|undefined)}}} param1 */
function nestedTooDeeply({ x: { num, str } }) { };
/** @param {{num: (number|undefined), str: (string|undefined)}=} param1 */
function nonShorthandProperty({ num: a, str: b } = {}) { };
/** @param {{a: number, b: number}} param1 */
function computedKey({ a, b, [a + b]: c }) { };
/** @param {{a: number, b: string}=} param1 */
function nontrivialDefault({ a, b } = { a: 2, b: 4 }) { };
``` 
 

### 5.3.8. Enums
Enumerations are defined by adding the `@enum` annotation to an object literal. Additional properties may not be added to an enum after it is defined. Enums must be constant, and all enum values must be deeply immutable.

Example: (Context - check enum variable rules)
```
/**
 * Supported temperature scales.
 * @enum {string}
 */
const TemperatureScale = {
    CELSIUS: 'celsius',
    FAHRENHEIT: 'fahrenheit',
};

/**
 * An enum with two options.
 * @enum {number}
 */
const Option = {
    FIRST_OPTION: 1,
    SECOND_OPTION: 2,
};
```

## 5.4. Classes
### 5.4.1. Constructors
Constructors are not mandatory, but when you create subclasses, you must call `super()` in the subclass constructor before manipulating any fields or accessing `this`. Additionally, non-method properties specified in interfaces should be declared within the constructor.

### 5.4.2. Fields
Set all of a concrete object’s fields (i.e. all properties other than methods) in the constructor. Annotate fields that are never reassigned with `@const` (these need not be deeply immutable). Annotate non-public fields with the proper visibility annotation (`@private`, `@protected`, `@package`), and end all `@private` fields' names leading with an underscore. Fields are never set on a concrete class' `prototype`.

Example:
```
class Foo {
    constructor() {
        /** @private @const {!Bar} */
        this._bar = computeBar();

        /** @protected @const {!Baz} */
        this.baz = computeBaz();
    }
}
```

Disallowed:
```
class Person {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }
}

// Adding a property directly to the Person class's prototype
Person.prototype.gender = 'Unknown';

const person1 = new Person("Alice", 30);
const person2 = new Person("Bob", 25);

console.log(person1.gender); // Output: Unknown
console.log(person2.gender); // Output: Unknown
```

_Tip: Properties should never be added to or removed from an instance after the constructor is finished, since it significantly hinders VMs’ ability to optimize. If necessary, fields that are initialized later should be explicitly set to undefined in the constructor to prevent later shape changes. Adding @struct to an object will check that undeclared properties are not added/accessed. Classes have this added by default._

Discouraged:
```
class Person {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }

    // Method to update the person's address
    updateAddress(newAddress) {
        this.address = newAddress; // Adding a new property dynamically
    }
}

const person1 = new Person("Alice", 30);
console.log(person1); // Output: Person { name: 'Alice', age: 30 }

person1.updateAddress("8 The Green Suite");
console.log(person1); // Output: Person { name: 'Alice', age: 30, address: '8 The Green Suite' }
```

Preferred:
```
class Person {
    constructor(name, age) {
        this.name = name;
        this.age = age;
        this.address = undefined; // Initialize address to undefined
    }

    // Method to update the person's address
    updateAddress(newAddress) {
        this.address = newAddress; // Now it's not a shape change
    }
}

const person1 = new Person("Alice", 30);
console.log(person1); // Output: Person { name: 'Alice', age: 30, address: undefined }

person1.updateAddress("123 Main St");
console.log(person1); // Output: Person { name: 'Alice', age: 30, address: '123 Main St' }
```

### 5.4.3. Computed properties

Computed properties may only be used in classes when the property is a symbol. Dict-style properties (that is, quoted or computed non-symbol keys, as defined in [Do not mix quoted and unquoted keys](add link)) are not allowed. A `[Symbol.iterator]` method should be defined for any classes that are logically iterable. Beyond this, `Symbol` should be used sparingly.

_Tip: be careful of using any other built-in symbols (e.g., `Symbol.isConcatSpreadable`) as they are not polyfilled by the compiler and will therefore not work in older browsers._

Disallowed:
```
class Base {
    /** @nocollapse */
    static foo() { }
}

class Sub extends Base { }

function callFoo(cls) {
    cls.foo(); // discouraged: don't call static methods dynamically
}

Sub.foo(); // Disallowed: don't call static methods on subclasses that don't define it themselves
```

### 5.4.4. Avoid manipulating `prototype`s directly
The class keyword allows clearer and more readable class definitions than defining `prototype` properties.

Exception: Framework code (such as Polymer, or Angular) may need to use `prototype`s.

### 5.4.5. Overriding toString
The toString method may be overridden, but must always succeed and never have visible side effects.

_Tip: Beware, in particular, of calling other methods from toString, since exceptional conditions could lead to infinite loops._

### 5.4.6. Interfaces
Interfaces may be declared with `@interface` or `@record`. Interfaces declared with `@record` can be explicitly (i.e. via `@implements`) or implicitly implemented by a class or object literal.

All non-static method bodies on an interface must be empty blocks. Fields must be declared as uninitialized members in the class constructor.

Example:
```
/** @record */
class Person {
    /**
     * An interface can also declare fields, which participating classes must
     * declare and initialize in their constructors.
     */
    constructor() {
        this.name;
        this.age;
    }

    /**
     * Method signature with an empty body.
     */
    sayHello() { }
}
```

### 5.4.7. Abstract Classes

Use abstract classes when appropriate. Abstract classes and methods must be annotated with `@abstract`.

## 5.5. Functions
### 5.5.1. Top-level functions
Top-level functions may be defined directly on the `exports` object, or else declared locally and optionally exported.

Examples:
```
/** @param {string} str */
exports.processString = (str) => {
    // Process the string.
};
```
```
/** @param {string} str */
const processString = (str) => {
    // Process the string.
};

exports = { processString };
```

### 5.5.2. Nested functions and closures
Functions may contain nested function definitions. If it is useful to give the function a name, it should be assigned to a local `const`.  Prefer arrow functions over the `function` keyword, particularly for nested functions

### 5.5.3. Arrow functions
Arrow functions provide a concise function syntax and simplify scoping `this` for nested functions. Prefer arrow functions over the `function keyword`, particularly for nested functions.

Prefer arrow functions over other `this` scoping approaches such as `f.bind(this)`, `f.bind(f, this)`, and `const self = this`

When you use arrow functions, you can control exactly which parameters are passed to a callback function. This is because arrow functions do not have their own `this` context; they inherit the `this` value from their enclosing function or context. In contrast, other scoping approaches like `bind` or creating a variable to store `this` (like `self`) may pass along unintended parameters or require additional steps to manage parameter passing.

The left-hand side of the arrow contains zero or more parameters. Parentheses around the parameters are optional if there is only a single non-destructured parameter. When parentheses are used, inline parameter types may be specified (see [Method and function comments](add link)).

_Tip: Always using parentheses even for single-parameter arrow functions can avoid situations where adding parameters, but forgetting to add parentheses, may result in parseable code which no longer works as intended._

The right-hand side of the arrow contains the body of the function. By default the body is a block statement (zero or more statements surrounded by curly braces). The body may also be an implicitly returned single expression if either: the program logic requires returning a value, or the `void` operator precedes a single function or method call (using `void` ensures `undefined` is returned, prevents leaking values, and communicates intent). The single expression form is preferred if it improves readability (e.g., for short or simple expressions).

```
/**
 * Arrow functions can be documented just like normal functions.
 * @param {number} numParam A number to add.
 * @param {string} strParam Another number to add that happens to be a string.
 * @return {number} The sum of the two parameters.
 */
const moduleLocalFunc = (numParam, strParam) => numParam + Number(strParam);

// Uses the single expression syntax with `void` because the program logic does
// not require returning a value.
getValue((result) => void alert(`Got ${result}`));

class CallbackExample {
    constructor() {
        /** @private {number} */
        this._cachedValue = 0;

        // For inline callbacks, you can use inline typing for parameters.
        // Uses a block statement because the value of the single expression should
        // not be returned and the expression is not a single function call.
        getNullableValue((/** ?number */ result) => {
            this._cachedValue = result == null ? 0 : result;
        });
    }
}
```

Disallowed:
```
/**
 * A function with no params and no returned value.
 * This single expression body usage is illegal because the program logic does
 * not require returning a value and we're missing the `void` operator.
 */
const moduleLocalFunc = () => anotherFunction();
```

### 5.5.4. Generators
Generators enable a number of useful abstractions and may be used as needed.

When defining generator functions, attach the `*` to the `function` keyword when present, and separate it with a space from the name of the function. When using delegating yields, attach the `*` to the `yield` keyword.

Example:
```
/** @return {!Iterator<number>} */
function* gen1() {
    yield 42;
}

/** @return {!Iterator<number>} */
const gen2 = function* () {
    yield* gen1();
}

class SomeClass {
    /** @return {!Iterator<number>} */
    * gen() {
        yield 42;
    }
}
```

### 5.5.5. Parameter and return types
Function parameters and return types should usually be documented with JSDoc annotations. See [Method and function comments for more information](add link)

#### 5.5.5.1. Default parameters

Parameters with default values go at the end of the parameter list. Default parameters are permitted using the equals operator in the parameter list. Optional parameters must include spaces on both sides of the equals operator, be named exactly like required parameters (i.e., not prefixed with `opt_`), use the `=` suffix in their JSDoc type, come after required parameters, and not use initializers that produce observable side effects. All optional parameters for concrete functions must have default values, even if that value is `undefined`. In contrast to concrete functions, abstract and interface methods must omit default parameter values.

Example:
```
/**
 * @param {string} required This parameter is always needed.
 * @param {string=} optional This parameter can be omitted.
 * @param {!Node=} node Another optional parameter.
 */
function maybeDoSomething(required, optional = '', node = undefined) { }
```

Note: Use default parameters sparingly. Prefer [Destructuring](add link) to create readable APIs when there are more than a small handful of optional parameters that do not have a natural order.

#### 5.5.5.2. Rest parameters

Use a rest parameter instead of accessing `arguments`. Rest parameters are typed with a `...` prefix in their JSDoc. The rest parameter must be the last parameter in the list. There is no space between the `...` and the parameter name. Do not name the rest parameter `var_args`. Never name a local variable or parameter `arguments`, which confusingly shadows the built-in name.

Example:
```
/**
 * @param {!Array<string>} array This is an ordinary parameter.
 * @param {...number} numbers The remainder of arguments are all numbers.
 */
function variadic(array, ...numbers) { }
```

### 5.5.6. Generics
Declare generic functions and methods when necessary with `@template TYPE` in the JSDoc above the function or method definition.

### 5.5.7. Spread operator
Function calls may use the spread operator (`...`). Prefer the spread operator to `Function.prototype.apply` when an array or iterable is unpacked into multiple parameters of a variadic function. There is no space after the `...`.

Example:
```
function myFunction(...elements) { }
myFunction(...array, ...iterable, ...generator());
```

## 5.6. String literals
### 5.6.1. Use double quotes
Ordinary string literals are delimited with double quotes (`"`), rather than single quotes (`'`). Ordinary string literals don't use in multiple lines.

### 5.6.2. Template literals
Use template literals (delimited with `) over complex string concatenation, particularly if multiple string literals are involved. Template literals may span multiple lines. If a template literal spans multiple lines, it does not need to follow the indentation of the enclosing block, though it may if the added whitespace does not matter.

Example:
```
function arithmetic(a, b) {
    return `Here is a table of arithmetic operations:
  ${a} + ${b} = ${a + b}
  ${a} - ${b} = ${a - b}
  ${a} * ${b} = ${a * b}
  ${a} / ${b} = ${a / b}`;
}
```

### 5.6.3. No line continuations
Do not use line continuations (that is, ending a line inside a string literal with a backslash) in either ordinary or template string literals. Even though ES5 allows this, it can lead to tricky errors if any trailing whitespace comes after the slash, and is less obvious to readers.

Disallowed:
```
const longString = "This is a very long string that far exceeds the 80 \
    column limit. It unfortunately contains long stretches of spaces due \
    to how the continued lines are indented.";
```

Preferred:
```
const longString = "This is a very long string that far exceeds the 80 " +
    "column limit. It does not contain long stretches of spaces since " +
    "the concatenated strings are cleaner.";
```

### 5.6.4. Number literals
Numbers may be specified in decimal, hex, octal, or binary. Use exactly `0x`, `0o`, and `0b` prefixes, with lowercase letters, for hex, octal, and binary, respectively. Never include a leading zero unless it is immediately followed by `x`, `o`, or `b`.

## 5.7. Control structures
### 5.7.1. For loops
With ES6, the language now has three different kinds of `for` loops. All may be used, though `for`-`of` loops should be preferred when possible. `for`-`in` loops may only be used on dict-style objects (see [Do not mix quoted and unquoted keys](add link)), and should not be used to iterate over an array. `Object.prototype.hasOwnProperty` should be used in `for`-`in` loops to exclude unwanted prototype properties. Prefer `for`-`of` and `Object.keys` over `for`-`in` when possible.

### 5.7.2. Exceptions
Exceptions are an important part of the language and should be used whenever exceptional cases occur. Always throw Errors or subclasses of Error (`TypeError` or `CustomError`): never throw string literals or other objects. Always use new when constructing an Error. This treatment extends to `Promise` rejection values as `Promise.reject(obj)` is equivalent to `throw obj` in async functions.
Example
```
throw new Error('This is an error message.'); // Correct (throwing Error class)
throw 'This is a string, not an Error.'; // Incorrect (throwing a string literal)
```

Custom exceptions provide a great way to convey additional error information from functions. They should be defined and used wherever the native Error type is insufficient.

Example:
```
class CustomError extends Error {
    constructor(message) {
        super(message);
        this.name = 'CustomError';
    }
}

// Usage:
throw new CustomError('This is a custom error message.');
```

Prefer throwing exceptions over ad-hoc error-handling approaches (such as passing an error container reference type, or returning an object with an error property).

Example:
```
function divide(a, b) {
    if (b === 0) {
        // Correct (throwing an exception)
        throw new Error('Division by zero is not allowed.');
    }
    return a / b;
}

function divide(a, b) {
    if (b === 0) {
        // Incorrect (using ad-hoc error-handling with an object):
        return { error: 'Division by zero is not allowed.' };
    }
    return a / b;
}
```

#### 5.7.2.1. Empty catch blocks

In situations where it is justified to take no action in a catch block when catching an exception, provide a comment explaining the rationale for this omission.

Example:
```
try {
    return handleNumericResponse(response);
} catch (ok) {
    // it's not numeric; that's fine, just continue
}
return handleTextResponse(response);
```

Disallowed:
```
try {
    shouldFail();
    fail('expected an error');
} catch (expected) { }
```

### 5.7.3. Switch statements
Inside the braces of a switch block are one or more statement groups. Each statement group consists of one or more switch labels (either `case FOO:` or `default:`), followed by one or more statements.

#### 5.7.3.1. Fall-through: commented

Within a switch block, each statement group either terminates abruptly (with a `break`, `return` or `thrown` exception), or is marked with a comment to indicate that execution will or might continue into the next statement group. Any comment that communicates the idea of fall-through is sufficient (typically `// fall through`). This special comment is not required in the last statement group of the switch block.

Example
```
const dayOfWeek = 3;

switch (dayOfWeek) {
  case 1:
    console.log('It\'s Monday.');
    break;
  case 2:
    console.log('It\'s Tuesday.');
    break;
  case 3:
    console.log('It\'s Wednesday.');
    // No 'break' here, so it falls through to the next case
  case 4:
    console.log('It\'s Thursday.');
    break;
  case 5:
    console.log('It\'s Friday.');
    break;
  default:
    console.log('It\'s the weekend.');
}

// Outputs:
// It's Wednesday.
// It's Thursday.
```

#### 5.7.3.2. Mandatory `default` Case

Each switch statement includes a `default` statement group, even if it contains no code. The `default` statement group must be last.

### 5.7.4. `this` keyword
Only use `this` in class constructors and methods, in arrow functions defined within class constructors and methods, or in functions that have an explicit `@this` declared in the immediately-enclosing function’s JSDoc.

Avoid using this to reference the global object, the execution context of an `eval`, the event target, or in cases where `call()` or `apply()` functions are used without necessity.

## 5.8. Equality checks
Using the identity operators (`===` and `!==`) is generally preferable in coding practices. These operators perform comparison without type coercion, ensuring that both value and type match for equality.

On the other hand, the loose equality operators (`==` and `!=`) perform type coercion, attempting to convert operands to a common type before comparison. It's essential to avoid using the loose equality operators when comparing values against falsy values, as they may lead to unexpected results due to type coercion.

### 5.8.1. Coercion for handling `null` and `undefined`
Catching both `null` and `undefined` values:

Example:
```
if (someObjectOrPrimitive == null) {
    // Checking for null catches both null and undefined for objects and
    // primitives, but does not catch other falsy values like 0 or the empty
    // string.
}
```

### 5.8.2. Type Checks
Avoid comparing variables directly to `true` or `false` using `x == true` or `x == false`. Instead, use the variables themselves, like (`x`) or (`!x`), for conditional checks. This makes the code more explicit and easier to understand. For non-boolean values like for objects compare them to `null`, for numbers compare to `0`, and for strings compare to an empty string `""`. This helps avoid ambiguity and ensures clarity in your comparisons.

These are the preferred ways of checking the type of an object:

```
String: typeof object === 'string'
Number: typeof object === 'number'
Boolean: typeof object === 'boolean'
Object: typeof object === 'object' or _.isObject( object )
Plain Object: jQuery.isPlainObject( object )
Function: _.isFunction( object ) or jQuery.isFunction( object )
Array: _.isArray( object ) or jQuery.isArray( object )
Element: object.nodeType or _.isElement( object )
null: object === null
null or undefined: object == null
undefined:
	Global Variables: typeof variable === 'undefined'
	Local Variables: variable === undefined
	Properties: object.prop === undefined
	Any of the above: _.isUndefined( object )

```

## 5.9. Disallowed features
### 5.9.1. Avoid `with` keyword
Do not use the `with` keyword. It makes your code harder to understand and has been banned in strict mode since ES5.

### 5.9.2. `eval` is Evil
Avoid dynamic code evaluation using `eval` or the `Function(...string)` constructor except in cases where they are required for specific, validated use cases.

Example: using `eval`
```
const x = 10;
const y = 20;
const operation = 'x + y';
const result = eval(operation); // Output: 30
```

Example: using `Function` constructor:
```
const x = 10;
const y = 20;
const operation = 'x + y';
const addFunction = new Function('x', 'y', 'return ' + operation);
const result = addFunction(x, y); // Output: 30
```

It's essential to understand the associated risks:
* Security Risks: Dynamic code evaluation can introduce security vulnerabilities, as it allows arbitrary code execution. If you're not careful, malicious code injection could occur.
* Performance: Code evaluated with eval or Function is not optimized during compilation, potentially leading to slower execution.
* Debugging and Maintainability: Code evaluated dynamically can be challenging to debug, as it's not visible in the source code and doesn't benefit from tools like linters or code analysis.
* Compatibility: In certain environments (e.g., Content Security Policy - CSP), the use of eval or the Function constructor may be restricted or disallowed entirely.

### 5.9.3. Automatic semicolon insertion
Always terminate statements with semicolons (except function and class declarations, as noted above).

### 5.9.4. Non-standard features
Do not use non-standard features. This includes old features that have been removed (e.g., `WeakMap.clear`), new features that are not yet standardized (e.g., the current TC39 working draft, proposals at any stage, or proposed but not-yet-complete web standards), or proprietary features that are only implemented in some browsers. Use only features defined in the current ECMA-262 or WHATWG standards. (Note that projects writing against specific APIs, such as Chrome extensions or Node.js, can obviously use those APIs). Non-standard language “extensions” (such as those provided by some external transpilers) are forbidden.

### 5.9.5. Wrapper objects for primitive types
Never use `new` on the primitive object wrappers (`Boolean`, `Number`, `String`, `Symbol`), nor include them in type annotations. The wrappers may be called as functions for coercing (which is preferred over using `+` or concatenating the empty string) or creating symbols.

Example: using wrapper functions for type coercion
```
const num1 = Number('42'); // Converts the string '42' to a number
const bool1 = Boolean('true'); // Converts the string 'true' to a boolean
const str1 = String(123); // Converts the number 123 to a string
```

Disallowed: using the `+` operator and concatenating an empty string for type coercion
```
const num2 = +'42'; // Converts the string '42' to a number
const bool2 = !!'true'; // Converts the string 'true' to a boolean
const str2 = '' + 123; // Converts the number 123 to a string
```

### 5.9.6. Modifying builtin objects
Never modify builtin types, either by adding methods to their constructors or to their prototypes. Avoid depending on libraries that do this. Note that the JSCompiler’s runtime library will provide standards-compliant polyfills where possible; nothing else may modify builtin objects.

Do not add symbols to the global object unless absolutely necessary (e.g. required by a third-party API).

### 5.9.7. Omitting `()` when invoking a constructor
Never invoke a constructor in a new statement without using parentheses ().

Example:
```
new Foo; // Disallowed
new Foo(); // Preferred
```

### 5.9.8. Comma(`,`) Operator
Avoid the use of the comma operator except for very disciplined use in the control part of for statements. (This does not apply to the comma separator, which is used in object literals, array literals, var statements, and parameter lists.)

Example:
```
for (let i = 0, j = 10; i < 5; i++, j--) {
    // This loop increments 'i' and decrements 'j' in each iteration.
    console.log(`i: ${i}, j: ${j}`);
}
```

### 5.9.9. Avoid confusing uses of plus, minus operator
Avoid consecutive plus signs `+ or a plus sign followed by the increment operator `++`, as it can lead to confusion. To clarify your intention, use parentheses to separate them.

Discouraged:
```
total = subtotal + +myInput.value;
```

Preferred:
```
total = subtotal + (+myInput.value);
```

# 6. Naming
## 6.1. Rules common to all identifiers

Identifiers use only ASCII letters (A .. Z, a .. z) and digits (0 .. 9), and underscores (_). Offer a comprehensive and descriptive name, prioritizing code clarity over space efficiency. Avoid uncommon abbreviations and ensure full word representation for clear understanding.

Note: Avoid using leading underscore, it is used for private members.

Example:
```
errorCount: Use a clear and descriptive name without abbreviations.
dnsConnectionIndex: Use widely understood abbreviations, like 'DNS' in this case.
```

Disallowed:
```
n                   // Meaningless.
nErr                // Ambiguous abbreviation.
wgcConnections      // Only your group knows what this stands for.
cstmrId             // Deletes internal letters.
```

## 6.2. Rules by identifier type
### 6.2.1. Package names
Package names are all `lowerCamelCase`. For example, `my.exampleCode.deepSpace`, but not `my.examplecode.deepspace` or `my.example_code.deep_space`.

### 6.2.2. Class names
Class, interface, record, and typedef names are written in `PascalCase`. Type names are typically nouns or noun phrases. For example, `Request`, `ImmutableList`, or `VisibilityMode`. Additionally, interface names may sometimes be adjectives or adjective phrases instead (for example, `Readable`).

### 6.2.3. Method names
Method names are written in `lowerCamelCase`. Names for `@private` methods must start with a leading underscore. Method names are typically verbs or verb phrases. For example, `sendMessage` or `_stop`. Getter and setter methods for properties are never required, but if they are used they should be named getFoo (or optionally isFoo or hasFoo for booleans), or setFoo(value) for setters.

Underscores may also appear in JsUnit test method names to separate logical components of the name. One typical pattern is `test<MethodUnderTest>_<state>_<expectedOutcome>`, for example `testPop_emptyStack_throws`. There is no One Correct Way to name test methods.

### 6.2.4. Enum names
Enum names are written in `PascalCase`, similar to classes, and should generally be singular nouns. Individual items within the enum are named in `CONSTANT_CASE`.

### 6.2.5. Constant names
Constant names use `CONSTANT_CASE`. Constants names are typically nouns or noun phrases. All uppercase letters, with words separated by underscores. There is no reason for a constant to be named with a leading underscore, since private static properties can be replaced by (implicitly private) module locals.

Every constant is a `@const` static property or a module-local `const` declaration, but not all `@const` static properties and module-local `const`s are constants. However, not all variables labeled as `@const` static properties or module-local `const`s truly qualify as constants. Before opting for the constant case, carefully consider whether the field genuinely exhibits deep immutability like a classic constant.

For instance, if any observable state within an object instance has the potential to change, it likely shouldn't be treated as a constant. Mere intentions of never altering the object's properties are often insufficient to categorize it as a true constant.

Examples: constants
```
/** @const */ const NUMBER = 5; // constant because it is declared using `const`.
/** @const */ const NAMES = ImmutableList.of('Ed', 'Ann'); // Making immutable explicitly, makes it immutable.
/** @enum */ const SomeEnum = { ENUM_CONSTANT: 'value' }; // enum with constant properties
```

Examples: not constants
```
let letVariable = 'non-const'; // Not a constant; declared with 'let,' can be reassigned

class MyClass {
    constructor() {
        /** @const {string} */
        this.nonStatic = 'non-static'; // Not a constant, can be reassigned
    }
};

/** @type {string} */ MyClass.staticButMutable = 'not @const, can be reassigned';
const /** Set<string> */ mutableCollection = new Set();
const /** ImmutableSet<SomeMutableType> */ mutableElements = ImmutableSet.of(mutable);
const Foo = goog.require('my.Foo');  // mirrors imported name
const logger = log.getLogger('loggers.are.not.immutable');
```

### 6.2.6. Non-constant field names
Non-constant field names (static or otherwise) are written in `lowerCamelCase`, with a leading underscore for private fields. These names are typically nouns or noun phrases. For example, `computedValues` or `_index`.

### 6.2.7. Parameter names
Parameter names are written in lowerCamelCase.

### 6.2.8. Local variable names
Local variable names are written in `lowerCamelCase`, except for module-local (top-level) constants, as described above. Constants in function scopes are still named in `lowerCamelCase`. 

### 6.2.9. Template parameter names
Template parameter names should be concise, single-word or single-letter identifiers, and must be all-caps, such as TYPE or THIS.

Example:
```
// Generic function using a template parameter
function processArray(arr, TYPE) {
    return arr.map((item) => new TYPE(item));
}

class Person {
    constructor(name) {
        this.name = name;
    }
}

const names = ['Alice', 'Bob', 'Charlie'];
const people = processArray(names, Person);
```

## 6.3. Camel case examples

Prose form        | Correct         | Incorrect
----------------- | --------------- | ---------------
XML HTTP request  | XmlHttpRequest  | XMLHTTPRequest
new customer ID   | newCustomerId   | newCustomerID
inner stopwatch   | innerStopwatch  | innerStopWatch

# 7. JSDoc

The Closure Compiler can leverage data type information about JavaScript variables to offer improved optimization, validation and warnings. However, JavaScript lacks built-in syntax for declaring variable types. To overcome this limitation, comments within the code are used to indicate the data type of variables. Therefore, these comments must be well-formed.

The type system used by the Closure Compiler is based on the annotations commonly used with the JSDoc documentation generation tool.

## 7.1. General form
The basic formatting of JSDoc blocks is as seen as below:
Example: multi-line
```
/**
 * Multiple lines of JSDoc text are written here,
 * wrapped normally.
 * @param {number} arg A number to do something to.
 */
function doSomething(arg) { }
```

Example: single line
```
/** @const @private {!Foo} A short bit of JSDoc. */
this._foo = foo;
```

If a single-line comment overflows into multiple lines, it must use the multi-line style with `/**` and `*/` on their own lines.

## 7.2. Markdown
JSDoc is written in Markdown, though it may include HTML when necessary.

Note: Tools that automatically extract JSDoc (e.g. [JsDossier](https://github.com/jleyba/js-dossier)) will often ignore plain text formatting, so if you did this:
```
/**
 * Computes weight based on three factors:
 *   items sent
 *   items received
 *   last timestamp
 */
```
it would come out like this:
```
Computes weight based on three factors: items sent items received last timestamp
```

Instead, write a Markdown list:
```
/**
 * Computes weight based on three factors:
 *
 * - items sent
 * - items received
 * - last timestamp
 */
```

## 7.3. JSDoc tags
Most tags must appear on their own line, with the tag at the beginning of the line. Combining tags on a single line is disallowed.

Disallowed:
```
/**
 * The "param" tag must occupy its own line and may not be combined.
 * @param {number} left @param {number} right
 */
function add(left, right) { }
``` 

Simple tags that do not require any additional data (such as `@private`, `@const`, `@final`, `@export`) may be combined onto the same line, along with an optional type when appropriate.

Example:
```
/**
 * Place more complex annotations (like "implements" and "template")
 * on their own lines.  Multiple simple tags (like "export" and "final")
 * may be combined in one line.
 * @export @final
 * @implements {Iterable<TYPE>}
 * @template TYPE
 */
class MyClass {
    /**
     * @param {!ObjType} obj Some object.
     * @param {number=} num An optional number.
     */
    constructor(obj, num = 42) {
        /** @private @const {!Array<!ObjType|number>} */
        this._data = [obj, num];
    }
}
```

Note: There is no hard rule for when to combine tags, or in which order, but be consistent.

## 7.4. JSDoc tags line wrapping
Line-wrapped block tags are indented four spaces. Wrapped description text may be lined up with the description on previous lines, but this horizontal alignment is discouraged.

Example:
```
/**
 * Illustrates line wrapping for long param/return descriptions.
 * @param {string} foo This is a param with a description too long to fit in
 *     one line.
 * @return {number} This returns something that has a description too long to
 *     fit in one line.
 */
exports.method = function (foo) {
    return 5;
};
```

Note: Do not indent when wrapping a `@desc` or `@fileoverview` description.

##<a name="top/file-level comments"></a>7.5. Top/file-level comments
A file may include a top-level file overview, which may contain copyright notices, author information, and default visibility level (optional). File overviews are generally recommended whenever a file consists of more than a single class definition. The top level comment is designed to orient readers unfamiliar with the code to what is in this file. It may provide a description of the file's contents and any dependencies or compatibility information. Wrapped lines within this comment should not be indented.

Example:
```
/**
 * @fileoverview Description of file, its uses and information
 * about its dependencies.
 * @package
 */
```

## 7.6. Class comments
Classes, interfaces, and records must be documented with a description and any relevant tags such as template parameters, implemented interfaces, visibility, or others as appropriate. The description of a class should provide sufficient information to guide users on how and when to utilize the class, along with any essential considerations for its proper usage. For constructors, textual descriptions may be omitted. 

Example:
```
/**
 * A fancier event target that does cool things.
 * @implements {Iterable<string>}
 */
class MyFancyTarget extends EventTarget {
    /**
     * @param {string} arg1 An argument that makes this more interesting.
     * @param {!Array<number>} arg2 List of numbers to be processed.
     */
    constructor(arg1, arg2) {
        // ..
    }
};
```

## 7.7. Enum and typedef comments
Every enum and typedef must have appropriate JSDoc tags (`@typedef` or `@enum`) on the line just before their declaration. Public enums and typedefs must also include a description. Additionally, individual items within an enum may be documented with a JSDoc comment on the line just before their declaration.

Example:
```
/**
 * A useful type union, which is reused often.
 * @typedef {!Bandersnatch|!BandersnatchType}
 */
let CoolUnionType;

/**
 * Types of bandersnatches.
 * @enum {string}
 */
const BandersnatchType = {
    /** This kind is really frumious. */
    FRUMIOUS: 'frumious',
    /** The less-frumious kind. */
    MANXOME: 'manxome',
};
```

## 7.8. Method and function comments
Method descriptions begin with a verb phrase that describes what the method does. This phrase is not an imperative sentence, but instead is written in the third person, as if it's describing what "This method..." accomplishes.

In methods and named functions, parameter and return types must be documented, except in the case of same-signature overriding method (a method in a subclass that replaces a method in the superclass). It must include an `@override` annotation. Overridden methods inherit all JSDoc annotations from the super class method (including visibility annotations) and they should be omitted in the overridden method. If the overriding method refines or changes the data types specified in the type annotations (such as parameter types or return types), then you should provide explicit `@param` and `@return` annotations to clarify these changes.

Example:
```
/** A class that does something. */
class SomeClass extends SomeBaseClass {
    /**
     * Operates on an instance of MyClass and returns something.
     * @param {!MyClass} obj An object that for some reason needs detailed
     *     explanation that spans multiple lines.
     * @param {!OtherClass} obviousOtherClass
     * @return {boolean} Whether something occurred.
     */
    someMethod(obj, obviousOtherClass) { }

    /** @override */
    overriddenMethod(param) { }
}

/**
 * Demonstrates how top-level functions follow the same rules.  This one
 * makes an array.
 * @param {TYPE} arg
 * @return {!Array<TYPE>}
 * @template TYPE
 */
function makeArray(arg) { }
```

Descriptions for method, parameter, and return values (but not types) can be omitted if they are clear and evident from the surrounding JSDoc or the method's signature.

Example:
```
/**
 * Adds two numbers.
 * @param {number} a
 * @param {number} b
 * @returns {number}
 */
function add(a, b) {
    return a + b;
}
```

The `this` type should be documented when necessary. However, if a function contains no non-empty `return` statements, the return type can be omitted.

If you only need to document the param and return types of a function, you may optionally use inline JSDocs in the function's signature. These inline JSDocs specify the return and param types without tags.

Example:
```
function /** string */ foo(/** number */ arg) { }
```

Disallowed:
```
class MyClass {
    /** @return {string} */ foo() { }

    /** Function description. */ bar() { }
}
```

In anonymous functions annotations are generally optional. If the automatic type inference is insufficient or explicit annotation improves readability, then annotate param and return types like this:
```
promise.then(
    /** @return {string} */
    (/** !Array<string> */ items) => {
        doSomethingWith(items);
        return items[0];
    });
```

For function type expressions, see [Function type expressions](Add link)

## 7.9. Property comments
Property types must be documented. The description may be omitted for private properties, if name and type provide enough documentation for understanding the code.

Publicly exported constants are commented the same way as properties.

Example:
```
/** My class. */
class MyClass {
    /** @param {string=} someString */
    constructor(someString = 'default string') {
        /** @private @const {string} */
        this._someString = someString;

        /** @private @const {!OtherType} */
        this._someOtherThing = functionThatReturnsAThing();

        /**
         * Maximum number of things per pane.
         * @type {number}
         */
        this.someProperty = 4;
    }
}
```

## 7.10. Type annotations
Type annotations are found on `@param`, `@return`, `@this`, and `@type` tags, and optionally on `@const`, `@export`, and any visibility tags. Type annotations attached to JSDoc tags must always be enclosed in braces.

Example:
```
/**
 * Calculates the sum of two numbers.
 * @param {number} x - The first number.
 * @param {number} y - The second number.
 * @return {number} - The sum of x and y.
 */
function calculateSum(x, y) {
    return x + y;
}
```

### 7.10.1. Nullability
Nullability in the type system is defined using modifiers `!` and `?` which indicate non-null and nullable respectively. These modifiers should come before the type. Nullability modifiers have different requirements for different types, which fall into two broad categories:

* For primitives (`string`, `number`, `boolean`, `symbol`, `undefined`, `null`) and literals (`{function(...): ...}` and `{{foo: string...}}`), they are non-nullable by default. Use the `?` modifier to make it nullable, but omit the redundant `!`.
* Reference types, typically anything in `PascalCase` (like `some.namespace.ReferenceType`), refer to classes, enums, records, or typedefs defined elsewhere. These types can be nullable or non-nullable, and it's impossible to determine from their name alone. To avoid ambiguity, always use explicit `?` and `!` modifiers for these types.

Discouraged:
```
const /** !number */ someNum = 5; // Primitives are non-nullable by default.
const /** MyObject */ myObject = null; // Non-primitive types must be annotated.
const /** number? */ someNullableNum = null; // ? should precede the type.
```

Preferred:
```
const /** number */ someNum = 5;
const /** ?MyObject */ myObject = null;
const /** ?number */ someNullableNum = null;

/**
 * Example of nullable and non-nullable types.
 * @param {?string} nullableString - A nullable string.
 * @param {number} nonNullableNumber - A non-nullable number.
 * @param {!MyClass} notNullableClass - An not nullable class.
 * @param {?SomeClass} nullableClass - An nullable class.
 */
function exampleFunction(nullableString, nonNullableNumber, notNullableClass, nullableClass) {
    // ..
}
```

### 7.10.2. Template parameter types
Always provide template parameters. This practice benefits both the compiler's performance and the code's readability.

Discouraged:
```
const /** !Object */ users = {};
const /** !Array */ books = [];
const /** !Promise */ response = ...;
```

Preferred:
```
const /** !Object<string, !User> */ users = {};
const /** !Array<string> */ books = [];
const /** !Promise<!Response> */ response = ...;

const /** !Promise<undefined> */ thisPromiseReturnsNothingButParameterIsStillUseful = ...;
const /** !Object<string, *> */ mapOfEverything = {};
```

### 7.10.3. Function type expressions
_function type expression refers to a type annotation that includes the keyword `function` for defining function types._

When providing a function definition, avoid using function type expressions. Instead, specify parameter and return types using `@param` and `@return` tags or inline annotations (as explained in [Method and function comments](add link)). This guideline applies to anonymous functions and functions that are defined and assigned to constants, where the JSDoc comment for the function should be placed above the entire assignment expression.

Discouraged:
```
// Avoid using a function type expression in function definitions:
/**
 * @param {function(string): number} myFunction - A function that takes a string and returns a number.
 */
const myFunction = function (input) {
    return parseInt(input, 10);
};
```

Preferred:
```
/**
 * This function takes a string and returns a number.
 * @param {string} input - The input string.
 * @return {number} The parsed number.
 */
const myFunction = function (input) {
    return parseInt(input, 10);
};
```

Function type expressions are still necessary in certain contexts, such as within `@typedef`, `@param`, or `@return` tags. They should also be used for variables or properties of function type if they are not immediately initialized with the function definition.

Example:
```
/**
 * A private function that generates an ID based on the input string.
 * @private {function(string): string}
 */
this._idGenerator = utilFunctions.identity;
```

When using a function type expression, it's essential to explicitly define the return type. Otherwise the default return type is "unknown", which leads to strange and unexpected behavior, usually not the intended outcome.

Discouraged:
```
/** @param {function()} operation */
function performOperation(operation) {
    const result = operation();  // No compile-time type error here.
    console.log(result.toUpperCase());  // Potential runtime error.
}

performOperation(() => 42);  // This function returns a number, but the type is not specified.
```

Preferred:
```
/**
 * @param {function(): string} operation A function that returns a string.
 * @param {function(): undefined} calculate A function that doesn’t returns anything.
 */
function performOperation(operation, calculate) {
    const result = operation();  // Type error if operation doesn't return a string.
    console.log(result.toUpperCase());  // Safe to call methods on a string.
    calculate();
}

performOperation(() => 'hello', calculate);
```

# 8. Coding beyond the guidelines
## 8.1. Practices not covered by the guidelines
In situations where specific coding style guidelines are not provided, it is essential to prioritize consistency across the project. A uniform coding style enhances codebase readability and maintainability, benefiting developers working on the project. Here's how to approach such situations:
* Follow existing style: When you encounter a coding style issue or question that isn't explicitly addressed in the project's coding guidelines, start by looking at the code in the same file. If there's a consistent style used in that file, follow that style. This helps maintain a uniform appearance within the file.
* Emulate other files: If the coding style question remains unresolved after checking the current file, broaden your scope to look at how the same issue is handled in other files within the same software package or project. Try to emulate the style used in those files.

## 8.2. Practices not follow the guidelines
You might come across files that do not follow to the established coding guidelines or standard. These files could originate from various sources, such as open source, third-party etc, or they might have been written before guidelines were established. Additionally, there could be instances where a file intentionally deviates from guidelines and standard for specific reasons. In such cases, these files may not adhere to follow coding conventions and standards.

## 8.3. Reformatting existing code
When you're in the process of updating the style of existing code, it's essential to consider the following guidelines:
* Selective refactoring: You don't need to revise all existing code to align perfectly with the current style guidelines. Refactoring existing code should strike a balance between maintaining consistency and avoiding unnecessary code changes. As style rules evolve over time, it's not necessary to retrofit every piece of code to meet the latest standards. However, if you're making substantial alterations to a file, it's expected that the file should adhere to current standard.

## 8.4. Newly added code
When you're introducing new code, it's important to adhere to current guidelines. Here are some key points to remember:
* New files: For brand new files, always use current coding guidelines, regardless of the formatting choices in other files within the same package.
* Reformatting existing code (Recommended): If you're adding new code to a file that doesn't currently adhere to coding guidelines, it's advisable to reformat the existing code to meet the style guidelines. However, this recommendation is subject to the advice given in section [Reformatting existing code](add link).

## 8.5. Local coding rules
Teams and projects may adopt additional style rules beyond what's outlined in this document. However, it's important to recognize that during cleanup changes, these additional rules might not always be followed. Such cleanup changes should not be prevented simply because they don't adhere to these extra rules.

It's crucial to exercise caution when introducing extra rules, ensuring they serve a meaningful purpose. The style guide provided here doesn't aim to cover every conceivable style scenario, and neither should you attempt to do so with local rules.


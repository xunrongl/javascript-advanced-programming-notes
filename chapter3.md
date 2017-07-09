# Chapter 3

## syntax

- Camel-Case naming convention

- `"use strict"`

  - it could be used in the whole file like 

  - ```javascript
    //Immediately-Invoked Function Expression
    (function (){

    　　　　"use strict";
    　　　　// some code here

    　　 })();
    ```

  - or with in a function

  - ```javascript
    function strict() {
      "use strict"
      return "this is strict mode";
    }
    ```

  - strict mode requires:

    - explicity declaration for global vars
    - `with` is not allowed
    - `this` pointing to the global object is not allowed
    - ...

- `{}` code block does not start from the new line

## Variables

- if you miss the `var` and use a variable directly, it will become a `global variable` automatically
- change the type of the var is not recommended

## Data Type

5 Primitive types (undefined, boolean, string, number, function) and `Object` type

- `typeof`

  - a operator not a function, so `()` is not required

- `NULL`

  - `typeof null` will return `object` because null is an empty object
  - Note: `null == undefined` will return `true` while  `null === undefined` will `false`
    - which give us an option to check if a var is `null` or `undefined` using `if(value)`

- `Undefined`

  - declare but not defined var will be `undefined`
  - not declare var will cause error
  - However, `typeof` will return `undefined` for both cases

- `Boolean`

  - all values could be transfered to `Boolean` using `Boolean()`, which is used by `if()` automatically
    - empty string, 0, NaN, null, undefined will be `false`

- `Number`

  - Start with 0 and the rest number is 0~7, octonary
  - Start with 0x and the rest numbers are 0~9 and A~F, hex
  - exponential - `var floatNum = 3.125e7`
  - do not check computation of float numbers e.g `var a = 0.1, b 0.2; a + b == 0.3 // false`
  - `Number.MIN_VALUE` and `Number.MAX_VALUE` are infinity, which is not allowed in the computation
    - `isFinite()`
  - NaN - `value / 0`
    - any operation inlcudes NaN will return NaN
    - `NaN == NaN` returns false, NaN not equal to anything
    - isNaN()
  - Number transfer
    - `Number()`
      - null -> 0
      - undefined -> NaN
      - object -> `valueOf()`, if `NaN`, `toString()`
      - string inlcude any non-numberic -> 0
    - `parseInt(number, base)` - will ignore the space and find the first non-space char to parse until meet any non-numberic char
      - `parseInt("1234blue")  //1234` 
      - `parseInt()` // 0
      - `parseInt("070") // 56` (different behavior for ECMAScript 3 and 5, 5 will be 70)
      - `parseInt("10", 16) // 16`
    - `parseFloat()` only works for decimal, it will ignore the 0s in front of the numbers
      - `parseFloat("3.125e7") // 31250000`
      - `parseFloat("3.0") // 3`

- String

  - Number `toString()` can set the base like 2, 8, 10, 16

  - if you are not sure the value is `null` or `undefine`, you can use `String()` as follow:

  - ```javascript
    var value1 = null;
    var value2;
    alert(String(value1)) // "null"
    alert(String(value2)) // "undefined"
    ```

- Object

## Operator

- ++, - -, +, - single operator

  - for `String`, convert to `Number` then compute. (could be NaN)
  - for `Boolean`, convert to 1 or 0, then compute

- ~ NOT get negative value and minus 1

- & AND

- | OR

- ^ XOR

- <<, >> (signed right shift), >>> (unsigned right shift)

- !, &&, ||

  - &&:  Note: if object && object, it could return object
  - ||: avoid define null and undefined to the var `var myObject = preferredObject || backupObject`

- \*:  infinity \* 0 = NaN, will call `Number()` if the operated number is not a number

- /:  Infinity / Infinity = Infinity

- &: finity % Infinity = finity

- +: Infinity + -Infinity = NaN; +0 + -0 = +0;

  - if any operand is String, it will be string concate. Other operand will be converted to be String using `toString()`

  - ```javascript
    5 + "5" // "55"
    "The sum of 5 and 10 is " + 5 + 10 // "Teh sum of 5 and 10 is 510"
    "The sum of 5 and 10 is " + 15 // "Teh sum of 5 and 10 is 15"
    ```

- -: if any operand is non-numberic, it will be converted to be number using `Number()` or `valueOf()`

- <, >, >=, <=

  - ```javascript
    "Brick" < "alph" //true
    "23" < "3"  //true
    "23" < 3 //false
    ```

- ==/!= convert and then compare

  - ```javascript
    undeinfed == 0 //false (underfined if convertable)
    null == 0 // false (null is not convertable)
    NaN == NaN //false (if there is one operand as NaN, == will return false)
    ```

- ===/!== only compare

## Statement

- for-in can be used to enum the props of object, **but the order is unpredictable**
- switch statement is more flexible, there could be var and even operations in the expression, and it use `===` to compare

## Function

- argus is always an array in the function, so even if you define two args but you can still pass one or three argus

- there is no block scope, so the `i` var in for loop could be accessed outside, pay attention to the `i` after for loop 

- `with` statement is to set the scopt with a specific object like

  - ```javascript
    var qs = location.search.substring(1);
    var hostName = location.hostname;
    var url = location.href;

    //use with statement
    with(location) {
      var qs = search.substring(1);
      var hostName = hostname;
      var url = href;
    }
    ```

  - ​

- you can also use `arguments` to get all the args

  - you can take advantage of `arguments.length` to overload the function
  - arguments[0] and the first arg are always synced 
  - and if not passing args, it will be `undefined`

- no functional overloading

  - if there are two functions with the same name, the latter one wins
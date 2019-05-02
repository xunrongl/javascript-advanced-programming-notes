# 5.Reference Types

## Pass by Reference

Because reference types \(array, object\) are passed by reference, so in the real world, instead of mutate the state directly, which could cause some unexpected results, we should copy the array/object before we mutate them

```javascript
// copy array
const newArr = arr.slice()
const newArr = [...arr]
// copy object
const newObj = {...obj}
const newObj = Object.assign({}, obj)
const newObj = JSON.parse(JSON.stringify(obj))
```

## Object

* person\["name"\], \[\] has an advantage to using var to access the property
* 通常，除非必须实用变量来访问属性，否则我们建议用点表示法

## Array

* allow different types in the array
* length is adjustable dynamically, you can even change the length directly because the length is not read-only
* use `Array.isArray(value)` to check array is preferred, instead of `value instance Array` because the passed array might have different constructor with the default Array if you are using two different frameworks

### Transfer Array

* toLocalString\(\), toString\(\), will concat values in String, split by ","
* valueOf\(\), return Array
* join\(\), set the splitation char, e.g `colors.join("|")`

### Stack method

* push\(\), allow multiple parameters
* pop\(\), remove and get the last entry

### Queue method

* shift\(\), remove and get the first entry
* unshift\(\), add multiple parameters to the head

### Sort method

* reverse\(\)
* sort\(\), sort by value, start with the minimum
  * but the value is from `toString()`, so the comparision is based on String too
  * so we can add a compare function as the parameter of the sort method
  * ```javascript
    function compare(value1, value2) {
      if(value1 < value2) {
        return -1;
      } else if(value1 > value2) {
        return 1;
      } else {
        return 0;
      }
    }

    // for the Number type or the Object which valueOf() will return Number, you can use a simple function
    function compare(value1, value2) {
      return value2 - value1;
    }

    values.sort(compare);
    ```
* ​

### Operate method

* concat\(\), create a **new Array** based on all entries in the current array, the parameters will be added to the end, they could be 1 or more values and arrays `colors.concat("yellow", ["black", "blue"])`
* slice\(\), create a **new Array** based on 1 or more entries in the current array
  * if only one parameter, return the parameter to the end of the array
  * if two parameters, return the Array between start and end indexes \(not include end position\)
    * if end &lt; start, return \[\]
  * if there are negative values, the position should be values + length until it becomes positive
* splice\(\)
  * delete, start index and the number of entries that you want to delete
  * insert, start index, 0, the entries that you want to insert \(could be multiple item1, item2, ...\)
  * replace, start index, the number of entries that you want to delete and the number of entries that you want to insert
* the return value of slice or splice will be the values are deleted

### Position method

* indexOf\(\), start from the head, comparision following ===
* lastIndexOf\(\), start searching from the end, comparison following ===

### Iteration method - for given function

* every\(\), if every entry return true, then it returns true
* some\(\), if any entry returns true, then it returns true
* filter\(\), return the subArray that return true
* forEach\(\), execute some operations, no return
* map\(\), return the result of this function to be a new Array

### Reduce method \(recursive\)

* reduce, reduceRight - accept 4 parameter, pre, cur, index and array. The return value wil be the first parameter for next recursion
* ```javascript
  var values = [1,2,3,4,5]
  var sum = values.reduce(fucntion(prev, cur, index, array) {
    return prev + cur;
  }) //15
  ```
* ​

## Date

* Date.parse\(\), new Date\(\)中也会后台调用
* Date.now\(\) = +new Date\(\);

## RegExp

* `var pattern = new RegExp("[bc]at", "i");`
  * flags:
    * g: global, for all strings, not first match
    * i: case-insensitive
    * m: multiline
    * all these flags are attr of the RegExp
* exec\(String\), return the array of the first match, even though you might use the "g" flag
  * ```javascript
    var text = "cat, bat, sat, fat";
    var pattern = /.at/g;
    var matches = pattern.exec(text);
    console.log(matches.index); //0
    console.log(matches[0]); //cat
    console.log(pattern.lastIndex);//0

    matches = pattern.exec(text);
    console.log(matches.index); //5
    console.log(matches[0]); //bat
    console.log(pattern.lastIndex);//8

    //if there is no g flag, then the second time will remain the same,
    ```
* text\(String\), return true if there is a match

## Function

* No Functional Overloading
* Function declaration vs Function Expression

  * ```javascript
    alert(sum(1,1));
    function sum(num1, num2) {
      return num1 + num2;
    }
    //the above works correctly because the function declaration hoisting process

    alert(sum(1,1));
    var sum = function sum(num1, num2) {
      return num1 + num2;
    }

    //error, it is function initialization not declaration, which is not parsed in advanced
    ```

  ​

### Function as a value

* understand that the name of the function is actually a var, so function could be used as a value, a.k.a, it could be a passing parameter or return value
* ```javascript
  function createComparisonFucntion(propertyName) {
    return function(obj1, obj2) {
      var value1 = obj1[propertyName];
      var value2 = obj2[propertyName];
      return value1 - value2;
    }
  }

  var data = [{name:"Tom", age: 28}, {name:"Jack", age: 29}];
  data.sort(createComparisonFucntion("name")); //data[0].name="Jack"
  data.sort(createComparisonFucntion("age")); //data[0].name="Tom"
  ```

### Function Internal Attr

* arguments
  * callee, a point which points to the function owning the arguments
  * ```javascript
    function factorial(num) {
      if(num <=1) {
        return 1;
      } else {
        //return num * factorial(num -1);
        return num * arguments.callee(num -1); //better, so that it does not have to rely on the function name, which might be changed 
      }
    }
    ```
* this
  * 引用的是函数据以执行的环境对象
  * ```javascript
    window.color = "red";
    var o = {color: "blue"};
    function sayColor() {
      alert(this.color);
    }
    sayColor(); //"red"
    o.sayColor() = sayColor();
    o.sayColor(); //"blue"
    ```
  * 注意改变的只是Scope，函数仍然指向的是同一个函数
* caller

  * 保存调用当前函数的引用
  * ```javascript
    function outer() {
      inner();
    }
    function inner() {
      alert(inner.caller);
      alert(arguments.callee.caller);
    }
    outer(); //source codes of outer function
    ```

  ​

### Function Attribute and Method

* length, means the params that function expects to get
* apply\(scope, args\)
  * scope: 
  * args: the array of params
* call\(scope, arg1, arg2, ...\)
* bind\(obj\)
* ```javascript
  window.color = "red";
  var o = { color: "blue"};
  function sayColor() {
    alert(this.color);
  }
  sayColor(); //red
  sayColor.call(this); //red
  sayColor.call(window); //red
  sayColor.call(o); //blue, so we dont have to put o.sayColor = sayColor()
  //we can also use bind
  var objSayColor() = sayColor.bind(o);
  objSyaColor(); //blue
  ```

### Boolean type

* `var falseObject = new Boolean(false) // falseObject is true`
* never use Boolean obj

### Number type

* `num.toFixed(2)` //10.00
* `num.toExponential(1)` //"1.0e+1"
* ```javascript
  var numObj = new Number(10);
  var numVal = 10;
  alert(typeof numObj); //"object"
  alert(typeof numVal); //"number"
  alert(numObj instanceof Number); //true
  alert(numVal instanceof Number); //false
  ```

### String type

* str.charAt\(\)
  * str\[\]
* str.concat\(\) - get a new string
* slice\(start, end\)
  * 负值与字符串长度相加
* substr\(start, length\),
  * 负的第一个参数加上字符串长度
  * 负的第二个参数转换为0
* substring\(start, end\)
  * 所有负值都转换为0，并且在有需要的情况下会根据大小交换start和end的位置
* indexOf, lastIndexOf,
  * the second param is the start index, so we can use a loop to get all occurs
  * ```javascript
    var str = "xxxx";
    var positions = new Array();
    var pos = str.indexOf("x");

    while(pos > -1) {
      positions.push(pos);
      pos = str.indexOf("x", pos+1);
    }
    ```
  * ​
* trim
* toLowerCase, toLocaleLowerCase, toUpperCase, toLocaleUpperCase, noramlly with locale is preferred
* match\(\)
  * return an array of all matches
* search\(\)
  * return the index of the first match
  * always search from the beginning
* replace\(\)
  * first param:
    * if it is string, then it only replaces the first match
    * if it is reg

### Global Object

* URI encoding
  * encodeURI\(\) 对于整个URI进行编码
  * encodeURIComponent\(\) 对于URI中的某一段进行编码，替换所有无效的字符，实践中更常用，用于对查询的字符串参数进行编码
  * decodeURI\(\), decodeURIComponent\(\)
* eval\(\)
  * 执行js string

### Math Object

* Math.random\(\), return a random float between 0 and 1
  * get a random num between 1 and 10, Math.floor\(Math.random\(\) \* 10 + 1\);


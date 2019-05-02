# 7.Function Expression

* function declaration
  * function declarartion hoisting
  * 只能创造局部函数
* anonymous function

## Recursion

* `arguments.callee()` pointer which points to the executing function
  * ```javascript
    `use strict`
    // strict mode does not allow arguments.callee
    var factorial = (function f(num) {
      if (num <= 1) {
        return 1;
      } else {
        return num * f(num - 1);
      }
    });
    ```

## Closure

* 闭包指的是有权访问另一个函数作用域中的变量的函数
* 创建闭包的常见方式，就是在一个函数内部创建另一个函数，内部函数能够访问到外部函数的变量

### Closure and Variables

* 闭包只能取得包含函数中任何变量的最后一个值

```javascript
//every function will return 10, because the closure refers to the var obj not the var
function createFunctions() {
  var result = new Array();
  for (var i = 0; i < 10; i++) {
    result[i] = function() {
      return i;
    };

    // the correct usage, 通过立即执行匿名函数的结果赋值给数组，在调用每个匿名函数的时候，我们传入变量i，由于函数参数是按值传递的，所以变量i的当前值就复制给了num。而在这个匿名函数内部，又创建并返回了一个访问num的闭包
    result[i] = function(num) {
      return function() {
        return num;
      }
    }(i);
  }

  return result;
}
```

### this obj

* 匿名函数的执行环境具有全局性，所以其this对象通常指向window
* ```javascript
  var name = "the window";
  var obj = {
    name: "My obj",
    getNameFunc: function() {
      //case 1
      return this.name;

      //case 2
      return: function() {
        return this.name;
      }

      //case 3
      var that = this;
      return function() {
        return that.name;
      }
    }
  }

  //case 1
  alert(obj.getNameFunc()); // my obj
  //case 2
  alert(obj.getNameFunc()()); // the window, note that it is a function, and you can take it as that you are calling a function which return this.name, and there is no object scope of this function, so it uses the global scope

  //case 3
  alert(obj.getNameFunc()()); //My obj
  ```
* Memory release, if there is an obj in the closure scope is still referred, then memory will never be collected
  * ```javascript
    //the element referrance will be at leaset 1
    function assignHandler() {
      var element = document.getElementById("xxx");
      element.onclick = function() {
        alert(element.id);
      };
    }

    //
    function assignHandler() {
      var element = document.getElementById("xxx");
      var id = element.id;
      element.onclick = function() {
        alert(id);
      };
      element = null; // release the referrence to the DOM obj
    }
    ```

### Block Scope

* there is no block scope in js
  * typical example: the for loop index will be the last value outside of the for loop block
* anonymous function could create a block scope
  * 将函数声明变为函数表达式可以通过加上一对（）
  * ```javascript
    function outputNumbers(count) {
      (function() {
        for (var i = 0; i < count; i++) {
          alert(i);
        }
      })();
      alert(i); //undefined
    }
    ```
  * this is often used in the global scope so that it can make the global scope to be clean without too many vars and functions

### Private Variables

```javascript
function MyObj() {
  //private vars and functions
  var privateVar = 10;
  function privateFunc() {
    return false;
  }

  //privileged method, only this method can access the private var and methods
  this.publicMethod = function() {
    privateVar++;
    return privateFunction();
  };
}
```

* the cons is the cons of using constructor

### Static Private Variables

```javascript
(function(){
  var name = "";

  // Note that the constructor object is global
  Person = function(value) {
    name = value;
  };

  // use the prototype and anonymous functions to make it accessible outside of the block scope
  Person.prototype.getName = function(){
    return name;
  };
  Person.prototype.setName = function(value){
    name = value;
  };
})();
```

### Module Pattern

* singleton，只有一个实例的对象
* 如果必须创建一个对象并以某些数据对其进行初始化，同时还要公开一些能够访问这些私有数据的方法，可以实用模块模式。

```javascript
var application = function() {

  //private var and methods
  var components = new Array();

  //initialization
  components.push(new BaseComponent());

  //public
  return {
    getComponentCount: function() {
      return components.length;
    },
    registerComponent: function() {
      if(typeof component == "object") {
        components.push(component);
      }
    }
  };
}();
```


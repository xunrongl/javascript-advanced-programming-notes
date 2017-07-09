# Chapter 4

## Primitives types V.S Object

### Primitives Value

- immutable

  - ```javascript
    var str = "abc";
    str[0]; // "a"
    str[0] = "d";
    str; // 仍然是"abc";赋值是无效的。没有任何办法修改字符串的内容
    ```

  - ​

### Copy Var value

- primitives: call by value
  - 原始值：存储在栈（stack）中的简单数据段，也就是说，它们的值直接存储在**变量访问的位置**。这是因为这些原始类型占据的空间是固定的，所以可将他们存储在较小的内存区域 – 栈中。这样存储便于迅速查寻变量的值。
  - copy by value, they are different and independent vars but happen to have the same value
- object: call by reference 
  - 引用值：存储在堆（heap）中的对象，也就是说，存储在变量处的值是一个指针（point），指向存储对象的内存地址。这是因为：引用值的大小会改变，所以不能把它放在栈中，否则会降低变量查寻的速度。相反，放在变量的栈空间中的值是该对象存储在堆中的地址。地址的大小是固定的，所以把它存储在栈中对变量性能无任何负面影响。


- copy is the point which points to the object stored in heap

### Pass parameters to function - always copy by value

- need to understand the object case, access to the same object in the memory but the object parameter is still passed by value. **Because the var stores the address of this object in the heap memory**

- ```javascript
  function setName (obj) {
    obj.name = "Nick";
  }
  var person = new Object();
  setName(person);
  alert(person.name); //"Nick"

  function setName (obj) {
    obj.name = "Nick";
    obj = new Object();
    obj.name = "Greg";
  }
  var person = new Object();
  setName(person);
  alert(person.name); //"Nick", if the parameter obj is passed by reference, it will be "Greg"

  var person = {name: "Tom"};
  function foo(name) {
  	name = "Jack";
  }
  foo(person.name);
  console.log(person.name); //"Tom"
  ```

- 另一种理解Call by sharing

  - 该策略的重点是：调用函数传参时，函数接受对象实参*引用的副本*(既不是按值传递的对象副本，也不是按引用传递的隐式引用)。 它和按引用传递的不同在于：在共享传递中对函数形参的赋值，不会影响实参的值。如下面例子中，不可以通过修改形参o的值，来修改obj的值。

    ```
    var obj = {x : 1};
    function foo(o) {
        o = 100;
    }
    foo(obj);
    console.log(obj.x); // 仍然是1, obj并未被修改为100.
    ```

    然而，虽然引用是副本，引用的对象是相同的。它们共享相同的对象，所以修改形参对象的属性值，也会影响到实参的属性值。

    ```javascript
    var obj = {x : 1};
    function foo(o) {
        o.x = 3;
    }
    foo(obj);
    console.log(obj.x); // 3, 被修改了!
    ```

  - 对于对象类型，由于对象是可变(mutable)的，修改对象本身会影响到共享这个对象的引用和引用副本。而对于基本类型，由于它们都是不可变的(immutable)，按共享传递与按值传递(call by value)没有任何区别，所以说JS基本类型既符合按值传递，也符合按共享传递。

- treat the function parameters as the local variables

### Check type

- typeof
- instanceof

### execution context and scope

- extended scope chain

  - try-catch, catch block
  - with statement

- no block scope

  - ```javascript
    if(true) {
      var color = "blue";
    }
    alert(color); //"blue", the var in the if statement will be added to the current context

    //especially note in the for loop
    for (var i = 0; i < 10; i++) {
      doSomething(i);
    }
    alert(i); // 10

    // However, the scope chain is different
    function changeColor() {
      var color = "red";
    }

    console.log(color); // cannot access color
    ```

  - ​

- Extend scope chain

  - catch block in try-catch

  - with statement, which add the 

    - ```javascript
      fucntion buildUrl() {
        var qs = "?debug=true";
        with(location) {
          var url = href + qs; //here href means "location.href"
        }
        
        return url; // with statement make url as a part of the exectuion env
      }
      ```

    - ​

- Immediately-Invoked Function Expression （IIFE）

  - create naming space

  - ```javascript
    (function (){

    　　　　"use strict";
    　　　　// some code here

    　　 })();  //在函数后面加上()，当js引擎解析到此处时能立即调用函数，括号相当于该函数的括号，用于获取参数，把他由一个函数声明式变成一个函数表达式

    /*------------保存闭包的状态---------*/

    // 它的运行原理可能并不像你想的那样，因为`i`的值从来没有被锁定。
    // 相反的，每个链接，当被点击时（循环已经被很好的执行完毕i已经变成总数，而function仍然去取i的值），因此会弹出所有元素的总数，
    // 因为这是 `i` 此时的真实值，因为function只是被声明了并没有被执行

    var elems = document.getElementsByTagName('a');
    for(var i = 0;i < elems.length; i++ ) {
        elems[i].addEventListener('click',function(e){
            e.preventDefault();
            alert('I am link #' + i)
            },false);
    }

    // 而像下面这样改写，便可以了，因为在IIFE里，`i`值被锁定在了`lockedInIndex`里。
    // 在循环结束执行时，尽管`i`值的数值是所有元素的总和，但每一次函数表达式被调用时，
    // IIFE 里的 `lockedInIndex` 值都是`i`传给它的值,所以当链接被点击时，正确的值被弹出。

    var elems = document.getElementsByTagName('a');
    for(var i = 0;i < elems.length;i++) {
        (function(lockedInIndex){
            elems[i].addEventListener('click',function(e){
                e.preventDefault();
                alert('I am link #' + lockedInIndex);
                },false)
        })(i);
    }
    ```

### Garbage Collection

- 离开作用域的值将被自动标记为可回收，因此在垃圾收集期间被删除
- 『mark-and-sweep』是目前主流的垃圾收集算法，其思想是给当前不使用的值加上标记，然后再回首
- 解除变量的引用（set it to be null）有助于消除代码中存在的循环引用现象，对垃圾收集也有好处，为了确保有效的回收内存，应该及时解除不再使用的全局对象、全局对象属性以及循环引用变量的引用
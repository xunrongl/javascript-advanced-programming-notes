# 6.OOP

## Object

### Property

* Data Property
  * Configurable
  * Enumerable: for-in
  * Writable
  * Value
  * ```javascript
    var person = {
      name: "Tom"
    };

    var person = {};
    Object.defineProperty(person, "name", {
      writable: false,
      value: "Tom"
    });
    person.name = "Jack";
    alert(person.name); //Tom

    //一旦把configurable设置为false，就不能改为true了
    ```
* Accessor Property
  * Configurable
  * Enumerable
  * Get
  * Set
  * ```javascript
    var book = {
      _year: 2004,  //_means this property is only accessible by functions
      edition: 1
    };
    Object.defineProperty(book, "year", {
      get: function() {
        return this._year;
      },
      set: function(newValue) {
        if (newValue > 2004) {
          this._year = newValue;
          this.edition += newValue - 2004;
        }
      }
    });

    book.year = 2005;
    alert(book.edition); // 2
    ```
* Object.defineProperties
* Object.getOwnPropertyDescriptor\(\)
  * ```javascript
    var book = {};
    Object.defineProperties(book, {
      _year: {
        value: 2004
      },
      edition: {
        value: 1
      },
      year: {
        get: function() {
          return this._year;
        },
        set: function() {
          if (newValue > 2004) {
            this._year = newValue;
            this.edition += newValue - 2004;
          }
        }
      }
    });

    var descriptor = Object.getOwnPropertyDescriptor(book, "_year");
    alert(descriptor.value); //2004
    alert(descriptor.configurable);  //false
    alert(typeof descriptor.get); //undefined
    ```

```text
var descriptor = Object.getOwnPropertyDescriptor(book, "year");
alert(descriptor.value); //undefined
alert(descriptor.configurable);  //false
alert(typeof descriptor.get); //"function"
​
```

```text
## Create Object

### Factory Pattern

```javascript
function createPerson(name, age) {
  var o = new Object();
  o.name = name;
  o.age = age;
  o.sayName = function() {
    alert(this.name);
  };
  return o;
}
var person1 = createPerson("Tom", 22);
```

Solve the problem on creating similar objects, but fails to recognize objects

### Constructor Pattern

* ```javascript
  function Person(name, age) {
    this.name = name;
    this.age = age;
    this.sayName = function() {
      alert(this.name);
    };
    //this.sayName = sayName;
  }
  var person1 = new Person("Tom", 22);

  alert(person1.constructor == Person); //true
  alert(person1.sayName == person2.sayName); //false

  //让对象共享global的sayName()函数，但这就牺牲了封装性
  function sayName() {
    alert(this.name);
  }
  ```
* cons: 因为js中函数是对象，所以每定义一个函数，也就是实例化了一个对象。会导致不同实例上的同名函数是不相等的

### Prototype Pattern

* 每个函数都有一个prototype属性，这个属性是一个指针，指向一个对象，而这个对象的用途是包含可以由特定类型的所有实例共享的属性和方法。
* ```javascript
  function Person() {
  }
  Person.prototype.name = "Tom";
  Person.prototype.age = 29;
  Person.prototype.sayName = function() {
    alert(this.name);
  };
  var person1 = new Person();
  person1.sayName(); //Tom

  alert(Person.prototype.isPrototypeOf(person1)); //true
  alert(Object.getPrototypeOf(person1) == Person.prototype);  //true
  alert("name" in person1); //true

  person1.name = "Jack";
  alert(person1.hasOwnProperty("name")); //true
  person1.sayName(); //Jack

  var keys = Object.keys(Person.prototype);
  alert(keys); //"name, age, sayName"
  var p1keys = Object.keys(person1);
  alert(p1keys); //"name"

  delete person1.name;
  person1.sayName(); //Tom

  //clearer grammer
  Person.prototype = {
    name: "Tom",
    age: 29,
    sayName: function() {
      alert(this.name);
    }
  };
  //However, constructor has changed
  alert(person1.construtor == Person); //false

  //dynamic prototype, you can create the instance then update the prototype, Note: just update, not recreate the prototype, b/c point points to the prototype not constructor
  var friend = new Person();
  Person.prototype.sayHi = function() {
    alert("hi");
  };
  friend.sayHi(); //"hi"

  //Prototype properties are shared by instances, it is ok for functions and primitive types, however for reference values, there will be an issue
  Person.prototype.friends = ["Jack", "Tony"];
  person1.friends.push("Van");
  alert(person2.frends); //"Jack", "Tony", "Van"
  ```

```text
- https://pic3.zhimg.com/3f1d54bcf3b324540e263390f318f92a_r.png

### Constructor + Prototype Pattern

​```javascript
//reference property should be defined in the constructor
function Person(name, age) {
  this.name = name;
  this.age = age;
  this.friends = ["Jack", "Tony"];
}
Person.prototype = {
  constructor: Person,
  sayName: function() {
    alert(this.name);
  }
};
```

## Inheritance

### Prototype Chaining

```javascript
function SuperType() {
  this.property = true;
}

SuperType.prototype.getSuperValue = function() {
  return this.property;
};

function SubType() {
  this.subproperty = false;
}

//inherit SuperType
SubType.prototype = new SuperType();
SubType.prototype.getSubValue = function() {
  return this.subproperty;
}

var instance = new SubType();
alert(instance.getSuperValue()); //true

//search example
var o = {a:1};
console.log(o.toString); // no errors, Object has toString
console.log(o.push("c")); //error, Object does not even has this function
```

* 每个对象都有一个指向它的原型对象的内部链接，这个原型对象又有自己的原型，直到某个对象的原型为null为止。所以当你试图访问一个对象的属性的时候，他不仅仅在该对象上搜寻，还会搜寻该对象的原型。以此层层向上的搜索。
* Object is the default prototype
* use `instanceof` and `isPrototypeOf` to confirm the relationship between prototype and instance
* 不能使用对象字面量创建原型方法
  * ```javascript
    // this will rewrite the prototype chain, SubType is the instance of Object instead of 
    SubType.prototype = {
      getSubValue: function() {
        return this.subproperty;
      }
    }
    ```
* Cons：
  * 原型中包含引用类型值类型会带来问题
    * ```javascript
      function SuperType() {
        this.colors = ["red", "blue"];
      }

      function SubType() {
      }

      //SubType 通过原型链继承了SuperType后，SubType.prototype 变成SuperType的一个实例，他也因此拥有了一个自己的colors属性——就跟专门创建了一个SubType.prototype.colors属性一样，这个属性将被所有的SubType实例所共享
      SubType.prototype = new SuperType();
      var instance1 = new SubType();
      instance1.colors.push("black");
      alert(instance1.colors); //red, blue, black
      var instance2 = new SubType();
      alert(instance2.colors); //red, blue, black
      ```
    * ​
  * 无法在不影响所有对象实例的情况下，给SuperType的构造函数传递参数
  * 所以很少单独实用原型链

### Constructor Stealing

* use call or apply

```javascript
function SuperType() {
  this.colors = ["red", "blue"];
}

function SubType() {
  //inherit SuperType
  SuperType.call(this);

  //you can also use this to pass the parameters
  //SuperType.call(this, "Tom")
}

var instance1 = new SubType();
instance1.colors.push("black");
alert(instance1.colors); //red, blue, black
var instance2 = new SubType();
alert(instance2.colors); //red, blue
```

* However, in this way, all the functions are in constructor

### Combination inheritance

```javascript
function SuperType(name) {
  this.name = name;
  this.colors = ["red", "blue"];
}

SuperType.prototype.sayName = function() {
  alert(this.name);
}

function SubType(name, age) {
  //inherit SuperType property
  SuperType.call(this, name);
  this.age = age;
}

//inherit methods
SubType.prototype = new SuperType();
SubType.prototype.sayAge = function() {
  alert(this.age);
}

var instance1 = new SubType("Tom", 29);
instance1.colors.push("black");
alert(instance1.colors); //red, blue, black
instance1.sayName(); //Tom
instance1.sayAge(); //29
var instance2 = new SubType("Jack", 22);
alert(instance2.colors); //red, blue
instance2.sayName(); //Jack
instance2.sayAge(); //22
```

### Parasitic Inheritance

### Parasitic Combination Inheritance


# 13.Events

## Event flow

* event bubbling: event pass from leaf to the root in the DOM tree until the document obj
* event capturing:from the root to the leaf that is actually clicked
* event capturing -&gt; target stage -&gt;event bubbling，有两个机会在目标对象上操作事件

## Event handler

* DOM 0 level event handler

```javascript
// the event is captured during the bubbling 
btn.onclick = function () {
  alert(this.id); //this refers to the current element
}
```

* DOM 2 level event handler

```javascript
btn.addEventListener("click", function() {
  alert(this.id);
}, false);
// the last param, if it is true, then it is captured during capturing stage, otherwise, it is in bubbling stage
btn.removeEventListener("click", fuction(){ // the function has to be exactly the same, i.e. same function obj
  alert(this.id);
}, false); //this wont work
```

## Event Object

* 如果事件处理程序指定给了目标元素

```javascript
btn.onclick = function(event) {
  alert(event.currentTarget === this); // true
  alert(event.target === this); //true
}
```

* 如果事件处理程序指定给了父节点

```javascript
document.body.onclick = function(event) {
  // 前两个都是在body上，因为事件注册在此
  alert(this === document.body); //true
  alert(event.currentTarget === document.body); // true
  // 是click的真正目标(点在btn上)，而click上并没有注册事件处理程序，结果click事件就冒泡到了document.body
  alert(event.target === document.getElementById("myBtn")); //true
}
```

* 利用`event.type`可以在一个handler里通过switch处理多种events
* `preventDefault()`
* `stopPropagation()`
* eventPhase
  * ```javascript
    var btn = document.getElementById("myBtn");
    btn.onclick = function(event) {
      alert(event.eventPhase); // 2
    };

    document.body.addEventListener("click", function(event) {
      alert(event.eventPhase); // 1
    }, true);

    document.body.onclick = function(event) {
      alert(event.eventPhase); // 3
    };

    // when the btn is clicked, 1 -> 2 -> 3
    ```

## Event Type

* load

```javascript
EventUtil.addHandler(window, "load", function(event) {
  alert("Loaded!");
});

<body onload="alert('Loaded!')">
//image has onload has well
```

* unload
* resize
* scroll
* focus
* MouseEvents
  * ```javascript
    EventUtil.addHandler(div, "click", function(event) {
      event = EventUtil.getEvent(event);
      alert("Client coordinates: " + event.clientX + "," + event.clientY);
    })

    //event.pageX, event.pageY equals to clientX, clientY when there is no scroll
    //screenX, screenY
    ```
  * RelatedElements when mouseover and mouseout
* touchscreen
* keyboard
* textInput event
* Mutation events
  * removeChild, replaceChild
  * appendChild, insertBefore
* HTML5 events
  * contextmenu
* Device events
  * orientationchange
* touch and gesture events

## Memory and performance

* event delegation
  * ```javascript
    <ul id="mylinks">
      <li id="1">Go</li>
      <li id="2">To</li>
      <li id="3">Heaven</li>
    </ul>

    // the click on li will be handled when it goes through the bubbling process 
    var list = document.getElementById('myLinks');
    EventUtil.addHandler(list, "click", function(event) {
      event = EventUtil.getEvent(event);
      var target = EventUtil.getTarget(event);
      switch(target.id) {
        case "1":
          break;
        ...
      }
    })
    ```
  * ​

## Simulate DOM event


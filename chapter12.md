# 12.DOM2 and DOM3

## DOM Changes

## Styles

```javascript
<div id="myDiv" style="background-color:blue">
  // set styles in js
var myDiv = document.getElementById("myDiv");
myDiv.style.width = "100px";

//style在这里是一个obj
myDiv.style.cssText = "整个html的in line style"
myDiv.style.length
myDiv.style[prop]=value
// or
prop = style.item(i);
value = myDiv.style.getPropertyValue(prop);
```

### CSSStyleSheet

* document.styleSheets
* CSSRule

### Element Size

* offset dimension
  * offsetHeight,  offsetTop
  * offsetWidth, offsetLeft = border + padding + content
* client dimension
  * clientHeight, clientWidth = padding + content
* scroll dimension
  * scrollHeight, scrollWidth: the total height/Width without the scroll bar
  * scrollLeft, scrollTop, 被隐藏在内容区域左侧/上方的像素数（包括内容区的边框），通过修改这个值，可以修改元素的滚动位置

## Traversal

### NodeIterator

### TreeWalker

## Range

### Range in the DOM


# DOM

## Node

### Node Type

- `someNode.nodeType`
- nodeName and nodeValue
- `someNode.childNodes[i]` - NodeList
- cloneNode(true)
  - true - deep copy
  - false - shallow copy, does not include the children nodes

### Document Type

- <html>

  - ```javascript
    var html = document.documentElement;
    alert(html === document.childNodes[0]);
    alert(html === document.firstChild);
    ```

- `var doctype = document.doctype; //<!DOCTYPE>`

- document.title

- document.URL

- document.domain

  - writable, but some limitation

- getElementById

- getElementsByTagName() - return NodeList obj

- getElementsByName()

- document.anchors

- document.forms which equals to document.getElmentsByTageName("form")

- document.images

- document.links, return all anchors with href attrs

- document文档写入方法

### Element Type

- nodeType = 1, this can be used to check if the obj is an element
- nodeName is the tag name, `tagName`
- nodeValue = null
- html element attributes
  - standard attributes, `id, className, title, lang`
  - customized attributes normally starts with `data-`
  - `div.getAttribute("id")`, 对于style的返回值为css文本，而通过属性来访问则它会返回一个对象
  - setAttribute("id", "newId")
  - removeAttribute()
- attributes property
  - `var id = element.attributes.getNamedItem("id").nodeValue`
- create elements
  - `document.createElement()`, just create elment, not add to the DOM yet
- ​

### Text Type

- nodeValue is the text in the node

- create text node

  - ```javascript
    var element = document.createElement("div");
    var textNode = document.createTextNode("xx");
    element.appendChild(textNode);
    document.body.appendChild(element);
    ```

- textNode.parentNode.normalize() - combine the text node which are next to each other

## DOM Operation Technology

### Dynamic Script

### Dynamic Style

### Manipulate Table

```javascript
var table = document.createElement("table");
table.border = 1;
table.width = "100%";
var tbody = document.createElement("tbody");
table.appendChild(tbody);
tbody.insertRow(0);
tbody.rows[0].insertCell(0);
tbody.rows[0].cells[0].appendChild(document.createTextNode("Cell 1,1"));
document.body.appendChild(table)
```


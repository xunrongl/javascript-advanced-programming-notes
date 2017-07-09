# 21. AJAX and Comet

## XMLHttpRequest Object

```javascript
var xhr = createXHR()l
xhr.onreadystatechange = function() {
  if (xhr.readyState == 4) { // only care about the status 4, when all the data is received
    if((xhr.status >= 200 && xhr.status < 300 || xhr.status == 304) {
      alert(xhr.responseText);
    } else {
      alert("request was unsuccessful: " + xhr.status);
    })
  }
};
xhr.open("get", "example.txt", true); //true means it is ayncs
xhr.setRequestHeader("MyHeader", "MyValue");
xhr.send(null);
// for post
var form = document.getElementById("user-info");
xhr.send(serialize(form));
// or
xhr.send(new FormData(form));
```


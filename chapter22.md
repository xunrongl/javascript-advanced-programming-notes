# 22.Advanced Techniques

### Function binding

```javascript
var handler = {
  message: "Event handled",
  handleClick: function(event) {
    alert(this.message);
  }
};

var btn = document.getElementById("my-btn");
EventUtil.addHandler(btn, "click", handler.handleClick); //"undefined", because this points to the DOM/Window

EventUtil.addHandler(btn, "click", handler.handleClick.bind(handler));
```

### Function Currying

## Tamper-proof object


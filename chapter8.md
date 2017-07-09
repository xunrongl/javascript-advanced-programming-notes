# Chapter8 BOM

## window

### Window size

- innerWidth, innerHeight 页面视图区大小
- outerWidth, outerHeight 浏览器窗口本身尺寸
- Chrome中inner和outer都返回相同的值，都是viewport大小，即document.documentElment.clientWidth，这类在各个浏览器都是返回viewport

### navigation

- window.open()

  - block

    - ```javascript
      var blocked = false;
      try {
        var wroxWin - window.open("http:xxx", "_blank");
        if(wroxWin == null) {
          blocked = true;
        }
      } catch(ex) {
        blocked = true;
      }

      if (blocked) {
        alert("xxx");
      }
      ```

### interval and timeout

- setTimeout完并不是立即执行，the second param is the time waited to put the current task to the task queue;
- setInternal
  - remember to `clearInterval(intervalId)`

### system dialogue

- confirm()
- prompt()

## location

- `window.location === document.location`

- `location.search` returns the query strings

- ```javascript
  function getQueryStringArgs() {
    var query = location.search.length > 0 ? location.search.substring(1) : "";
    var args = {};
    var items = query.split("&");
    
    for(var i = 0; i < items.length; i++) {
      var strs = decodeURIComponent(items[i]).split("=");
      args[strs[0]] = strs[1];
    }
   
    return args;
  }
  ```

- `location.href = "xxx"` equals to `location.assign("xxxx");`

- use `location.replace`, the back button won't work

## navigator

- navigaotr.plugins

## screen

## history

- history.go()
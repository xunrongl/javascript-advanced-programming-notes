# 17.Error Handling and Debugging

## Error Handle

* finally block statement will always run even though there may be `return` in try block

### Error Type

* type convert error
* data type error
  * if you want to use sort\(\) method, make sure check `if(values instanceof Array)`
* communication error
  * must `encodeURIComponent()` before sending data to the server
* log errors to the server
  * ```javascript
    function logError(sev, msg) {
      var img = new Image();
      img.src = "log.pho?sev=" + encodeURIComponent(sev) + "&msg=" + encodeURIComponent(msg);
    }
    ```
  * ​

## Debug


# 20. JSON

- property name has to be double quoted

- JSON.stringify()

  - `JSON.stringify(book, ["title", "edition"])`

  - ```javascript
    JSON.stringify(book, function(key, value) {
      //switch commands to set up a filter based on the key
      //if the return value is undefined, then remove this property from the JSON
    })
    ```

  - JSON.stringify(book, null, 4): the last params are for indent

- JSON.parse()
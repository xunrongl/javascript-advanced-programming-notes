# 14.Forms

* submit form
  * set the type to be "submit" in `<input>` or `<button>`
  * `form.submit()` won't trigger submit event
* reset form
  * `type="reset"`
  * `form.reset();` will trigger reset event
* form properties
  * ```javascript
    // use submit event instead of click event, because the click might happen before or after submit depends on the browser
    EventUtil.addHandler(form, "submit", function(event) {
      event = EventUtil.getEvent(event);
      var target = EventUtil.getTarget(event);
      var btn = target.elements["submit-btn"];
      btn.disabled = true; // prevent multiple submit
    })
    ```
* focus\(\), blur\(\)
  * `<input type="text" autofocus>`
  * onchange event

## Text Script

* `<input type="text" size="the number of the chars to display" maxlength="max chars">`
* `<textarea rows="25" cols="5">` cannot set the max chars for textarea
* select\(\) - select all texts
* select event
* textbox element has `textbox.selectionStart` and `textbx.selectionEnd` which you can get the selected text
* setSelectionRange\(\)
* filter input
  * block charcode
    * ```javascript
      EventUtil.addHandler(textbox, "keypress", function(event) {
        event = EventUtil.getEvent(event);
        var target = EventUtil.getTarget(event);
        var charCode = EventUtil.getCharCode(event);
        if(!/\d/.test(String.fromCharCode(charCode))) {
          EventUtil.preventDefault(event);
        }
      })
      ```
  * clipboardData
    * `event.clipboardData.getData("text")`
    * `event.clipboardData.setDate("text/plain", value)`
  * use `maxlength` to set the focus automatically

### HTML5 form API

* required
* type = "email", "url", "number"
  * for number, `<input type="number" min="0" max="100" step="5" name="count">`
* pattern, `<input type="text" pattern="\d+" name="count">`, only allows numbers
* select


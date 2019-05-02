# 2.Javascript in HTML

## script

* async
  * optional, `<script src="src.js" async></script>`
  * download script immediately but not blocking other operations in the page \(a.k.a async\)
  * cannot guarantee the order
  * do not modify the DOM in the async script - because it is hard to tell if it is loaded
    * The `$(document).ready` occus after the HTML document has been loaded while the `document.onload` occurs later, when all content \(e.g images\) also has been loaded
    * The `onload` is a standard DOM event, while the `ready` is from jQuery and it is to add funcitionality to the elments in the page before all the content loads.
* defer
  * optional, `<script src="src.js" defer></script>`
  * download immediately but not be executed until the document is fully resolved and displayed \(broswer hit the `</html>`\)
  * deferred script might not follow the orders, it'd better only include **one** deferred script
* if there is no `defer` or `async`, the script will load following the orders
* To avoid the delay causing by loading script, we will put the script in the `<body>` instead of `head`, otherwise the DOM content will not be rendered until all scripts are downloaded


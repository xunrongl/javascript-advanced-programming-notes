# 24. Best Practice

- do not compare with null
- scope
  - avoid global search
    - if you have multiple document.getElementsById or so, use `var doc = document` to reduces multiple referrences to document objects
- For large changes in DOM, innerHTML is much faster than createElement and appendChild
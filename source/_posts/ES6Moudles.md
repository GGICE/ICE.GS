---
title: ES6 Modules in Chrome 61 +
date: 2017-11-04 22:24:28
tags: 
 - web development
---

在Chrome 61+ 中尝试使用ES6 Modules

### Index.html

```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
</head>
<body>
  <p>2+3 = <span class="result"></span></p>
  <script type="module">
    import { add } from './common.js'; 
    (function () { 
      document.querySelector('.result').innerText = add(2, 3); 
      console.log(add(2, 3));
    }());
  </script>
</body>
</html>
```

### Common.js

```
console.log('common.js');
export function add (a, b) {
    return a + b;
}
```

### Run

[Click it](https://codepen.io/GGICE/project/editor/ZwBqyg)


---
title: "JSON 返回参数"
subtitle: ""
layout: post
author: "libyasdf"
header-style: text
tags:
  - JSON
  - DOM
---

```
  let pass = JSON.stringify(values, (key, val) => {
      if (typeof val === "string") {
        // 替换json中string前后的空格
        return val.replace(/^\s+|\s+$/gm, '');
      }
      return val;
    });
```
---
title: path.Base
description: Base returns the last element of a path.
date: 2018-11-28
publishdate: 2018-11-28
lastmod: 2018-11-28
categories: [functions]
menu:
  docs:
    parent: "functions"
keywords: [path, base]
signature: ["path.Base PATH"]
workson: []
hugoversion: "0.40"
relatedfuncs: [path.BaseName, path.Clean, path.Dir, path.Ext, path.Join, path.Split]
deprecated: false
---

`path.Base` returns the last element of `PATH`.

If `PATH` is empty, `.` is returned.

**Note:** On Windows, `PATH` is converted to slash (`/`) separators.

```go-html-template
{{ path.Base "a/news.html" }} → "news.html"
{{ path.Base "news.html" }} → "news.html"
{{ path.Base "a/b/c" }} → "c"
{{ path.Base "/x/y/z/" }} → "z"
```

---
title: VSCode Snippet For Hexo
date: 12-08-2019 15:03:08
categories: "hexo"
tags:
---

```json
{
	"write a new post for hexo": {
    "prefix": "hexo-new",
    "body": [
      "---",
      "title: ${1:post's title}",
      "date: $CURRENT_DATE-$CURRENT_MONTH-$CURRENT_YEAR $CURRENT_HOUR:$CURRENT_MINUTE:$CURRENT_SECOND",
      "categories: ${2:categories}",
      "tags: [\"${3:tag1}\", \"${4:tag2}\"]",
      "---",
      ""
    ],
    "description": "新建一篇HEXO文章.md"
  },
}
```

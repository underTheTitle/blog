---
title: "trim script"
description: "Elasticsearch trim을 사용하여 공백 제거"
date: 2020-04-27
---

keyword필드의 앞, 뒤 공백을 잘라야 할 일이 생겨서 쿼리를 작성해보았다.

```json
GET test_index/_update_by_query

{
  "script": {
    "inline": "ctx._source.str_field = ctx._source.str_field.trim();",
    "lang": "painless"
  }
}

"  문 자  열  "-> "문 자  열"
```

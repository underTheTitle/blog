---
title: "terms aggs에서 script로 정규식 사용"
description: "Elasticsearch terms aggregation에서 정규식 사용"
date: 2020-04-27
---

데이터가 d_100, w_100 이런식으로 저장되어 있었는데,

d, w따로 나누어서 갯수를 집계해야 할 일이 생겼다.

어떻게 처리할까 생각하다가 정규식을 이용해서 분리했다.

```json
{
  "size": 0,
  "aggs": {
    "office": {
      "terms": {
        "field": "office_field"
      },
      "aggs": {
        "danger": {
          "terms": {
            "script": "String aaa = /_/.split(doc.value_list[0])[0]; if (aaa == 'd') {doc['grp_nm'].value}",
            "size": 20
          }
        },
      "aggs": {
        "warning": {
          "terms": {
            "script": "String aaa = /_/.split(doc.value_list[0])[0]; if (aaa == 'w') {doc['grp_nm'].value}",
            "size": 20
          }
        }
      }
    }
  }
}
```

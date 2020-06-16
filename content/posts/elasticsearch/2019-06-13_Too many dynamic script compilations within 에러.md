---
title: 'Too many dynamic script compilations within 에러'
description: 'Elasticsearch 스크립트 에러'
date: 2019-06-13
---

## Too many dynamic script compilations within 에러

```json
{
  "error": {
    "root_cause": [
      {
        "type": "circuit_breaking_exception",
        "reason": "[script] Too many dynamic script compilations within, max: [75/5m]; please use indexed, or scripts with parameters instead; this limit can be changed by the [script.max_compilations_rate] setting",
        "bytes_wanted": 0,
        "bytes_limit": 0
      }
    ]
    ...
  "status": 400
}
```

검색 결과 일정시간안에 너무 많은 스크립트가 발생해서 생기는 오류.

75/5m 으로 세팅되어 있는데 5분에 75개까지의 스크립트를 실행 할 수 있다는 의미.

공식문서를 찾아보니 스크립트를 사용할 떄 가급적 파라미터를 사용하기를 권장한다.

- <https://www.elastic.co/guide/en/elasticsearch/reference/current/modules-scripting-using.html#prefer-params>

@TODO 스크립트 캐시 확인

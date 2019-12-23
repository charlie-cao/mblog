---
title: "hugo 函数解析"
date: 2019-12-17T00:00:00.000Z
description: hugo index.
---
```
{{ if (index .Site.Data.s (.Params.data_file)).teams }}
    {{ range $index, $element := first 10 ( sort ( where (index .Site.Data.s (.Params.data_file)).teams "rating" ">" "4") "score" "desc")}}
        <span style="font-size:12px;">Rated #{{ add $index 1 }}</span>

        {{$element}}
    {{end}
{{end}}}
```

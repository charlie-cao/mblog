---
title: "hugo 函数解析"
date: 2019-12-17T00:00:00.000Z
description: hugo index.
---
```
{{ if (index .Site.Data.s (.Params.data_file)).teams }}
    {{ range $index, $element := first 10 (sort ( where (index .Site.Data.s (.Params.data_file)).teams "rating" ">" "4") "score" "desc")}}
        <span style="font-size:12px;">Rated #{{ add $index 1 }}</span>

        {{$element}}
    {{end}
{{end}}}
```

{{ if (index .Site.Data.s (.Params.data_file)).teams }}

index .Site.Data.s (.Params.data_file) 获取页面参数中的data_file中的数据

(index .Site.Data.s (.Params.data_file)).teams 判断是否存在.

(index .Site.Data.s (.Params.data_file)).teams 获取teams中的数据

( where (index .Site.Data.s (.Params.data_file)).teams "rating" ">" "4") 通过where 进行过滤

(sort ( where (index .Site.Data.s (.Params.data_file)).teams "rating" ">" "4") "score" "desc") 通过sort 进行排序

first 10 (sort ( where (index .Site.Data.s (.Params.data_file)).teams "rating" ">" "4") "score" "desc") 获取前10条 limit
10

range $index, $element := first 10 (sort ( where (index .Site.Data.s (.Params.data_file)).teams "rating" ">" "4") "score" "desc") 遍历结果 获得 $index 和 $element 类似 foreach($datas as $key=>$data){}

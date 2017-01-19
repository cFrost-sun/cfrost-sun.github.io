---
layout: default
title: 改变MySql表中自增列的当前值
---
## {{ page.title }}
很简单，一条语句：

```
ALTER TABLE table_name AUTO_INCREMENT = new_value;
```

{{ page.date | date_to_string }}
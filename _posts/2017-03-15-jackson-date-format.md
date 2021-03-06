---
layout: default
title: 设置Jackson序列化Date类型的时间格式,并支持多种反序列化时间格式
---
## {{ page.title }}

Jackson的```ObjectMapper```在序列化```Date```类型的属性时，会转换为时间戳输出，这种处理方式是兼容性最好的，不会出现任何歧义，一般不建议修改，但是对于一些老系统不得不修改输出时间格式的情况，如果直接调用```ObjectMapper```的```setDateFormat```方法，会同时改变序列化和反序列化的时间格式，这样会导致```ObjectMapper```失去反序列化各种不同时间格式字符串的能力，因此我将```ObjectMapper```扩展，新增一个只修改序列化时间格式的方法

```
public class MyObjectMapper extends ObjectMapper {

    private static final long serialVersionUID = -2795479277489658982L;

    public MyObjectMapper setSerializationDateFormat(DateFormat dateFormat) {
        _serializationConfig = _serializationConfig.with(dateFormat);
        return this;
    }
}
```

这样就可以自定义序列化时间格式，但不会影响反序列化对各种时间格式的支持。

{{ page.date | date_to_string }}

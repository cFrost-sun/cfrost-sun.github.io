---
layout: default
title: Java Class.getResource(name)读取位置优先级
---
## {{ page.title }}

先上一个读取类代码:

```
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.net.URL;
import java.util.Properties;

import org.apache.logging.log4j.LogManager;
import org.apache.logging.log4j.Logger;

public class PropertySource {

    protected final Logger logger = LogManager.getLogger();

    private Properties props;

    public PropertySource(String configFile, Class<?> clazz) throws IOException {
        try {
            URL url = clazz.getResource(configFile);
            InputStream inputStream = url.openStream();
            this.logger.info("Load property file: " + url.toString());
            BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(inputStream));
            this.props = new Properties();
            this.props.load(bufferedReader);
        } catch (NullPointerException e) {
            throw new IOException("Property file \"" + configFile + "\" read error. Does this file exist?", e);
        }
    }

    public Object getProperty(String key) throws NullValueException {
        Object value = this.props.get(key);
        if (value == null) {
            throw new NullValueException("Key read error, no such key: " + key);
        }
        return value;
    }

}
```

* configFile以'/'开头
    1. /WEB-INF/classes
    2. 代码所在包的classpath
    3. clazz所在包的classpath
* configFile不以'/'开头
    1. clazz所在包的classpath

{{ page.date | date_to_string }}

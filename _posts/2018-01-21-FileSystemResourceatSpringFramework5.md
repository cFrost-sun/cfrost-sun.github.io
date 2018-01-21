---
layout: default
title: FileSystemResource在Srping FrameWork 5中的变化
---
## {{ page.title }}

之前在项目中一直使用FileSystemResource这个类作为PropertyPlaceholderConfigurer的Resource引入部署目录外的配置文件，并设置了setIgnoreResourceNotFound为true，最近把Spring Framework升级为5.0.2，当外部配置文件不存在时，PropertyPlaceholderConfigurer的初始化会报错，貌似setIgnoreResourceNotFound不再起作用，今天看了一下源码

先看PropertyPlaceholderConfigurer的父类PlaceholderConfigurerSupport中的一段：

```java
/**
* Load properties into the given instance.
* @param props the Properties instance to load into
* @throws IOException in case of I/O errors
* @see #setLocations
*/
protected void loadProperties(Properties props) throws IOException {
    if (this.locations != null) {
        for (Resource location : this.locations) {
            if (logger.isDebugEnabled()) {
                logger.debug("Loading properties file from " + location);
            }
            try {
                PropertiesLoaderUtils.fillProperties(
                        props, new EncodedResource(location, this.fileEncoding), this.propertiesPersister);
            }
            catch (IOException ex) {
                // Resource not found when trying to open it
                if (this.ignoreResourceNotFound &&
                        (ex instanceof FileNotFoundException || ex instanceof UnknownHostException)) {
                    if (logger.isInfoEnabled()) {
                        logger.info("Properties resource not found: " + ex.getMessage());
                    }
                }
                else {
                    throw ex;
                }
            }
        }
    }
}
```

得知，在设置了ignoreResourceNotFound之后，Resource类抛出的异常必须时FileNotFoundException才能生效，但是在Spring Framework 5中FileSystemResource已经换用nio下面的包来实现了，抛出的也是nio包下面的异常java.nio.file.NoSuchFileException，所以ignoreResourceNotFound不再有效，从FileSystemResource的说明中得知还有PathResource这个类，就换它试了一下，问题解决。

{{ page.date | date_to_string }}

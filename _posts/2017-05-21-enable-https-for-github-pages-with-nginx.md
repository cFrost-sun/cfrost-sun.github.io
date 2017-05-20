---
layout: default
title: 使用NGINX为自定义域名的GitHub Pages启用https
---
## {{ page.title }}

GitHub Pages配置自定义域名后是不支持https的，可参考[为自定义域名的GitHub Pages添加SSL 完整方案](https://www.yicodes.com/2016/12/04/free-cloudflare-ssl-for-custom-domain/)的方案。

但是我并不想注册Cloudflare，参考了一下它的实现原理，觉得用Nginx也可以完成，今晚试了一下，果然成功:

1. 参考[使用Let’s Encrypt配置免费SSL证书](https://cfrost.net/2017/01/16/lets-encrypt.html)为你的Nignx服务器配置免费的SSL证书
2. 在GitHub设置好自定义域名，但是自定义域名的DNS要解析到自己的Nginx服务器
3. 在Nginx上添加GitHub的```upstream```
```
upstream github-pages {
    server 192.30.252.153:443;
    server 192.30.252.154:443;
}
```
4. Nginx的location作如下配置，注意```eulerproject.io```换成自己的域名
```
location / {
    proxy_pass    http://github-pages;
    proxy_set_header Host eulerproject.io;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto  $scheme;
}
```
5. 在浏览器输入自己的域名监测是否成功。

{{ page.date | date_to_string }}

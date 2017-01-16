---
layout: default
title: 使用Let's Encrypt配置免费SSL证书
---
## {{ page.title }}
首先是安装，一定要安装在你域名指向的服务器，而且这个服务器外网能访问到，比如你的nginx服务器，或者就是你网站所在的服务器
```
git clone https://github.com/certbot/certbot
```
安装完成后要保证443端口没有被占用

进入certbot目录，运行
```
./letsencrypt-auto certonly --standalone --email admin@eulerproject.io -d eulerproject.io -d www.eulerproject.io
```
其中eulerproject.io应替换为你的域名，稍作等待，一切会自动运行，成功之后会在```/etc/letsencrypt/```目录生成一系列证书文件，我们只需要到```/etc/letsencrypt/live/yourdomain```目录下，里面有最新版证书文件的link。证书有效期为90天，到期后可运行下面的命令续期：
```
./letsencrypt-auto certonly --renew-by-default --email admin@eulerproject.io -d eulerproject.io -d www.eulerproject.io
```

下面以nginx为例说明如何配置证书
```
server {
	listen      443;
	server_name eulerproject.io;

        ssl on;
        ssl_certificate /etc/letsencrypt/live/eulerproject.io/fullchain.pem;
        ssl_certificate_key /etc/letsencrypt/live/eulerproject.io/privkey.pem;
        ssl_trusted_certificate /etc/letsencrypt/live/eulerproject.io/chain.pem;
        ssl_ciphers 'ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-DSS-AES128-GCM-SHA256:kEDH+AESGCM:ECDHE-RSA-AES128-SHA256:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES256-SHA:ECDHE-ECDSA-AES256-SHA:DHE-RSA-AES128-SHA256:DHE-RSA-AES128-SHA:DHE-DSS-AES128-SHA256:DHE-RSA-AES256-SHA256:DHE-DSS-AES256-SHA:DHE-RSA-AES256-SHA:AES128-GCM-SHA256:AES256-GCM-SHA384:AES128-SHA256:AES256-SHA256:AES128-SHA:AES256-SHA:AES:CAMELLIA:DES-CBC3-SHA:!aNULL:!eNULL:!EXPORT:!DES:!RC4:!MD5:!PSK:!aECDH:!EDH-DSS-DES-CBC3-SHA:!EDH-RSA-DES-CBC3-SHA:!KRB5-DES-CBC3-SHA';
        ssl_prefer_server_ciphers on;
        ssl_dhparam /root/work/dhparam/dhparam.pem;

        orther config...
}
```

* 更多信息请参考[官方文档](https://letsencrypt.org/docs/)
* HTTPS配置检测[https://wosign.ssllabs.com/](https://wosign.ssllabs.com/)


{{ page.date | date_to_string }}
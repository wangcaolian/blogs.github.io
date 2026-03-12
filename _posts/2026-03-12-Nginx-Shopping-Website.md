---

layout: post
title: 用Nginx搭建站点
date: 2026-03-12
catagories: Nginx

---

### 用Nginx搭建站点

- 设置站点名：

	- buy.fighting.top
	
- 站点根目录：

	- /app/code/buy/

- 首页

	- /app/code/buy/index.html
	
- 修改配置文件：
	
	- vim /etc/nginx/conf.d
```
server {
	listen 80;
	server_name buy.fighting.top;
	root /app/code/buy;
	location / {
		index index.html;
	}
	location /admin/ {
	allow 172.16.1.0/24;
	deny all
	}
}
```

- url 为 buy.fighting.top/admin
- uri 为 /admin/

- mkdir -p /app/code/buy/admin/
- echo "购物网站" > /app/code/buy/index.html
- echo "admin" > /app/code/buy/admin/index.html

- 本地测试
- curl -H buy.fighting.top
- curl -H buy.fighting.top/admin/


---


- 进阶（设置缓存时间）：
	
	- 位置：
	- buy.fighting.top/video
	- buy.fighting.top/image/image.1

- 修改配置:
	- vim /etc/nginx/conf.d/buy.fighting.top.conf
```
server {
	listen 80;
	server_name buy.fighting.top;
	root /app/code/buy;
	location / {
	index index.html;
	}
	location */ \.(html|css|js)${
		expires max;           #一天
	}
	location */ \.(jpg|png|jpeg|gif|bmp)${
		expires 1h;            #1小时
	}
}
```

- 设置完打开网站，F12检查 “control cache”



- location 规则：
```
 location / {xxx}                         默认规则（保底，当其他规则失效时）
 location /image/ {xxx}                   用于匹配 uri（路径）
 location ~ \.(jpg|jpeg)${xxx}            支持正则，区分大小写
 location ~* \.(jpg|jpeg)${xxx}           支持正则，不区分大小写
```

- location 正则优先级：
```
	1           =
	
	2          ^~
	
	3          ~~*
	
	4         /image/
	
	5          /
```

---

- 用户访问流程：


	- Client == > Web ==> Nginx ==> conf.d ==> server_name/response ==> default.conf--server_name ==> buy1.conf--server_name


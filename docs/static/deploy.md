# 部署发布

##### 一、首先在服务器上安装nginx环境，网上有很多指南，可直接在百度搜索。

##### 二、将处理好的markdown静态页面整个目录复制到服务器上，具体复制方法请直接在百度搜索。

##### 三、打开nginx的配置文件，增加新的配置
```json
server {
    listen         80;
    server_name    localhost;

    location / {
        root 静态网页所在目录;
        index index.html index.htm;
        try_files $uri $uri/ @router;
        index index.html;
    }

    location @router {
        rewrite ^.*$ /index.html last;
    }
}
```

##### 四、重启启动nginx
```shell
nginx -s reload
```


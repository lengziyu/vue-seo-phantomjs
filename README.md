# vue seo phantomjs方案

## 安装

本地phantomjs安装：http://npm.taobao.org/dist/phantomjs/

配置环境变量，以windows为例，如你放在`D:\software\phantomjs-2.1.1-windows`目录下，
设置环境变量：控制面板->系统和安全->系统->高级系统设置->高级->系统变量->编辑变量Path->将`D:\software\phantomjs-2.1.1-windows\bin`添加到最末端即可（cmd命令要重新打开窗口才生效）。

执行
```
$ phantomjs -v
```
输出版本号说明成功了。


```
# 克隆项目到本地
$ git clone https://github.com/lengziyu/vue-seo-phantomjs.git

# 安装express
$ cnpm i
```
测试是否可以：
```
$ phantomjs spider.js 'https://www.baidu.com/'
```
打印出一堆html代码就说明成功了。

# 线上部署
请先安装PM2、phantomjs、nodejs，并配置全局环境变量。
```
# 运行
PM2 start server.js
```
nginx配置：
```
upstream spider_server {
  server localhost:8081;
}

server {
    listen       80;
    server_name  example.com;
    
    location / {
      proxy_set_header  Host            $host:$proxy_port;
      proxy_set_header  X-Real-IP       $remote_addr;
      proxy_set_header  X-Forwarded-For $proxy_add_x_forwarded_for;

      if ($http_user_agent ~* "Baiduspider|twitterbot|facebookexternalhit|rogerbot|linkedinbot|embedly|quora link preview|showyoubot|outbrain|pinterest|slackbot|vkShare|W3C_Validator|bingbot|Sosospider|Sogou Pic Spider|Googlebot|360Spider") {
        proxy_pass  http://spider_server;
      }
    }
}
```

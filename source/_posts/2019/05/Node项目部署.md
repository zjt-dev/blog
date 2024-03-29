---
title: Node项目部署
date: 2019-05-12 14:43:37
tags: Nginx
categories: Nginx
---

## 利用宝塔可以很快的搭建你需要的环境
安装 ：[https://www.bt.cn/bbs/thread-19376-1-1.html](https://www.bt.cn/bbs/thread-19376-1-1.html)

## 上传文件
将你的项目上传到指定的目录

## 添加网站

![](https://img2020.cnblogs.com/blog/1524685/202006/1524685-20200601100634195-561242539.png)

## 安装pm2管理器，方便管理你的Node项目和版本
PM2是Node.js应用程序的生产流程管理器，内置负载均衡。它可以帮助您保持Node应用程序永久活动，重起这些node应用程序也不需要停机，并简化常见的系统管理任务。
填写项目跟目录和启动文件,启动你的node服务，也可以自己安装`forever`
![](https://img2020.cnblogs.com/blog/1524685/202006/1524685-20200601100016539-687074251.png)

## 设置nginx反向代理
执行完以上步骤，你会发现并不能正常访问该项目，这时需要用到**[nginx的反向代理](/2019/06/01/正向代理和反向代理/)**
打开 网站——>设置——>配置文件
在 `server`中添加一个配置
```
location /
    {
      proxy_pass  http://127.0.0.1:3000;
    }
```
注意格式，在宝塔中
```
location /{
      proxy_pass  http://127.0.0.1:3000;
    }
```
这样配置会报错

### 配置完成后就可以用你配置的网站名访问了，如果有配置端口号，别忘了加




# 全面升级

前端部分所使用的技术和框架版本很久没有升级了，最近vue-cli3发布了，同时最近想尝试一下尤大推荐的vuetify.js组件库，
要求node版本大于8.9,索性把目前用到的工具进行一次大升级。

至于和老版本项目的兼容问题。。。装了以后再说吧。

## 升级node.js

npm中有一个模块叫做“n”，专门用来管理node.js版本的。

更新到最新的稳定版只需要在命令行中打下如下代码：

```
npm install -g n
n stable
```

如需最新版本则用`n latest`

当然，n后面也可以跟具体的版本号：n v6.2.0

## 升级npm

npm -g install npm@next

## vue-cli 2 卸载

有坑，忘记当时怎么安装的了，应该是使用cnpm安装的，所以也要用cnpm删除。

正规全局卸载方法`npm uninstall vue-cli -g`

用 cnpm 卸载时把 npm 换成 cnpm

## Vue CLI 3 安装

```
npm install -g @vue/cli
```

一句话完事了。

安装完检查一下版本`vue --version`
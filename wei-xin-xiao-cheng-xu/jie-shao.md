# 介绍

参考：[https://developers.weixin.qq.com/miniprogram/dev/quickstart/basic/file.html](https://developers.weixin.qq.com/miniprogram/dev/quickstart/basic/file.html)

## Json配置

#### 小程序配置app.json

app.json 是对当前小程序的全局配置，包括了小程序的所有页面路径、界面表现、网络超时时间、底部 tab 等

### 工具配置 project.config.json

针对编辑器的一些个性化配置

### 页面配置 page.json

独立定义每个页面的一些属性，比如顶部颜色，时候允许下拉刷新等。

## WXML模板

wxml 类似与html。定义了很多组件

MVVM的开发模式（如react,vue），渲染与逻辑分离。

提供wx：开头的属性。

## WXSS样式

对应css。具有CSS的大部分特性。

## JS 交互逻辑

响应用户点击，获取用户位置，调用微信SDK....


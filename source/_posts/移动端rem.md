---
title: 移动端rem
date: 2021-11-15 15:34:16
tags: 
  - 移动端
  - vue
categories: 前端
---

## 推荐使用以下两个工具
+ postcss-pxtorem  
    + 是一款postcss插件，用于将单位转化为 rem  
+ lib-flexible
    + 用于设置 rem 基准值

## 使用 lib-flexible 动态设置REM基准值（html标签的字体大小）
1. 安装
    ``` bash
    yarn add amfe-flexible
    # 或
    npm i amfe-flexible
    ```
2. 在 main.js 中加载执行该模块

    ``` bash
    import 'amfe-flexible'
    
    ```
3. 测试：  
    **在浏览器中切换不同的手机设备尺寸，观察 html 标签 font-size 的变化。**
    + 例如在 iPhone 6/7/8 设备下，html 标签字体大小为 37.5 px
    + 例如在 iPhone 6/7/8 Plus 设备下，html 标签字体大小为 41.4 px

## 使用 postcss-pxtorem 将 px 转为 rem
> 注意  
    **<font color="red">该插件不能转换行内样式中的 px，例如 `<div style="width: 200px;"></div>`</font>**  
    **<font color="red">postcss-pxtorem扫描的是css文件，要转换的class样式设置在当前vue文件转换是不会生效的</font>**
1. 安装
    ``` bash
    yarn add -D postcss-pxtorem
    # 或
    npm install postcss-pxtorem -D
    ```
2. 在vue.config.js中配置postcss-pxtorem

``` vue
module.exports = {
  lintOnSave: true,
  css: {
    loaderOptions: {
      postcss: {
        plugins: [
          //在这里配置postcss-pxtorem
          require('postcss-pxtorem')({
            rootValue({ file }) {
              //如果是 Vant 的样式，就把 rootValue 设置为 37.5 来转换
              //如果是我们的样式，就按照 75 的 rootValue 来转换
              return file.indexOf('vant') !== -1 ? 37.5 : 75
            },
            // 例如 * 就是所有属性都要转换， width 就是仅转换 width 属性
            propList: ['*']
          }),
        ]

      }
    }
  },
}
```
3. 配置完毕，重新启动服务
最后测试：刷新浏览器页面，审查元素的样式查看是否已将 px 转换为 rem 。
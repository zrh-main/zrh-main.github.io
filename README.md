# 简介
Hexo 是一个快速、简洁且高效的博客框架。[Hexo官网](https://hexo.io/zh-cn/docs/)
Hexo 使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。
## 目录结构说明
* package.json
    > 应用程序的信息
* scaffolds	
    > 模版文件夹  
    > 新建文章时，Hexo 会根据 scaffold 来建立文件;
    > Hexo的模板是指在新建的文章文件中默认填充的内容
* source		
    > 资源文件夹是存放用户资源的地方。
    > 除_posts 文件夹之外，开头命名为 _ (下划线)的文件 / 文件夹和隐藏的文件将会被忽略。
    > Markdown 和 HTML 文件会被解析并放到public 文件夹，而其他文件会被拷贝过去
* themes	
    > 主题文件夹。Hexo 会根据主题来生成静态页面
* _config.yml
    > 配置文件
## 常用操作

### 安装
```
npm install -g hexo-cli
```
### 新建Hexo项目
```
hexo init 项目名称
cd 项目名称
npm install
```
### 新建文章
```
hexo new [layout] <title>
```

<font size="2px">
如果没有设置layout 的话，默认使用 _config.yml 中的default_layout 参数代替  
如果标题包含空格，使用引号括起来  
新建的文章会存放在source文件夹下生成静态文件
</font>

### 生成静态文件
```
hexo generate
```
### 发表草稿
```
hexo publish [layout] <filename>
```
### 启动服务器
```
hexo serve
```
### 部署网站
```
hexo deploy
```
### 渲染文件
```
hexo render <file1> [file2] ...
```
### 从其他博客系统
```
hexo migrate <type>
```
### 清除缓存文件 (db.json) 和已生成的静态文件 (public)
```
hexo clean
```
### 列出网站资料
```
hexo list <type>
```
### 显示 Hexo 版本
```
hexo version
```
### 显示草稿
```
hexo --draft
```
### 自定义当前工作目录（Current working directory）的路径。
```
hexo --cwd /path/to/cwd
```

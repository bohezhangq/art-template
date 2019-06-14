# art-template

[![NPM Version](https://img.shields.io/npm/v/art-template.svg)](https://npmjs.org/package/art-template)
[![NPM Downloads](http://img.shields.io/npm/dm/art-template.svg)](https://npmjs.org/package/art-template)
[![Node.js Version](https://img.shields.io/node/v/art-template.svg)](http://nodejs.org/download/)
[![Travis-ci](https://travis-ci.org/aui/art-template.svg?branch=master)](https://travis-ci.org/aui/art-template)
[![Coverage Status](https://coveralls.io/repos/github/aui/art-template/badge.svg?branch=master)](https://coveralls.io/github/aui/art-template?branch=master)

[English document](https://aui.github.io/art-template/) | [中文文档](https://aui.github.io/art-template/zh-cn/index.html)

art-template is a simple and superfast templating engine that optimizes template rendering speed by scope pre-declared technique, hence achieving runtime performance which is close to the limits of JavaScript. At the same time, it supports both NodeJS and browser. [speed test online](https://aui.github.io/art-template/rendering-test/).

art-template 是一个简约、超快的模板引擎。它采用作用域预声明的技术来优化模板渲染速度，从而获得接近 JavaScript 极限的运行性能，并且同时支持 NodeJS 和浏览器。[在线速度测试](https://aui.github.io/art-template/rendering-test/)。

[![chart](https://aui.github.io/art-template/images/chart@2x.png)](https://aui.github.io/art-template/rendering-test/)

## Feature

1. performance is close to the JavaScript rendering limits
2. debugging friendly. Syntax errors or runtime errors will be positioned accurately at which line of template. Support setting breakpoint in templating files (Webpack Loader)
3. support Express, Koa, Webpack
4. support template inheritance and sub template
5. browser version is only 6KB

## 特性

1. 拥有接近 JavaScript 渲染极限的的性能
2. 调试友好：语法、运行时错误日志精确到模板所在行；支持在模板文件上打断点（Webpack Loader）
5. 支持 Express、Koa、Webpack
6. 支持模板继承与子模板
7. 浏览器版本仅 6KB 大小

<br>

## 快速入门

### 浏览器

    <script id="tpl-user" type="text/html">
    <% if (user) { %>
      <h2><%= user.name %></h2>
    <% } %>
    </script>

    <script src="art-template/lib/template.js"></script>
    <script>
    var html = template('tpl-user', {
        user: {
            name: 'aui'
        }
    });
    </script>  
    
### 核心方法

    //基于模板名称模板（文件名，数据）渲染模板
     template(filename, data);

    //将模板源编译为函数
     template.compile（source，options）;

    //将模板源代码编译为函数并立即调用
     template.render（source，data，options）;

### 语法

art-template同时支持模板的两种语法。`{{expression}}` 简约语法与任意 JavaScript 表达式` <% expression %>`

#### 标准语法

    {{if user}} 
        < h2 > 
            {{user.name}} 
        </ h2 > 
    {{/ if}}
    
#### 原始语法

    < ％ if（user）{％> 
        < h2 > 
            < ％= user.name％> 
        </ h2 > 
    < ％ }％>
  
#### 标准产量（输出）

    {{value}} 
    
    <%= value %>
    
#### 原始产量（输出）
    
    {{@value}}
    
    < ％ -  值％>

#### 条件

    {{if value}} ... {{/ if}} 
    {{if v1}} ... {{else if v2}} ... {{/ if}}  
    
<br>

    < ％ if（value）{％> ... < ％ }％> 
    < ％ if（v1）{％> ... < ％ } 否则 if（v2）{％> ... < ％ }％>

#### 循环
    
    {{each list}}
        {{$index}} {{$value}}
    {{/each}}  

<br>

    <％for（var i = 0; i <target.length; i ++）{％> < ％= i％> < ％= target [ i ]％> < ％ }％>
    
<br>
1. `target`支持的迭代`array`和`object`，它的默认值是`$data`。<br>  
2. `$value`并且`$index`可以定制：`{{each target val key}}`。

#### 变量

    {{set temp = data.sub.content}}
    
<br>

    < ％ var  temp = data.sub.content; ％>
    
#### 子模板

    {{include'./header.art'}} 
    {{include'./header.art'data}}
    
`include `第二个参数默认值为 `$data`，可以自定义。
<br>

    < ％ 包括（ ' / header.art '）％> 
    < ％ 包括（' 。 / header.art '，数据）％>
    
1. data值是`$data`默认值。标准语法不支持声明`object`和`array`引用变量。但是，原始语法没有限制。<br>
2. art-template有内置的HTML minifier，请避免在子模板中编写异常的结束标记。否则，当minimize选项打开时，标签可能会意外“优化” 。


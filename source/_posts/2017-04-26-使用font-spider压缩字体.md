---
title: 使用font-spider压缩字体
date: 2017-04-26 19:00:44
layout: post
comments: true
categories: js
tags: [font-face,font-spider]
keywords: font,spider,font-face
description: font-spider按需压缩字体文件
---
## 引言
>字蛛是一个 WebFont 智能压缩工具，它能自动化分析页面中所使用的 WebFont 并进行按需压缩，通常好几 MB 的中文字体可以被压缩成几 KB 大小。 
>[font-spider](http://font-spider.org)

## @font-face
在前端开发中,经常会用到字体文件,比如常见的ttf、woff2、woff、eot、svg等格式。
有时候设计师出的图中用到的字体不是web安全字体,系统中不存在,然而程序员又不想通过切图这种lowB的方法实现,就得用到CSS3 @font-face定制相应的字体。
### Formal syntax

```css
    @font-face {
      [ font-family: <family-name>; ] ||
      [ src: [ <url> [ format(<string>#) ]? | <font-face-name> ]#; ] ||
      [ unicode-range: <urange>#; ] ||
      [ font-variant: <font-variant>; ] ||
      [ font-feature-settings: normal | <feature-tag-value>#; ] ||
      [ font-stretch: <font-stretch>; ] ||
      [ font-weight: <weight>; ] ||
      [ font-style: <style>; ]
    }
```
### Example
抄袭一下[大漠的示例](https://www.w3cplus.com/content/css3-font-face)
```css
    @font-face {
    	font-family: 'YourWebFontName';
    	src: url('YourWebFontName.eot'); /* IE9 Compat Modes */
    	src: url('YourWebFontName.eot?#iefix') format('embedded-opentype'), /* IE6-IE8 */
                 url('YourWebFontName.woff') format('woff'), /* Modern Browsers */
                 url('YourWebFontName.ttf')  format('truetype'), /* Safari, Android, iOS */
                 url('YourWebFontName.svg#YourWebFontName') format('svg'); /* Legacy iOS */
       }
```
Firefox、Chrome、Safari 以及 Opera 支持 .ttf (True Type Fonts) 和 .otf (OpenType Fonts) 类型的字体。
Internet Explorer 9+ 支持新的 @font-face 规则，但是仅支持 .eot 类型的字体 (Embedded OpenType)。
**注释：** Internet Explorer 8 以及更早的版本不支持新的 @font-face 规则。
据[大漠](https://www.w3cplus.com/content/css3-font-face)的文章说@font-face规则 IE4 就已经支持了,我又查了一下发现果然。
>'@font-face' 规则首先定义在 CSS2 规范中，但是在 CSS2.1 中被删除，目前又被纳入到 CSS3 推荐草案中。at-rule（@）包含一条或多条被称作 font descriptor （字体描述）的特性声明，与那些在常规的 CSS 规则中的类似。由于 '@font-face' 规则没有被纳入到目前应用最广泛的 CSS2.1 规范中，各浏览器虽然对此规则均支持，但支持的细节有所区别。
>IE 从 4.0 开始支持 '@font-face' ，使用了 Embedded OpenType (EOT) 格式。借助微软官方提供的字体压缩工具 'Microsoft WEFT(Web Embedding Fonts Tool)' 可以将 OpenType TT(.ttf) 格式的字体压缩至较小的体积，压缩后的类型即 .eot 格式。.eot 格式的字体仅被 IE 所支持。而到目前正式发布的最新版本的 IE8 也仍然仅支持 .eot 格式的字体。

## font-spider
最近我在项目中做了个营销页面,一共13页,设计师给出的图中用到了造字工房力黑和方正中黑两种字体,把字体都切成图片肯定不现实,这样用到的图片多而且体积大,肯定影响效率。所以我就引入了这两种字体的ttf,但是大家知道中文字体的格式很大,力黑字体3.6M、中黑字体2M,整个页面加载需要20秒左右,这就要了亲命了。
引入两种字体文件
```css
        @font-face {
            font-family: lihei;
            src: url(./fonts/mflh.ttf);
            font-weight: 400;
            font-style: normal
        }

        @font-face {
            font-family: fzzh;
            src: url(./fonts/fzzh.ttf);
            font-weight: 400;
            font-style: normal
        }
```
如果不能压缩字体文件,这个方案就得放弃,好在GitHub上找到了[font-spider](https://github.com/aui/font-spider)这样好用的工具可以按需压缩字体,成功的让我用到的两种字体瘦身到28k、6k。
```bash

    font-spider index.html
    
    Font family: lihei
    Original size: 3574.944 KB
    Include chars: 1234S下业中产介代优伙伴作信入公共分创功加势厂发司合咨品四回团国场大好审小展州巨市店成携政未来松核模步汽渠牌理神空策简级绍美者融规访询贷赢车轻道配金销闪间集额！
    Chars length: 82
    Font id: e583df40b8412c80822e7c7f5173e410
    CSS selectors: .title-top, .title-number, .page-1 .title, .page-2 .title, .page-3 .advantage-li--title, .partner-desc, .progress-title, .corner:before, .corner:after
    Font files:
    File fonts/mflh.ttf created: 28.94 KB
    
    Font family: fzzh
    Original size: 2037.116 KB
    Include chars: ()01489:产低作合品息按揭期热短线能贷高
    Chars length: 23
    Font id: c2b90772877e464bb783b3fed03b39ab
    CSS selectors: .prod-title, .hotline
    Font files:
    File fonts/fzzh.ttf created: 5.988 KB
```
实际上我分别只用到了几十个字符而已,font-spider会自动化分析页面中所使用的 WebFont 并进行按需压缩,删掉我没用到的字符。
font-spider实际上是通过爬虫实现的,通过虚拟浏览器技术加载解析HTML和CSS、操作样式语法树、查找字体,具体请看作者的wiki [WebFont 智能压缩工具——字蛛 1.0.0 正式版发布-新增图标字体压缩](https://github.com/aui/font-spider/issues/79)。

可惜的是目前font-spider只支持静态页面.html,还不支持各种模板。由于目前我开发所采用的是VUE,没办法把font-spider加到webpack工作流中去,所以无奈我使用的办法是把渲染好的页面重新再构建成一个新的html,然后通过命令压缩字体,然后再把压缩后的字体导回到项目中,尽管这种办法很恶心,很期待下个版本的font-spider支持动态模板的解析并能有webpack插件。

## 引用
>[CSS3 @font-face  https://www.w3cplus.com/content/css3-font-face
>RF1001: 各浏览器对 '@font-face' 规则支持的字体格式不同，IE 支持 EOT 字体，Firefox Safari Opera 支持 TrueType 等字体  http://w3help.org/zh-cn/causes/RF1001
>字蛛  https://github.com/aui/font-spider/blob/master/README-ZH-CN.md

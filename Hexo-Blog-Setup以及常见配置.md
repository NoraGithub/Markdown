---
title: Hexo Blog Setup以及常见配置
date: 2016-11-24 09:08:34
tags:
keywords: "hexo, 定制化, 结构"
render_drafts: true
layout: draft
---

使用Hexo进行了Blog的Setup后，我们希望选择一个心仪的theme（即不同页面类型保持一致的风格），配置需要的功能选项/属性，甚至进行一些个性化的定制。
<!--more-->
如何预览和使用心仪的theme，Google后有足够的信息，这里就不详细介绍了。Hexo默认theme是landscape。网络上也有不少对默认主题的自定义配置方案，例如，[hexo的私人订制](http://blog.sunnyxx.com/2014/03/07/hexo_customize/)
Simple is the best，毕竟不是搞设计和前端开发的。个人选择了Next作为theme，一方面偏好它的极简主义，另一方面，整体设计的留白和交互都很不错。

theme本身很优秀，但我们还是有一些个性化的需求，需要了解Hexo的更详细配置（对应next下的主题级别的[配置](http://theme-next.iissnan.com/)），或，简单地修改脚本。例如：
- post granularity的SEO
- post granularity的tracking & conversation tracking
- 隐藏部分post，不按timeline呈现，按特定方式访问
- 设置404 page
- 新增简历 page
- 修改搜索的UI（未完成）
- ...

大部分信息网络页都有，利用这篇文章简单总结，并探索Hexo & Next theme的架构。

# 简介

## Hexo（site） 配置

Hexo级别的[配置](https://hexo.io/zh-cn/docs/configuration.html) 关于站点/项目级别的内容、样式属性。如网站标题、网站描述、网站的SEO关键词、项目Github地址等。一般是内容属性。
类似meta类。

## theme 配置

theme级别的[配置](http://theme-next.iissnan.com/)已经对基础设置，如，如何切换theme，更新profie、social link等，做了比较详细的介绍。主要是样式属性。

就像html+css把内容、样式分离开来一样，theme负责Hexo的默认展现（样式）。
除了配置外信息一般存放于./source目录

# 进阶
theme级别的[配置](http://theme-next.iissnan.com/)有部分用户路径是没有考虑，如404页面，搜索等。

## Hexo的布局（layout）

### 默认布局（样式）-post
layout（布局）概念类似一种页面类型，（默认情况下，但是如何新建一个type？新的渲染格式？）Hexo有三种不同的layout，不同layout保存的路径并不一样。

|Layout|Path|
|:--:|:--|
|post|./source/_post
|page|./source
|draft|./source/_draft

1. 不同layout的唯一区别是保存到不同的路径（to some extent）
2. layout支持自定义，存放目录和post一样，当scaffold无自定义类型时，调用默认layout
3. hexo publish 可用于发表草稿（draft-->post，参考1）
4. hexo new page


可以利用layout组织template

|template|Page|Fallback|Path|Fallback Path
|:--:|:--:|:--:|
|index|Home page||
|post|Posts|index|_post|
|page|Pages|index|source|source/%pagename
|archive|Archives|index|
|category|Category archives|archive|
|tag|Tag archives|archive|

Fallback位于类似于scaffolds，只是定义了变量，
不同template的渲染文件位于./themes/next/layout/*下，利用swig组成，swig是一个js模版引擎

/source/_其它（page？）
[reference-写作](https://hexo.io/zh-cn/docs/writing.html)


hexo new draft ..
其中..d的layout是post，并非draft，有点坑啊。。。
在scaffolds中国年增加变量layout: {{ layout}}解决该问题，然而历史的的md都会被认为是默认的layout：post，需手动添加layout: draft补充
在post.swig和post-collapse的 <h1>标签增加
        {# added by Nora, for draft only #}
        {% if post.layout === "draft" %}
        <font color=#FF4500 size=1> 撰写中</font>
        {% endif %}
用于提醒。

## scaffold自定义模版（样式）

scaffolds/%layout
scaffolds决定不同layout的变量，在initial一个article时出现的变量，不同的layout变量使用不同的template和不同的path
template上述内容的展现方式
切换布局

*如果要显示草稿，页面级别设置是没用的，只能全站级别*
**希望修改为页面级别，而且修改样式提醒为草稿以及首页显示后提醒草稿样式**

## 设置404页面
我们希望404
## 新建页面
新建page和新建post是不一样的，新建page利用`hexo new page %pagename`，创建后会有%pagename文件夹，文件夹有index.md，md内容为该页面调用内容。

新建博客利用`hexo new %postname`创建后有%blogname.md为名的post文件，存放于source/_post

## 设置about
同理
# 设置rss
感觉rss是一个模版粒度的设置，换了模版就有必要重新设置（如果有需要）
[显示feed链接](https://github.com/iissnan/hexo-theme-next/wiki/%E6%98%BE%E7%A4%BA-feed-%E9%93%BE%E6%8E%A5)
# 设置分享插件（duoshuo）
# 设置站内搜索
利用Next主题的local search插件的时候，会出现一个特别的情况：
当你在yoursite.com/page1做search的时候，点击搜索结果会出现异常：
会navigate到yoursite.com/page1/%search_result_relativePath
出现这个问题的主要原因是站点配置/path_your_blog/source/_cofig.yml的url值配置问题，改为absolutePath可以杜绝这个情况。
即，
`yoursite.com`
`http://yoursite.com/`
其中最后的斜杠`/`不能遗漏，会导致转义出现问题
## encode& decode
根据http协议，URL中的部分字符会进行转义（encode），例如中文字符，会遇见的一个问题是把`/`转为`%2F`进行页面访问。
检查yoursite.com/search.xml可以发现`npm install hexo-generator-searchdb --save`会根据文章简历索引，并编码后访问
[#reference:]()
[#reference: HTML URL 编码](http://www.w3school.com.cn/tags/html_ref_urlencode.html)

# 文章配置
update time
# SEO优化设置
## keywords
页面级别配置
## 友链
## sitetracking
## 出现在搜索引擎
# Favicon
http://ww2.sinaimg.cn/mw690/6fa34428jw8e6sgfwn3suj20c80afmxk.jpg
# 反向代理
 Github 访问速度太慢，有没有通过反向代理或者CDN加速的方式提高访问速度？特别是如果更新DNS的情况下

经过调研，最简单快速的方法是利用coding.net（墙内版Github）的pages服务设置hook自动更新 
# 可能有的问题
https协议的支持
seo支持
自定义监测支持
[提示有编码错误](https://github.com/PaicHyperionDev/hexo-generator-search/issues/8)
## 不支持所有页面进行local search
当对配置中 search- field从`post`修改为`all`，会提醒post is not defined 


updatetime function

# 定制化（进阶）

[结构](https://hexo.io/zh-cn/docs/templates.html)

为了进行个性化定制，先定义一些主要的数据类型：
页面（类型），如首页，归档页，标签页等：page
页面：默认（default）post

我们页了解下Hexo框架下，页面（html）的生成逻辑。
另外也应该了解同一个选项，hexo的
配置的优先级：page>theme>hexo
从html角度进行个性化定制

我们来看看hexo 的名录以及各目录下配置或文件的作用
`tree -L 3  -I node_modules\|public`

```
.
├── _config.yml//站点配置
├── db.json
├── debug.log
├── package.json
├── scaffolds
│   ├── draft.md//草稿
│   ├── page.md//页面
│   └── post.md//博客
├── source
│   ├── 404 //404页面
│   │   └── index.md
│   ├── _posts //博客目录
│   │   ├── Hexo-Blog-Setup以?\217\212常?\201?\205\215置.md
│   │   ├── Markdown\ Web\ \ ?\226?\221?\231?产?\223\201?\203?\224.md
│   │   ├── README.md
│   │   ├── ..
│   │   ├── ..
│   │   
│   ├── about //关于页面
│   │   └── index.md
│   └── tags //tags页面
│       └── index.md
└── themes//模版信息
    ├── landscape //landscape模版脚本
    │   ├── Gruntfile.js
    │   ├── LICENSE
    │   ├── README.md
    │   ├── _config.yml
    │   ├── languages
    │   ├── layout
    │   ├── package.json
    │   ├── scripts
    │   └── source
    └── next //next模版脚本
        ├── README.en.md
        ├── README.md
        ├── _config.yml//模版配置
        ├── bower.json
        ├── gulpfile.coffee
        ├── languages
        ├── layout
        ├── package.json
        ├── scripts
        ├── source
        └── test//测试
```
我们进到next模版目录详细研究
`tree -I test`

```
.
├── README.en.md
├── README.md
├── _config.yml
├── bower.json
├── gulpfile.coffee
├── languages//语言包
│   ├── zh-Hans.yml
│   ├── default.yml
│   ├── en.yml
│   ├── zh-hk.yml
│   ├── ..
│   └── ..
│   
├── layout//layout控制
│   ├── _layout.swig//layout javascript 模版引擎
│   ├── _macro//所有变量
│   │   ├── post-collapse.swig
│   │   ├── post.swig
│   │   ├── reward.swig
│   │   ├── sidebar.swig
│   │   └── wechat-subscriber.swig
│   ├── _partials
│   │   ├── comments.swig
│   │   ├── duoshuo-hot-articles.swig
│   │   ├── footer.swig
│   │   ├── head
│   │   │   └── external-fonts.swig
│   │   ├── head.swig
│   │   ├── header.swig
│   │   ├── pagination.swig
│   │   ├── search
│   │   │   ├── localsearch.swig
│   │   │   ├── swiftype.swig
│   │   │   └── tinysou.swig
│   │   ├── search.swig
│   │   └── share
│   │       ├── add-this.swig
│   │       ├── baidushare.swig
│   │       ├── duoshuo_share.swig
│   │       └── jiathis.swig
│   ├── _scripts
│   │   ├── baidu-push.swig
│   │   ├── boostrap.swig
│   │   ├── commons.swig
│   │   ├── pages
│   │   │   └── post-details.swig
│   │   ├── schemes
│   │   │   ├── mist.swig
│   │   │   ├── muse.swig
│   │   │   └── pisces.swig
│   │   ├── third-party
│   │   │   ├── analytics
│   │   │   │   ├── baidu-analytics.swig
│   │   │   │   ├── busuanzi-counter.swig
│   │   │   │   ├── cnzz-analytics.swig
│   │   │   │   ├── facebook-sdk.swig
│   │   │   │   ├── google-analytics.swig
│   │   │   │   └── tencent-analytics.swig
│   │   │   ├── analytics.swig
│   │   │   ├── comments
│   │   │   │   ├── disqus.swig
│   │   │   │   └── duoshuo.swig
│   │   │   ├── comments.swig
│   │   │   ├── lean-analytics.swig
│   │   │   ├── localsearch.swig
│   │   │   ├── mathjax.swig
│   │   │   └── tinysou.swig
│   │   └── vendors.swig
│   ├── archive.swig
│   ├── category.swig
│   ├── index.swig
│   ├── page.swig
│   ├── post.swig
│   └── tag.swig
├── package.json
├── scripts
│   ├── merge-configs.js
│   └── tags
│       ├── center-quote.js
│       ├── full-image.js
│       └── group-pictures.js
└── source
    ├── 404.html
    ├── css
    │   ├── _common
    │   │   ├── components
    │   │   │   ├── back-to-top.styl
    │   │   │   ├── buttons.styl
    │   │   │   ├── comments.styl
    │   │   │   ├── components.styl
    │   │   │   ├── footer
    │   │   │   │   └── footer.styl
    │   │   │   ├── header
    │   │   │   │   ├── header.styl
    │   │   │   │   ├── headerband.styl
    │   │   │   │   ├── menu.styl
    │   │   │   │   ├── site-meta.styl
    │   │   │   │   └── site-nav.styl
    │   │   │   ├── highlight
    │   │   │   │   ├── highlight.styl
    │   │   │   │   └── theme.styl
    │   │   │   ├── pages
    │   │   │   │   ├── archive.styl
    │   │   │   │   ├── categories.styl
    │   │   │   │   ├── pages.styl
    │   │   │   │   └── post-detail.styl
    │   │   │   ├── pagination.styl
    │   │   │   ├── post
    │   │   │   │   ├── post-collapse.styl
    │   │   │   │   ├── post-eof.styl
    │   │   │   │   ├── post-expand.styl
    │   │   │   │   ├── post-gallery.styl
    │   │   │   │   ├── post-meta.styl
    │   │   │   │   ├── post-more-link.styl
    │   │   │   │   ├── post-nav.styl
    │   │   │   │   ├── post-reward.styl
    │   │   │   │   ├── post-tags.styl
    │   │   │   │   ├── post-title.styl
    │   │   │   │   ├── post-type.styl
    │   │   │   │   └── post.styl
    │   │   │   ├── sidebar
    │   │   │   │   ├── sidebar-author-links.styl
    │   │   │   │   ├── sidebar-author.styl
    │   │   │   │   ├── sidebar-blogroll.styl
    │   │   │   │   ├── sidebar-feed-link.styl
    │   │   │   │   ├── sidebar-nav.styl
    │   │   │   │   ├── sidebar-toc.styl
    │   │   │   │   ├── sidebar-toggle.styl
    │   │   │   │   ├── sidebar.styl
    │   │   │   │   └── site-state.styl
    │   │   │   ├── tag-cloud.styl
    │   │   │   ├── tags
    │   │   │   │   ├── blockquote-center.styl
    │   │   │   │   ├── full-image.styl
    │   │   │   │   ├── group-pictures.styl
    │   │   │   │   └── tags.styl
    │   │   │   └── third-party
    │   │   │       ├── baidushare.styl
    │   │   │       ├── busuanzi-counter.styl
    │   │   │       ├── duoshuo.styl
    │   │   │       ├── jiathis.styl
    │   │   │       ├── localsearch.styl
    │   │   │       └── third-party.styl
    │   │   ├── outline
    │   │   │   └── outline.styl
    │   │   └── scaffolding
    │   │       ├── base.styl
    │   │       ├── helpers.styl
    │   │       ├── normalize.styl
    │   │       ├── scaffolding.styl
    │   │       └── tables.styl
    │   ├── _custom
    │   │   └── custom.styl
    │   ├── _mixins
    │   │   ├── Mist.styl
    │   │   ├── Muse.styl
    │   │   ├── Pisces.styl
    │   │   ├── base.styl
    │   │   └── custom.styl
    │   ├── _schemes
    │   │   ├── Mist
    │   │   │   ├── _base.styl
    │   │   │   ├── _header.styl
    │   │   │   ├── _logo.styl
    │   │   │   ├── _menu.styl
    │   │   │   ├── _posts-expanded.styl
    │   │   │   ├── _search.styl
    │   │   │   ├── index.styl
    │   │   │   ├── outline
    │   │   │   │   └── outline.styl
    │   │   │   └── sidebar
    │   │   │       └── sidebar-blogroll.styl
    │   │   ├── Muse
    │   │   │   ├── _layout.styl
    │   │   │   ├── _logo.styl
    │   │   │   ├── _menu.styl
    │   │   │   ├── _search.styl
    │   │   │   ├── index.styl
    │   │   │   └── sidebar
    │   │   │       └── sidebar-blogroll.styl
    │   │   └── Pisces
    │   │       ├── _brand.styl
    │   │       ├── _full-image.styl
    │   │       ├── _layout.styl
    │   │       ├── _menu.styl
    │   │       ├── _posts.styl
    │   │       ├── _sidebar.styl
    │   │       └── index.styl
    │   ├── _variables
    │   │   ├── Mist.styl
    │   │   ├── Muse.styl
    │   │   ├── Pisces.styl
    │   │   ├── base.styl
    │   │   └── custom.styl
    │   └── main.styl
    ├── fonts
    ├── images
    │   ├── avatar.gif
    │   ├── cc-by-nc-nd.svg
    │   ├── cc-by-nc-sa.svg
    │   ├── cc-by-nc.svg
    │   ├── cc-by-nd.svg
    │   ├── cc-by-sa.svg
    │   ├── cc-by.svg
    │   ├── cc-zero.svg
    │   ├── loading.gif
    │   ├── placeholder.gif
    │   ├── quote-l.svg
    │   ├── quote-r.svg
    │   └── searchicon.png
    ├── js
    │   └── src
    │       ├── affix.js
    │       ├── bootstrap.js
    │       ├── hook-duoshuo.js
    │       ├── motion.js
    │       ├── post-details.js
    │       ├── schemes
    │       │   └── pisces.js
    │       ├── scrollspy.js
    │       └── utils.js
    └── vendors
        ├── fancybox
        │   └── source
        │       ├── blank.gif
        │       ├── fancybox_loading.gif
        │       ├── fancybox_loading@2x.gif
        │       ├── fancybox_overlay.png
        │       ├── fancybox_sprite.png
        │       ├── fancybox_sprite@2x.png
        │       ├── helpers
        │       │   ├── fancybox_buttons.png
        │       │   ├── jquery.fancybox-buttons.css
        │       │   ├── jquery.fancybox-buttons.js
        │       │   ├── jquery.fancybox-media.js
        │       │   ├── jquery.fancybox-thumbs.css
        │       │   └── jquery.fancybox-thumbs.js
        │       ├── jquery.fancybox.css
        │       ├── jquery.fancybox.js
        │       └── jquery.fancybox.pack.js
        ├── fastclick
        │   ├── LICENSE
        │   ├── README.md
        │   ├── bower.json
        │   └── lib
        │       ├── fastclick.js
        │       └── fastclick.min.js
        ├── font-awesome
        │   ├── HELP-US-OUT.txt
        │   ├── bower.json
        │   ├── css
        │   │   ├── font-awesome.css
        │   │   ├── font-awesome.css.map
        │   │   └── font-awesome.min.css
        │   └── fonts
        │       ├── FontAwesome.otf
        │       ├── fontawesome-webfont.eot
        │       ├── fontawesome-webfont.svg
        │       ├── fontawesome-webfont.ttf
        │       ├── fontawesome-webfont.woff
        │       └── fontawesome-webfont.woff2
        ├── jquery
        │   └── index.js
        ├── jquery_lazyload
        │   ├── CONTRIBUTING.md
        │   ├── README.md
        │   ├── bower.json
        │   ├── jquery.lazyload.js
        │   └── jquery.scrollstop.js
        ├── ua-parser-js
        │   └── dist
        │       ├── ua-parser.min.js
        │       └── ua-parser.pack.js
        └── velocity
            ├── bower.json
            ├── velocity.js
            ├── velocity.min.js
            ├── velocity.ui.js
            └── velocity.ui.min.js



```
更新语言包的映射表。
更新_marco/post.swig，用于增加updated time元素，在post增加updated变量
[swig使用指南](http://www.cnblogs.com/elementstorm/p/3142644.html) 
[从源码级别优化hexo next主题](http://www.jianshu.com/p/4d39b6578266)


为了增加page的toc
可以在page.swig里添加逻辑语句进行渲染

```
#默认设置
{% block sidebar %}
  {{ sidebar_template.render(false) }}
{% endblock %}
#}

{#
<!--page增加sidebar控制-->
<!--sidebar不代表toc，toc是sidebar的一部分-->
<!--sidebar_template.render(false)显示非toc部分-->
<!--sidebar_template.render(true)显示toc部分-->
<!--toc和非toc是二选一的关系-->
#}
{% block sidebar %}
 {% if theme.sidebar.display == 'always' %}
  {{ sidebar_template.render(true) }}
 {% else %}
  {{ sidebar_template.render(false) }}
 {% endif %}
{% endblock %}


```

page是所有页面变量？
在%page_name/index.md里增加变量toc: true变量，
在page.swig增加判断逻辑对该变量校验
```
{#
<!--page增加sidebar控制-->
<!--sidebar不代表toc，toc是sidebar的一部分-->
<!--sidebar_template.render(false)显示非toc部分-->
<!--sidebar_template.render(true)显示toc部分-->
<!--toc和非toc是二选一的关系-->

{% block sidebar %}
 {% if theme.sidebar.display == 'always' %}
  {{ sidebar_template.render(true) }}
 {% else %}
  {{ sidebar_template.render(false) }}
 {% endif %}
{% endblock %}
#}

{% block sidebar %}
 {% if page.toc %}
  {{ sidebar_template.render(true) }}
 {% else %}
  {{ sidebar_template.render(false) }}
 {% endif %}
{% endblock %}
```
## 在about增加update时间
## 在about自我简介＋resume链接
增加is_hidden: true属性
首页不显示
```
{% block content %}
  <section id="posts" class="posts-expand">
    {% for post in page.posts %}
    {#test#}
      {% if post.is_hidden != 'true' %}
        {{ post_template.render(post, true) }}
      {% endif %}
    {% endfor %}
  </section>

  {% include '_partials/pagination.swig' %}
{% endblock %}
```

# 自己创建&管理主题



# regenerate
hexo clean
删除db.json



# 支持流程图
npm install hexo-diagram --save
npm install phantomjs  -g
`phantomjs@2.1.7: Package renamed to phantomjs-prebuilt. Please update 'phantomjs' package references to 'phantomjs-prebuilt'`
npm install phantomjs-prebuilt -g
通过sublime插件搜索node_module下所有包涵'phantomjs'语句，替换为'phantomjs-prebuilt'

/Users/NoraChan/Desktop/Blog/node_modules/.bin/esvalidate:
/Users/NoraChan/Desktop/Blog/node_modules/esprima/bin/esvalidate.js:
/Users/NoraChan/Desktop/Blog/node_modules/phantom/phantom.coffee:
/Users/NoraChan/Desktop/Blog/node_modules/phantom/phantom.js:

npm install phantomjs  --save
--save和-g的区别是？（phantomjs-prebuilt）

决定按照insecure方式处理
修改回这些文件
/Users/NoraChan/Desktop/Blog/node_modules/.bin/esvalidate:
/Users/NoraChan/Desktop/Blog/node_modules/esprima/bin/esvalidate.js:
/Users/NoraChan/Desktop/Blog/node_modules/phantom/phantom.coffee:
/Users/NoraChan/Desktop/Blog/node_modules/phantom/phantom.js:



[hexo-diagram 渲染问题](http://www.luohanjie.com/2016-03-22/hexo-support-flowchart.html#fnref2)


流程图支持的markdown语法
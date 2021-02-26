title: Hexo Fluid添加tag-cloud插件
tags:

  - Hexo
  - tag-cloud
  - 前端
categories:
  - 前端
  - Hexo
date: 2021-02-25 20:29:00

index_img: /img/artical_cover/tag_cloud_example.png

banner_img: /img/artical_cover/tag_cloud_example.png

---


## 前言
官方文档：https://github.com/D0n9X1n/hexo-tag-cloud/blob/master/README.ZH.md

在搭建博客的过程中无意间看到某个大佬的博客有很炫酷的动态标签云，瞬间心动了，于是决定自己也安装一个。tag-cloud插件比较特殊，根据官方文档，它需要将一段代码替换原本的标签云。而标签云的代码位置可能会根据主题、模板引擎、插件安装方式的不同而有较大差异。

虽然Fluid不算冷门的主题，但是看了若干技术博客，并没有Fluid+tag-cloud的相关教程，官方文档给出的代码替换方式也和我的实际操作有所不同，故写此篇博客以供参考。

- hexo v5.0
- npm安装所有插件

## 确认模板引擎

通过npm安装主题，主题文件在`node_modules\hexo-theme-fluid\`中，查看代码文件发现后缀都是ejs，因此是ejs引擎。

选取这段代码备用：

```ejs
<% if (site.tags.length) { %>
  <script type="text/javascript" charset="utf-8" src="<%- url_for('/js/tagcloud.js') %>"></script>
  <script type="text/javascript" charset="utf-8" src="<%- url_for('/js/tagcanvas.js') %>"></script>
  <div class="widget-wrap">
    <h3 class="widget-title"><%= __('tagcloud') %></h3>
    <div id="myCanvasContainer" class="widget tagcloud">
      <canvas width="250" height="250" id="resCanvas" style="width:100%">
        <%- tagcloud() %>
      </canvas>
    </div>
  </div>
<% } %>
```

## 确认代码修改位置

打开博客的标签页面，按F12打开控制台进行元素审查，发现标签云是一个`class="text-center tagcloud"`的元素。在工程文件中全局搜索这两个类，最后确定标签云在这个文件中：`node_modules\hexo-theme-fluid\layout\tags.ejs`

打开文件，标签云的代码长这样：

```ejs
<%- tagcloud({
    min_font: min_font,
    max_font: max_font,
    amount: 999,
    unit: unit,
    color: true,
    start_color,
    end_color
  }) %>
```

直接把这段代码注释掉，替换成上一段代码：

```ejs
<div class="text-center tagcloud">
  <!-- <%- tagcloud({
    min_font: min_font,
    max_font: max_font,
    amount: 999,
    unit: unit,
    color: true,
    start_color,
    end_color
  }) %> -->
  <% if (site.tags.length) { %>
    <script type="text/javascript" charset="utf-8" src="<%- url_for('/js/tagcloud.js') %>"></script>
    <script type="text/javascript" charset="utf-8" src="<%- url_for('/js/tagcanvas.js') %>"></script>
    <div class="widget-wrap">
      <h3 class="widget-title"><%= __('tagcloud') %></h3>
      <div id="myCanvasContainer" class="widget tagcloud">
        <canvas width="250" height="250" id="resCanvas" style="width:100%">
          <%- tagcloud() %>
        </canvas>
      </div>
    </div>
  <% } %>
</div>
```

大功告成...才怪！打开网页发现这个标签云太大了，而且似乎因为被放大，里面的标签都是高糊，在配置文件里面改了`font-height`等东西，无果。推测可能是这个组件适应了容器宽度，而恰巧Fluid的标签页容器宽度较大。

F12打开控制台，找到标签云元素`canvas`，审查样式，将与尺寸有关的样式注释掉试试。当注释掉`width=100%`的时候，画面瞬间舒服了。顺便去掉了TagCloud标题。下面这段代码可以直接在Fluid里面用：

```ejs
  <% if (site.tags.length) { %>
    <script type="text/javascript" charset="utf-8" src="<%- url_for('/js/tagcloud.js') %>"></script>
    <script type="text/javascript" charset="utf-8" src="<%- url_for('/js/tagcanvas.js') %>"></script>
    <div class="widget-wrap">
      <!-- <h3 class="widget-title"><%= __('tagcloud') %></h3> -->
      <div id="myCanvasContainer" class="widget tagcloud">
        <canvas width="250" height="250" id="resCanvas">
          <%- tagcloud() %>
        </canvas>
      </div>
    </div>
  <% } %>
```

如果使用别的冷门主题+tag-cloud有困惑，且没有现成的技术博客的话，可以参考以上方法自力更生

祝各位在搭博客的过程中武运昌隆~
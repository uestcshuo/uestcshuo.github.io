---
layout: post
title: 使用Tapir给Jekyll博客添加全文搜索
category : 技术
tags : Tapir jekyll
description : 用Tapir为Jekyll博客添加搜索功能
---
{% include JB/setup %}


### 1.修改atom.xml文件 ###

{% raw %}
    ---
    layout: nil
    title : Atom Feed
    ---
    <?xml version="1.0" encoding="utf-8"?>
    <feed xmlns="http://www.w3.org/2005/Atom">
     
     <title>{{ site.title }}</title>
     <link href="{{ site.production_url }}/{{ site.atom_path }}" rel="self"/>
     <link href="{{ site.production_url }}"/>
     <updated>{{ site.time | date_to_xmlschema }}</updated>
     <id>{{ site.production_url }}</id>
     <author>
       <name>{{ site.author.name }}</name>
       <email>{{ site.author.email }}</email>
     </author>
    
     {% for post in site.posts %}
     <entry>
       <title>{{ post.title }}</title>
       <link href="{{ site.production_url }}{{ post.url }}"/>
       <updated>{{ post.date | date_to_xmlschema }}</updated>
       <id>{{ site.production_url }}{{ post.id }}</id>
       <content type="html">{{ post.content | xml_escape }}</content>
       <summary type="html">{{ post.description | xml_escape }}</summary>
     </entry>
     {% endfor %}
     
    </feed>

{% endraw %}
<!--break-->

以上修改是在`<entry>`节点的最后添加了如下内容：

{% raw %}
`<summary type="html">{{ post.description | xml_escape }}</summary>`
{% endraw %}

添加这段内容的目的在于：搜索出的结果不会直接显示文章的全文，而是只显示文章的摘要。这么做有一点要求，那就是`_post`目录下，每篇文章的`YAML头部`都必须包含有`description`信息。

    ---
    layout: post
    title: 使用Tapir给Jekyll博客添加全文搜索
    category : Tutorial
    tags : [Tapirgo]
    description : 用Tapir为Jekyll博客添加搜索功能
    ---

以上是本文的`YAML头部`，给出以作说明。

### 2.提交更改到GitHub ###

    $ git add .
    $ git commit -m "modify atom.xml"
    $ git push origin master

### 3.在Tapir注册atom.xml ###
登入[Tapir](http://tapirgo.com/)，并填入`atom.xml`文件的位置和自己的邮箱，点击那个大大的橘色按钮以后，会生成一个token和一个secret token。本文只会使用到token，所以暂时可以不用在意secret token。

### 4.加入Tapir ###
Tapir的搜索用到了jQuery，所以需要加入jQuery和一个处理Tapir搜索的js文件。修改`_includes\themes\twitter`目录下的`default.html`文件的`<head>`部分如下：

{% raw %}

    <!-- tapiro search -->
    <script src="{{ ASSET_PATH }}/js/jquery-1.6.1.min.js"></script>
    <script src="{{ ASSET_PATH }}/js/jquery-tapir.min.js"></script>

{% endraw %}

### 5.在导航条里加搜索框 ###
这一步对于使用Bootstrap的人来说比较简单，只要修改`_includes\themes\twitter`目录下的`default.html`文件即可:

{% raw %}

    <div class="navbar navbar-fixed-top navbar-inverse">
      <div class="navbar-inner">
        <a class="brand" href="{{ HOME_PATH }}">{{ site.title }}</a>
        <ul class="nav">
        {% assign pages_list = site.pages %}
        {% assign group = 'navigation' %}
        {% include JB/pages_list %}
          <li>
            <form class="navbar-search" action="search.html" method="get">
              <input name="query" type="text" class="search-query" placeholder="search">
            </form>
          </li>
        </ul>
      </div>
    </div>

{% endraw %}

以上是修改后的`default.html`的导航条片段。值得注意的是，`form`元素的`action`地址是`search.html`，那么接着就该创建`search.html`了。

### 6.创建search.html ###
在`username.github.io`目录下创建一个`search.html`，文件内容如下：

{% raw %}
    ---
    layout: page
    title: Search Results
    header: Search Results
    ---
    {% include JB/setup %}
    <div id="search_results">
    </div>
    <script>
      $('#search_results').tapir({'token': '5192e86e3f61b05bbb0009f6'});
    </script>

{% endraw %}

需要关注的是jQuery选择的是ID为`search_results`的元素，这是`search.html`页面中用来摆放搜索结果的DIV的ID。另外，请自行替换`token`的值。

现在搜索功能就已经完成了，可以先在本地使用`$ jekyll serve`进行编译和预览，然后再提交到GitHub上。
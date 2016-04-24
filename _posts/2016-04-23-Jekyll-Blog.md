---
layout:     post
title:      "GitHub Pages Jekyll项目初步"
subtitle:   "Start Jekyll Project"
date:       2016-04-24 12:00:00
author:     "dytan"
header-img: "img/post-bg-re-vs-ng2.jpg"
header-mask: 0.3
catalog:    true
tags:
    - GitHub
    - Blog
    - Markdown
    - Jekyll
---

> 主要参考[Francis Soung](http://www.francissoung.com)的文章["使用Jekyll+GitHub搭建自己的免费静态博客"](http://www.francissoung.com/2016/03/09/%E4%BD%BF%E7%94%A8Jekyll+GitHub%E6%90%AD%E5%BB%BA%E8%87%AA%E5%B7%B1%E7%9A%84%E5%85%8D%E8%B4%B9%E9%9D%99%E6%80%81%E5%8D%9A%E5%AE%A2/), Thanks to Francis Soung! 

## 一 Jekyll 工作机制

Jekyll 的核心其实是一个文本转换引擎。它的概念其实就是： 使用你最喜欢的标记语言来写文章，可以是 Markdown，也可以是 Textile,或者就是简单的 HTML, 
然后 Jekyll 就会帮你套入一个或一系列的布局中。在整个过程中可以设置URL路径, 你的文本在布局中的显示样式等等。这些都可以通过纯文本编辑来实现，
最终会生成网站静态页面。

使用jekyll建立GitHub Pages时，_username.github.io_项目下，_master_分支需具有一定的结构。一个基本的 Jekyll 网站的目录结构一般是像这样的：

```      
├── _config.yml     
├── _drafts     
|   ├── begin-with-the-crazy-ideas.textile      
|   └── on-simplicity-in-technology.markdown        
├── _includes       
|   ├── footer.html     
|   └── header.html         
├── _layouts            
|   ├── default.html            
|   └── post.html           
├── _posts          
|   ├── 2007-10-29-why-every-programmer-should-play-nethack.textile     
|   └── 2009-04-26-barcamp-boston-4-roundup.textile         
├── _data  
|   └── members.yml               
├── _site                  
└── index.html    
```          

当访问 http://username.github.io/_ 时，GitHub会使用Jekyll解析用户username名下的_username.github.io/master_的源代码，并构建一个静态网站，返回生成的页面。

>   一个GitHub账号对应一个用户或者一个组织，GitHub会给这个用户分配一个域名：username.github.io，当用户访问这个域名时，GitHub会去解析username用户下，username.github.io项目的master分支，这与我们之前的描述一致。
另外，GitHub还为每个项目提供了域名，例如，你有一个项目名为blog， GitHub为这个项目提供的域名为username.github.io/blog， 当你访问这个域名时，GitHub会去解析username用户下，blog项目的gh-pages分支。
所以，要搭建自己的博客，你可以选择建立名为 username.github.io的项目， 在master分支下存放网站源代码，也可以选择建立名为 blog 的项目，在 gh-pages分支下存放网站源代码。


GitHub的Help文档中的[User, Organization and Project Pages](https://help.github.com/articles/user-organization-and-project-pages)有详细的描述。


>    Jekyll提供了插件功能，在网站源代码目录下，有一个名为_plugins的目录，你可以将一些插件放进去，这样，Jekyll在解析网站源代码时，就会运行你的插件，这样插件是Ruby写成的。可以为Jekyll添加功能，例如，Jekyll默认是不提供分类页面的，你可以写一个插件，根据文章内容生成分类页面。
如果没有插件，你只能每次写文章添加分类时为每个分类手动写HTML页面。本地运行 Jekyll 时，这些插件会自动被调用，但是GitHub在解析网站源代码时，出于安全考虑，会开启安全模式，禁用这些插件。如果想用这些插件GitHub为我们提供了另一种解析网站的方式，那就是直接上传最终的静态网页，
这样，我们可以在本地使用 Jeklly 把网站解析出来，然后再上传到 GitHub上，这就使得我们既使用了插件，又使用了 GitHub。在上文的目录结构中，有一个 名为 _site 的目录，这个就是Jeklly在本地解析时最终生成的静态网站，我们把其中的内容上传到 GitHub 的项目中就可以了。


 在`master`分支下存放网站源代码，也可以选择建立名为 `blog` 的项目，在 `gh-pages`分支下存放网站源代码。

GitHub 的 Help 文档中的 [User, Organization and Project Pages](https://help.github.com/articles/user-organization-and-project-pages)对此有 详细的描述。


如果你不使用插件，那么只需要维护一个分支就好:

	- username/username.github.io 的 master 分支
	- username/blog 的 gh-pages 分支

其中 username 是你的 GitHub 帐号。

你需要在本地维护一份网站源代码，添加新文章后，使用 jekyll 在本地测试一下，没有问题后，commit 到 GitHub 上的相应分支中就可以了。

如果你需要使用插件，那么需要维护两个分支，一个是网站的源代码分支，另一个 是 Jeklly 解析源代码后生成的静态网站。

## 二 jekyll项目结构

### 2.1 [目录说明](http://jekyll.bootcss.com/docs/structure/)

对于基本的目录结构：

```      
├── _config.yml     
├── _drafts     
|   ├── begin-with-the-crazy-ideas.textile      
|   └── on-simplicity-in-technology.markdown        
├── _includes       
|   ├── footer.html     
|   └── header.html         
├── _layouts            
|   ├── default.html            
|   └── post.html           
├── _posts               
|   └── 2009-04-26-barcamp-boston-4-roundup.textile         
├── _data  
|   └── members.yml               
├── _site                  
└── index.html    
``` 

>
文件 / 目录| 描述         
------------|-----------------------------------------|        
_config.yml |保存配置数据。很多配置选项都会直接从命令行中进行设置，但是如果你把那些配置写在这儿，你就不用非要去记住那些命令了。               
_drafts     |drafts 是未发布的文章。这些文件的格式中都没有 title.MARKUP 数据。学习如何使用 drafts.              
\_includes   |你可以加载这些包含部分到你的布局或者文章中以方便重用。可以用这个标签`/\{\%/ include file.ext /\%/}/`来把文件 _includes/file.ext 包含进来。       
\_layouts    |layouts 是包裹在文章外部的模板。布局可以在 YAML 头信息中根据不同文章进行选择。 这将在下一个部分进b行介绍。标签 `{{` content `}}` s可以将content插入页面中。       
_posts      |这里放的就是你的文章了。文件格式很重要，必须要符合: YEAR-MONTH-DAY-title.MARKUP。 The permalinks 可以在文章中自己定制，但是数据和标记语言都是根据文件名来确定的。        
_data       |Well-formatted site data should be placed here. The jekyll engine will autoload all yaml files (ends with .yml or .yaml) in this directory. If there's a file members.yml under the directory, then you can access contents of the file through site.data.members.          
_site       |一旦 Jekyll 完成转换，就会将生成的页面放在这里（默认）。最好将这个目录放进你的 .gitignore 文件中。                
index.html and other HTML, Markdown, Textile files|如果这些文件中包含 YAML 头信息 部分，Jekyll 就会自动将它们进行转换。当然，其他的如 .html， .markdown， .md，或者 .textile 等在你的站点根目录下或者不是以上提到的目录中的文件也会被转换。            
Other Files/Folders|其他一些未被提及的目录和文件如 css 还有 images 文件夹， favicon.ico 等文件都将被完全拷贝到生成的 site 中。 这里有一些使用 Jekyll 的站点，如果你感兴趣就来看看吧。           

--------
 

#### [_config.yml](http://jekyll.bootcss.com/docs/configuration/)

这是针对 Jekyll 的[配置文件](http://jekyllrb.com/docs/configuration/)， 决定了 Jekyll 如何解析网站的源代码,下面是一个示例：

	baseurl: /StrayBirds
	markdown: redcarpet
	safe: false
	pygments: true
	excerpt_separator: "\n\n\n"
	paginate: 5

我的网站建立在 StrayBirds 项目中，所以 baseurl 设置成 StrayBirds， 我的文章采用 Markdown 格式写成，可以指定 Markdown 的解析器 redcarpet。 另外，安全模式需要关闭，以便 Jekyll 解析时会运行插件。 pygments 可以使得Jekyll解析文章中源代码时加入特殊标记，例如指定代码类型， 这可以被很多 javascript 代码高度库使用。 excerpt_separator 指定了一个摘要分割符号，这样 Jekyll 可以在解析文章时， 将文章的提要提取出来。 paginate 指定了一页有几篇文章，页数太多时，我们可以将文章列表分页，我们在 后文还会提到。

#### _layouts

这个目录存放着一些网页模板文件，为网站所有网页提供一个基本模板，这样 每个网页只需要关心自己的内容就好，其它的都由模板决定。例如，这个目录下的 default.html 文件：

    {% raw %}
    <!DOCTYPE html>
    <html lang="en">
    {% include head.html %}
    <body ontouchstart="">
        {% include nav.html %}

        {{ content }}
    
        {% include footer.html %}
    </body>
    </html>
    {% endraw %}

可以看出，这个文件就是所有页面共有的东西，每个页面的具体内容会被填充在`content`中，注意这个`content`两边的标记，这是一种叫`liquid`的标记语言。 另外，还有那个`page.title`，其中 page 表示引用 default.html的 那个页面，这个页面的 title 值会在 page 相应页面中被设置，例如 下面的 index.html 文件，开头部分就设置了 title值。
>在md文件中若要引用符合[Liquid](http://shopify.github.io/liquid/)规范的字符串，以防解析，需要使用_{% raw %}_ 与 _{% endraw %}_分隔

#### index.html

这是网站的首页，访问 `http://username.github.io` 时，会指向 `http://username.github.io/index.html`，我们看一下基本内容：

	---
	layout: default
	title: 首页
	---
	
	{% raw %}
	<ul class="post-list">
	    {% for post in site.posts %}
	        <a href="{{site.baseurl}}{{post.url}}"> {{ post.title }}  </a> <br>
	        {{ post.date | date: "%F" }} <br>
	        {{ post.category }} <br>
	        {{ post.excerpt }} 
	    {% endfor %}
	{% endraw %}
	</ul>

注意，文件开头的描述，我们称之为 [front-matter](http://jekyllrb.com/docs/frontmatter/)， 是对当前文件的一种描述，这里 设置的变量可以在解析时被引用，例如这里的 layout就会告诉 Jekyll, 生成 index.html 文件时，去 _layouts 目录下找 default.html 文件，然后把当前文件解析后，添加到 default.html 的 content 部分，组成最终的 index.html 文件。还有title 设置好的 值，会在 default.html 中通过 page.title 被引用。

文件的主体部分遍历了站点的所有文章，并将他们显示出来，这些语法都是 liquid 语法， 其中的变量，例如 site, 由 Jekyll 设置我们只需要引用就可以了。而 post 中的变量， 如 post.title, post.category 是由 post 文件中的 front-matter 决定，后面马上就会看到。

#### _posts

这个目录存放我们的所有博客文章，他们的名字有统一的格式：

	YEAR-MONTH-DAY-title.markdown

例如，2016-04-23-jeklly-Blog.md，这个文件名会被解析，前面的 index.html 中， post.date 的值就由这里文件名中的日期而来。下面，我们看看一篇文章的内容示例：

```
---
layout:     post
title:      "GitHub Pages Jekyll项目初步"
subtitle:   "Start Jekyll Project "
date:       2016-04-24 12:00:00
author:     "dytan"
header-img: "img/post-bg-re-vs-ng2.jpg"
header-mask: 0.3
catalog:    true
tags:
    - GitHub
    - Blog
    - Markdown
    - Jekyll
---
```

可以看出，文章的 front-matter 部分设置了多项值，以后可以通过类似 `post.title, post.category` 的方式引用这些些，另外，layout部分的值和之前解释的一样， 文件的内容会被填充到 `_layouts/default.html` 文件的 `content` 变量中。

另外，文章中 为什么不试试呢之后的有三个不可见的 \n，它决定了这三个 \n 之前的内容会被放在 post.excerpt 变量中，供其它文件使用。

#### _includes

这个文件中，存放着一些模块文件，例如 categories.ext，其它文件可以通过

	{% raw %}
	{% include categories.ext %}
	{% endraw %}

来引用这个文件的内容，方便代码模块化和重用。

#### _plugins

这个文件中存放一些Ruby插件, 例如 gen_categories.rb，这些文件会在 Jekyll 解析网站源代码时被执行。下一节讲述的就是插件。

### 2.2 插件

插件使用 Ruby 写成，放在 _plugins 目录下，有些 Jekyll 没有的功能，又不能 手动添加，因为页面的内容会随着文章日期类别的不同而不同，例如分类功能和归档功能， 这时，就需要使用插件自动生成一些页面和目录。

**分类**

分类插件推荐使用 [jekyll-category-archive-plugin](https://github.com/shigeya/jekyll-category-archive-plugin/tree/master/_plugins), 它会根据网站文章的分类信息，为每个类别生成一个页面。

使用方法是，把 `plugins/categoryarchive_plugin.rb` 放在 `plugins` 目录下， 把 `_layouts/categoryarchive.html` 放在 `layouts` 目录下， 这样，这个插件会在Jekyll解析网站时，生成相应categories目录，目录下是各个分类， 每个分类下都有一个 `index.html` 文件，这个文件是根据模板文件 `categoryarchive.html` 生成的，例如：

	_site/categories/
	├── 工具
	│   └── index.html
	├── 思想
	│   └── index.html
	├── 技术
	│   └── index.html
	└── 源代码阅读
	    └── index.html

然后，你就可以通过 `http://username.github.io/blog/categories/工具/` 访问 工具类下的 `index.html` 文件。

**归档**

归档插件推荐使用 [jekyll-monthly-archive-plugin](https://github.com/shigeya/jekyll-monthly-archive-plugin),它会根据网站 _posts目录下的文章日期，为每个月生成一个页面。

使用方法同上。注意，这个插件在 jekyll-1.4.2 中可能会出错，在 jekyll-1.2.0 中没有错误。

### 2.3 组件


**分页**

当文章很多时，就需要使用分页功能，在 Jekyll 官网上提供了一种 [实现](http://jekyllrb.com/docs/pagination/)，把相应代码放在 主页上，然后在 `_config.yml` 中设置 `paginate` 值就行了。

**评论**

评论功能需要使用社会化评论系统，使用的是 [DISQUS](http://disqus.com/), 注册 之后，将评论区的一段代码放在你需要使用评论功能的页面上, 然后，通过在页面的 front-matter 部分使用

	comments: true

启用评论。


### 2.4 自定义域名解析

这里很简单了，在你的网站根目录中新建一个`CNAME`的文件，**注：一定要大写**。

在CNAME文件中添加你要解析的域名。


获取GitHub Page的IP地址，最好的方法就是ping一下你的 `username.github.io` 域名。

最后在你的域名提供商域名管理系统中添加对应的解析。我这里做了一个CNAME，可直接解析到A记录到IP即可。

## 三 [jekyll变量](http://jekyll.bootcss.com/docs/variables/)

Jekyll 会遍历你的网站搜寻要处理的文件。任何有 YAML 头信息的文件都是要处理的对象。对于每一个这样的文件，Jekyll都会通过Liquid 模板工具来生成一系列的数据。
下面是常用的一些可用数据变量的参考

#### 全局(Global)变量


变量|说明   
|---------|----------|
site    |来自_config.yml文件，全站范围的信息 +配置。
page    |页面专属的信息 + YAML 头文件信息。通过 YAML 头文件自定义的信息都可以在这里被获取。详情请参考下文。      
content |被 layout 包裹的那些 Post 或者 Page 渲染生成的内容。但是又没定义在 Post 或者 Page 文件中的变量。            
paginator|每当 paginate 配置选项被设置了的时候，这个变量就可用了。详情请看，分页。          

#### 全站(site)变量


变量|说明       
|----------|---------|
site.time   |当前时间（跑jekyll这个命令的时间点）。      
site.pages  |所有 Pages 的清单。           
site.posts  |一个按照时间倒叙的所有 Posts 的清单。          
site.related_posts|如果当前被处理的页面是一个 Post，这个变量就会包含最多10个相关的 Post。默认的情况下， 相关性是低质量的，但是能被很快的计算出来。如果你需要高相关性，就要消耗更多的时间来计算。 用jekyll 这个命令带上 --lsi (latent semantic indexing) 选项来计算高相关性的 Post。            
site.categories.CATEGORY|所有的在 CATEGORY 类别下的帖子。           
site.tags.TAG|所有的在 TAG 标签下的帖子。               
site.[CONFIGURATION_DATA]|所有的通过命令行和 _config.yml 设置的变量都会存到这个 site 里面。 举例来说，如果你设置了 url: http://mysite.com 在你的配置文件中，那么在你的 Posts 和 Pages 里面，这个变量就被存储在了 site.url。Jekyll 并不会把对 _config.yml 做的改动放到 watch 模式，所以你每次都要重启 Jekyll来让你的变动生效。 
 
#### 页面(page)变量

变量|说明           
|----------|----------|
page.content    |页面内容的源码。            
page.title      |页面的标题。              
page.excerpt    |页面摘要的源码。                
page.url        |帖子以斜线打头的相对路径，例子： /2008/12/14/my-post.html。 
page.date       |帖子的日期。日期的可以在帖子的头信息中通过用以下格式 YYYY-MM-DD HH:MM:SS (假设是 UTC), 或者 YYYY-MM-DD HH:MM:SS +/-TTTT ( 用于声明不同于 UTC 的时区， 比如 2008-12-14 10:30:00 +0900) 来显示声明其他 日期/时间 的方式被改写，             
page.id         |帖子的唯一标识码（在RSS源里非常有用），比如 /2008/12/14/my-post                 
page.categories |这个帖子所属的 Categories。Categories 是从这个帖子的 _posts 以上 的目录结构中提取的。具体来说, 一个在 /work/code/_posts/2008-12-24-closures.md 目录下的 Post，这个属性就会被设置成 ['work', 'code']。不过 Categories 也能在 YAML 头文件信息 中被设置。       
page.tags       |这个 Post 所属的所有 tags。Tags 是在YAML 头文件信息中被定义的。              
page.path       |Post 或者 Page 的源文件地址。举例来说，一个页面在 GitHub上得源文件地址。 这可以在 YAML 头文件信息 中被改写。         

>任何自定义的头文件信息都会在 page 中可用。 具体来说，如果你在一个 Page 的头文件中设置了 custom_css: true， 这个变量就可以这样被取到 page.custom_css。 

#### 分页器(Paginator)

变量|说明       
|------------|-------------|
paginator.per_page  |每一页Posts的数量。       
paginator.posts     |这一页可用的Posts。       
paginator.total_posts|Posts 的总数。        
paginator.total_pages|Pages 的总数。        
paginator.page      |当前页号。          
paginator.previous_page|前一页的页号。         
paginator.previous_page_path|前一页的地址。            
paginator.next_page |下一页的页号。        
paginator.next_page_path    |下一页的地址。

这些变量仅在首页文件中可以，不过他们也会存在于子目录中，就像 /blog/index.html。 


## Reference

- [jekyll documentation en](http://jekyllrb.com/docs/home/)
- [jekyll documentation zh](http://jekyll.bootcss.com/docs/home/)
- [jekyll 官网文档部分翻译](http://blog.csdn.net/maoxunxing/article/details/40479753)
- [使用Jekyll+GitHub搭建自己的免费静态博客 by Francis Soung ](http://www.francissoung.com/2016/03/09/%E4%BD%BF%E7%94%A8Jekyll+GitHub%E6%90%AD%E5%BB%BA%E8%87%AA%E5%B7%B1%E7%9A%84%E5%85%8D%E8%B4%B9%E9%9D%99%E6%80%81%E5%8D%9A%E5%AE%A2/)
- [Liquid Official Doc](http://shopify.github.io/liquid)
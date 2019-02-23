<!--
.. title: Hello World
.. slug: hello-world
.. date: 2018-07-30 11:33:26 UTC+08:00
.. tags: Hexo, Nikola
.. category: ComputerScience
.. link: 
.. description: 搭建新博客的心路历程
.. type: text
-->



最近因为重拾学业（？）打算重新启用博客，但是原来使用 `nexT` 的主题的博客因为配置被我改花了所以没有兴趣接着用了。刚好也没有什么事情要忙，就打算重新搭一个博客。

原博客的基本内容都是 oi 题解，既然 oier 已经不适合用于定义我的身份，那么 oi 题解也没有必要重新搬运回新博客。于是这里空空如也，期待时间能让她渐渐丰满起来。

新博客仍然由 Hexo 驱动，使用 `inside` 主题（找了很久，觉得这个最萌）。

搭建的时候遇到了一些坑，记录一下。

<!-- TEASER_END -->


# npm 换源
下载 Hexo 的时候发现 npm 确实非常慢，不能支持使用。
解决方法是使用 cnpm，换源到阿里的镜像。

# inside 主题的分类和标签问题

这个问题真的很垃圾...
主题的作者在文档里说这个主题没有使用 Hexo 自带的分类和标签工具，因此需要在 `package.json` 文件中去掉几个 Hexo 自带的插件。这实在是太方便了，方便就方便在 **json 不支持注释**。
所以注释掉那三行后无限报错后，我发现了这个问题。

另外，在 `source` 文件夹下的 `categories` 等文件夹也需要删除，因为它们都是基于 Hexo 自带插件的，存放在那里会出现冲突。

# MathJax 支持
下载 `hexo-math` 插件后，其他主题都可以显示公式了，但是 `inside` 不行，我也懒得知道为什么了……
解决方法是在主题目录下的 `layout` 文件夹中修改 `index.ejs` 配置文件，在其中加入对 MathJax 的链接即可。修改了支持行内公式和 120% 缩放的配置。

还出现了一个比较垃圾的问题，`inside` 主题打开文章默认不会重新加载页面，这就导致 MathJax 不能渲染打开的文章，必须刷新一次。
__debug 帮助我解决了这个问题，按照规定不可以感谢他。
在 `inside/layout/index.ejs` 中加入以下代码：

```javascript
<script type="text/javascript">
    window.onload = function() {
        document.body.addEventListener('DOMSubtreeModified', function () {
            MathJax.Hub.Queue(["Typeset",MathJax.Hub]);
        }, false);
    };
</script>
```

逻辑是：如果监测到网页内容变化，则重新加载 MathJax。

# 一些页面修改
`inside` 默认的界面显示宽度很窄，不好看。
`inside/source/` 目录下你使用的那个语言的 js 一串乱码的文件中所有的 `640px` 改掉即可。
标题 1 的上部距离上一行文字太近，在同一目录下的 css 文件里加入 `padding` 的设置就可以了。

# 行间公式问题

为了解决这个问题我把原文件夹的 hexo 折腾坏了……只好重新建了文件夹（还好有备份没出问题）
因为 hexo 的 markdwon 渲染会出现一些转义冲突，比如 `_` 和 `/` 。
为了解决它，最后采用了最丑陋的方法，即手改转义的 js 文件（每次重新安装都要修改一次）

`Blog/node_modules/marked/lib` 下的 `marked.js`

将 
```
escape: /^\\([\\`*{}\[\]()# +\-.!_>])/，
```
替换为
```
escape: /^\\([`*\[\]()# +\-.!_>])/,
```
这一步取消了对 `\\,\{,\}` 的转义 (escape)。

将
```
em: /^\b_((?:[^_]|__)+?)_\b|^\*((?:\*\*|[\s\S])+?)\*(?!\*)/,
```
替换为
```
em:/^\*((?:\*\*|[\s\S])+?)\*(?!\*)/,
```
这一步取消了对斜体标记`_`的转义。因为单星号`*`也是斜体，所以这个改动无伤大雅。


# 换驱动器了！
换用 Nikola 驱动。
Nikola 真方便。



# 未完待续
经验告诉我这个博客还将被无限折腾下去。
这篇文章的剩余部分交给未来。
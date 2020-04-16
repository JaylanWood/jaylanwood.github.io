# 博客图床怎么弄


# 博客图床怎么弄

## 图床怎么搞定

hugo博客算是搭好了，主题也换成了 [LoveIt](https://themes.gohugo.io/loveit/)，挺不错的主题，稍微修改了一下，原作者的博客：[Dillon](https://dillonzq.com/)。

目前还挺满意，可是图床却挺烦的。

国内的服务商，七牛是挺多人推荐的，但是要备案，https 还要收费。穷啊～

国外的首先就想到 github，毕竟免费，有戏，搞起来！

## PicGo + GitHub 图床

### 核心思路：

1. 新建一个 github 仓库，专门用来上传图床。
2. 一个专门用来上传图床的工具，不然手动 commit 要累死人。
3. 新建一个 token 专门给图床工具用。

### 实现方案：

PicGo + GitHub 图床

### 参考文章：

[图床上传工具PicGo](https://sspai.com/post/44495)

## 最终结果

体验很不错，只是图片不算私有，若没梯子有时会很慢。

## 试一下直接在hugo博客项目加图

![轮播2原理](../../static/images/slide/轮播2原理.gif)


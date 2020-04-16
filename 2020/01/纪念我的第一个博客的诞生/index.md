# 纪念我的第一个博客的诞生


# 纪念我的第一个hugo博客的诞生

## Hello World

才刚搭起第一个博客，想不到写啥，那就先来句 `Hello World` 吧。

```javascript
console.log('Hello World')
```

## 记录hugo博客搭建的过程

1. 安装 hugo

```bash
brew install hugo
```

2. 新建一个 myblog 网站

```bash
hugo new site blog
cd blog/
```

3. 安装主题

```bash
git clone https://github.com/xiaoheiAh/hugo-theme-pure themes/pure
```

4. 新建一篇博客文章

```bash
hugo new posts/blog1.md
```

5. 本地 server 测试

```bash
hugo server -D
或者
hugo server -t 主题名 --buildDrafts
```

6. 生成静态网站到 public 目录

```bash
hugo -D
或者
hugo --theme=pure --baseUrl="https://jaylanwood.github.io/" --buildDrafts
```


8. 把博客部署到 github

```bash
git init
git add .
git commit -m "我的hugo博客"
git remote add origin git@github.com:JaylanWood/jaylanwood.github.io.git
git push -u origin master
```

7. 测试部署结果，浏览器打开 [jaylanwood.github.io](https://github.com/JaylanWood/jaylanwood.github.io)

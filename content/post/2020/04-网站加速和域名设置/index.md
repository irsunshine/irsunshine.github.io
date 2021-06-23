---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "网站加速和域名设置"
subtitle: ""
summary: ""
authors: [向天龙]
tags: []
categories: [其他]
date: 2020-06-20T10:36:27+08:00
lastmod: 2020-06-27T15:01:27+08:00
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

### 背景

网站托管在GitHub Pages，某些众所周知的原因，境内访问GitHub Pages有点慢。故而申请了个人域名，购买了国内云主机供应商的CDN加速服务。在设置加速服务的时候，想到了自己还有开发机器，上面部署了docker、frp、k8s等服务，这些服务都有配套的dashboard，本着不浪费的原则，配置了几个反向代理，全部挂上了二级域名。

当我美滋滋用着二级域名的时候，突发现www子域名无法访问了，阿里云上命名配置了DNS同时解析到www.xiangtianlong.com和xiangtianlong.com，尚未启用CDN加速的时候，两个域名都能正常使用。

在配置CDN加速的时候，由于二级域名太多，启用了泛域名规则，统一路由到了开发机器，结果导致www这个二级域名也挂了，是的，你没看错，www前缀是个二级域名。实际网站部署在GitHub Pages，开发机器没有任何网站的缓存信息。

至于为什么开发机器上没有部署站点，因为静态博客，配着GitHub提供的action，自动集成发布，真香。

### 域名

非专业的web开发，对于域名的理解不涉及SEO和跨域问题。作为博客站点，裸域容易突出博客主的站点，说的就是我这种用汉字拼音当做域名的小朋友，加之当前移动访问居多，能少输入几个字符。

> 电脑端能使用快捷键免去输入www和com

### CDN

阿里云和腾讯云的都用过，新人上手不难，腾讯云还有个视频单独讲解相关的概念。CDN加速的原理和京东仓库是一个道理，发售新商品，提前统一配送到全国各地的仓库，触发配送请求的时候，就近分发。

回源地址：网站资源原始存放的地址

缓存文件设置，浏览器F12，管理控制台，简单分析静态资源和动态资源
- 全部0天有效期
- `.php;.jsp;.asp;.aspx` 0天有效期
- `.jpg;.png;.js;.css;.woff2`	1天有效期

腾讯云配置规则：
- 缓存过期规则最多可配置10条
- 多条缓存过期规则之间的优先级为底部优先
- 缓存过期时间最多可设置365天

### 悲惨自述

以前也没用过Nginx，以为网站随便搜索就能明白反向代理的配置，结果有点混乱，折腾半天连个302跳转也没弄明白，结果屁用没有。就想着笨办法解决一下，DNS解析删除*模式的泛域名解析，单个二级域名进行独立设置。此时突然注意到了阿里云DNS解析有一个叫做显示URL跳转的模式，尝试了一下，这不就是我想要的302跳转。

> 设置了第一个二级域名正常访问，等我设置第二个的时候，发现没用，都快怀疑人生了，等了一会突然就能用了，看来阿里云的DNS扩散偶尔也是会抽风的

### 参考资料

- [为什么越来越多的网站域名不加「www」前缀？](https://www.zhihu.com/question/20414602)
- [带www和不带www域名有什么区别呢?](https://www.cloudxns.net/Support/detail/id/918.html)
- [Docker nginx 反向代理设置](https://gythialy.github.io/Docker-nginx-reverse-proxy/)
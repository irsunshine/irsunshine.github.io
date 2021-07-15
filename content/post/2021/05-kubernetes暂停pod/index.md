---
title: "kubernetes暂停pod"
date: 2021-07-12T11:23:09+08:00
lastmod: 2021-07-12T11:23:09+08:00
draft: false
keywords: [kubernetes]
description: ""
tags: []
categories: []
author: ""

# You can also close(false) or open(true) something for this content.
# P.S. comment can only be closed
comment: false
toc: true
autoCollapseToc: false
postMetaInFooter: false
hiddenFromHomePage: false
# You can also define another contentCopyright. e.g. contentCopyright: "This is another copyright."
contentCopyright: false
reward: false
mathjax: false
mathjaxEnableSingleDollar: false
mathjaxEnableAutoNumber: false

# You unlisted posts you might want not want the header or footer to show
hideHeaderAndFooter: false

# You can enable or disable out-of-date content warning for individual post.
# Comment this out to use the global config.
#enableOutdatedInfoWarning: false

flowchartDiagrams:
  enable: false
  options: ""

sequenceDiagrams: 
  enable: false
  options: ""

---

Deployment控制器实现了Kubernetes集群中一个很重要的功能，Pod的水平拓展和收缩功能。这个功能是传统云时代平台所必备的能力。

原本为了防止Pod由于意外关闭设计的自动恢复，在处理异常情况需要临时暂停下服务时应该如何处理？除了暴力的删除Deployment，有没有其他的方式，实现类似暂停的效果？

```shell
kubectl scale --replicas=0 deployment/<your-deployment>
```

## 参考链接

[how to stop/pause a pod in kubernetes](https://stackoverflow.com/questions/54821044/how-to-stop-pause-a-pod-in-kubernetes)

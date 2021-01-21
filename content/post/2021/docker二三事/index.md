---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "docker二三事"
subtitle: ""
summary: ""
authors: [向天龙]
tags: [docker,linux,centos]
categories: []
date: 2021-01-21T09:26:07+08:00
lastmod: 2021-01-21T09:26:07+08:00
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

### 安装

国内服务器安装，先设置阿里云仓库地址

```shell
yum install yum-utils device-mapper-persistent-data lvm2 && \
sudo yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo && \
sudo yum install -y docker-ce docker-ce-cli containerd.io && systemctl enable --now docker
```

### 普通用户添加docker权限

```shell
sudo usermod -aG docker ${USER}
```

### 卸载

```shell
sudo yum erase -y docker-ce docker-ce-cli containerd.io
```

### 镜像加速

加速的地址各位看管自己注册阿里云账号获取，此服务免费，阿里云也提供免费的镜像构建服务

```shell
cat > /etc/docker/daemon.json <<EOF
{
  "registry-mirrors": ["https://9fbz1if4.mirror.aliyuncs.com"]
}
EOF
```

重启设置生效
```shell
systemctl daemon-reload && \
systemctl restart docker
```

### 安装特定版本的`docker`

`kubernetes`和`docker`的发布并没与完全同步，此时需要安装特特定的版本

```shell
yum list docker-ce --showduplicates | sort -r
sudo yum install -y docker-ce-18.09.2-3.el7 docker-ce-cli-18.09.2-3.el7 containerd.io-18.09.2-3.el7 && systemctl enable --now docker
```

### 强烈推荐的控制面板

```shell
docker volume create portainer_data && \
docker run -d --name=portainer --restart=always -p 9000:9000 -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:2.0.1
```

### 常用镜像拉取合集

```shell
docker pull rancher/rancher:stable && docker pull  portainer/portainer-ce:2.0.1 && \
docker pull centos:7 && docker pull ubuntu:20.04 && docker pull ubuntu:18.04 && \
docker pull redis:5 && docker pull redis:6 && \
docker pull alpine:3.11 && docker pull busybox:1.32 && \
docker pull rabbitmq:3.7-management && \
docker pull mariadb:10.2 && \
docker pull nginx:1.18 && docker pull nginx:1.19 && \
docker pull mysql:5.6 && docker pull mysql:8 && \
docker pull elasticsearch:6.8.11 && docker pull logstash:6.8.11 && docker pull kibana:6.8.11 && \
docker pull zookeeper:3.4 && \
docker pull influxdb:1.7 && docker pull grafana/grafana:7.3.1 && \
docker pull percona:8 && docker pull percona:5.6 && \
docker pull cloverzrg/frps-docker:0.34.3 && docker pull cloverzrg/frpc-docker:0.34.3
```

### 常用组合命令

[https://docs.docker.com/engine/reference/commandline/docker/](https://docs.docker.com/engine/reference/commandline/docker/)

---

* 查看容器运行状态，附加`format`参数，查看详细的容器信息，此时不关注镜像信息
```shell
docker ps --format "{{.Names}}: {{.Ports}}: {{.Size}}"
#portainer: 0.0.0.0:8000->8000/tcp, 0.0.0.0:9000->9000/tcp: 0B (virtual 172MB)
#influxdb: 0.0.0.0:8086->8086/tcp: 183B (virtual 311MB)
```

---

* 一键停止所有容器
```shell
docker stop $(docker ps -a -q)
```

* 一键删除所有镜像
```shell
dokcer rmi $(docker images -a -q)
```

---

* 导出镜像
```shell
docker save <IMAGE NAME>:<IMAGE TAG> > -o XXX.tar
```

* 导出镜像并压缩
```shell
docker save <IMAGE NAME>:<IMAGE TAG> | gzip > XXX.tar
```

* 导入镜像
```shell
docker load -i XXX.tar
```
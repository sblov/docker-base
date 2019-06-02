# Docker

## 简介

### 理念

​	Docker基于Go语言实现的云开源项目。

​	Docker主要目标是“Build，Ship and Run Any App，Anywhere”（一次封装，到处运行）

​	解决运行环境和配置问题，方便做持续集成并有助于整体发布的容器虚拟化技术

### 作用

#### 	虚拟化

​	Docker与传统虚拟化方式区别：

- 传统虚拟机技术是虚拟出一套硬件后，在其上运行一个完整操作系统，在该系统上再运行所需的应用程序

- 容器的应用进程直接运行与宿主的内核，容器内没有自己的内核，而且也没有进行硬件虚拟。因此容器比传统虚拟机更为轻便

- 每个容器之间相互隔离，每个容器有自己的文件系统，容器之间进程不会相互影响，能区分计算资源

####	开发/运维（DevOps）

​	一次构建，随处运行

- 更快速的应用交付与部署
- 更便捷的升级与扩缩容
- 更简单的系统运维
- 更高效的计算资源利用

### Docker三要素

#### 镜像（Image）

​	一个只读的模板。镜像可以用来创建Docker容器，一个镜像可以创建多个容器

#### 容器（Container）

​	容器是用镜像创建的运行实例，它可以被启动、开始、停止、删除。每个容器都是相互隔离，保证安全的平台

​	可以将容器看做是一个简易版的Linux环境和运行在其中的应用程序

​	容器的定义与镜像几乎一样，也是一堆层的统一视角，唯一区别在于容器的最上面那一层是可读可写的

#### 仓库（Repository）

​	集中存放镜像文件的地方。

​	仓库注册服务器上存放多个仓库，每个仓库又包含多个镜像，每个镜像有不同的标签

​	国内的公开仓库：阿里云、网易云等

## 安装

​	1、安装必要的软件包以允许apt通过HTTPS使用存储库

```shell
sudo apt-get install apt-transport-https ca-certificates curl software-properties-common
```

​	2、添加GPG密钥，可以添加官方的和阿里的（国内镜像更快）

```shell
// 官方
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
// 阿里
curl -fsSL https://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg | sudo apt-key add -
```

​	3、命令验证

```sehll
sudo apt-key fingerprint 0EBFCD88
```

​	4、正常返回结果

```shell
pub   rsa4096 2017-02-22 [SCEA]
      9DC8 5822 9FC7 DD38 854A  E2D8 8D81 803C 0EBF CD88
uid           [ unknown] Docker Release (CE deb) <docker@docker.com>
sub   rsa4096 2017-02-22 [S]
```

​	5、设定稳定仓储库

```shell
sudo add-apt-repository "deb [arch=amd64] https://mirrors.aliyun.com/docker-ce/linux/ubuntu $(lsb_release -cs) stable"
```

​	6、安装docker

```shell
sudo apt-get update
sudo apt-get -y install docker-ce
```

​	**安装指定版本**

​	1、先查看版本

```shell
apt-cache madison docker-ce
```

​	2、安装

```shell
sudo apt-get install -y docker-ce=<VERSION>
```

### 配置

#### 	配置文件

​	Ubuntu中，docker的配置文件为`/etc/default/docker`

![](img/setting.png)

#### 	开启服务

​	`service docker start` / `sudo systemctl start docker`

​	查看相关信息

​	`docker verison`

![](img/service.png)

#### 	配置阿里云镜像加速

1、**注册阿里云账号**

2、**获取加速器地址配置**

`https://cr.console.aliyun.com/cn-hangzhou/instances/mirrors`

```shell
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://g5wqxm6c.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```


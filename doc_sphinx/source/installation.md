# Wio Platform服务器部署指南

## 1 前言
本指南只描述使用docker方式部署.使用docker方式部署在迁移,客户私有化部署,集群scale方面都具独特优势.

## 2 名词
- Core Service - Core Service包括以下服务: Thing管理, SSO接入, 在线编译, OTA Schedule, 这些服务以API方式提供.
- PP Service - PP即Physical Proxy, 承担Device的接入和Web of Things API请求, PP仅有缓存能力,数据过期后需向Core请求刷新.
- Core+PP节点 - 运行Core和PP service的服务器节点,其会包含数个docker容器.
- PP节点 - 运行PP service的服务器.

## 3 部署步骤
### 3.1 Core+PP节点初次部署
Core+PP节点的docker拓扑如下图:
![image](https://cloud.githubusercontent.com/assets/5130185/18627298/d673f27a-7e8c-11e6-8315-2314c7a06dc6.png)

#### step1
 `git clone https://git.oschina.net/seeedstudio_se/wio_pro_deploy_controller.git`

需要权限下载.

#### step2
 `chmod +x init_deployment_controller.sh && ./init_deployment_controller.sh`

根据提示输入oschina的git帐号密码.
此脚本会建立基本的目录结构, 并且初次编译固件所需的静态态.

#### step3
 修改必要的配置文件.
- docker-compose.yml
  - 修改mysql用户名密码等
  - 修改registry.cn-hangzhou.aliyuncs.com/killingjacky/wio_pro_core:dev-latest的tag, 目前为dev-latest, 正式版发布后为latest
  - 修改registry.cn-hangzhou.aliyuncs.com/killingjacky/wio_pro_pp:dev-latest的tag, 同上.
  - 修改需要letsencrypt的域名
- mount/core/server/config.py
- mount/pp/config.py


#### step4
启动docker容器.
目前docker镜像存储在阿里云docker HUB上, 为私有镜像, 因此需要阿里云docker帐号(https://cr.console.aliyun.com)
先登陆阿里docker registry:
`docker login --username=jacky.shaoxg@foxmail.com registry.cn-hangzhou.aliyuncs.com`

海外服务器用`docker login --username=jacky.shaoxg@foxmail.com registry.us-west-1.aliyuncs.com`
密码: Ali111111
由于初次部署, mysql建表需要一定时间,会导致其它容器连接mysql失败, 因此需要先启动mysql容器以创建初始数据表.
`docker-compose up -d mysql`
使用`docker-compose logs -f mysql`来监控日志, 看到"mysqld: ready for connections"的消息后说明mysql服务已经启动.
最后,启动其它所有容器:
`docker-compose up -d`

### 3.2 PP节点初次部署

PP节点的docker拓扑如下图:

![](https://cloud.githubusercontent.com/assets/5130185/18627024/2ed2c9de-7e8b-11e6-9c3b-f2ee993477e3.png)

#### steps TBD

### 3.3 PP+Core节点更新驱动与Device固件

由于驱动,固件源代码是以静态文件形式挂载进容器,因此更新这些静态文件只需要简单覆盖即可, 无需重启服务.

#### step1
更新源代码.
`cd git_repos/core`
`git pull`
`git checkout master`  # or dev

#### step2
编译静态库.
`python ./server/update.py`

#### step3
拷贝文件.
`cp -rf firmware_esp ../../mount/core`
`cp -rf grove_drivers ../../mount/core`
`cp -rf generated ../../mount/core`

### 3.4 任意节点更新server部分

#### step1
推送代码到阿里云code,触发阿里云docker镜像自动构建.

#### step2
进入项目部署目录,`docker-compose pull`拉取最新的镜像.

#### step3
`docker-compose up -d <the service name>` 重启容器.

core可实现不中断服务更新, 说明TBD.
pp无法实现不中断更新.

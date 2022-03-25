# install docker and nvidia-docker2

创建docker组

sudo groupadd docker

将当前用户加入docker组

sudo gpasswd -a ${USER} docker

重新启动docker服务（下面是CentOS7的命令）

需要好的网络

https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html#setting-up-docker

# docker图形化管理软件

$ docker run -d -p 8800:8000 -p 9900:9000 --name=portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainerci/portainer:pr4791

## open browser

[http://127.0.0.1:9900](http://127.0.0.1:9900)

# docker image

## 20.04 with cuda 11.4

docker pull registry.cn-shenzhen.aliyuncs.com/neoneone/autowareproj:latest 

## 18.04 with cuda 11.1

docker pull registry.cn-shenzhen.aliyuncs.com/neoneone/autowareproj:ubuntu18.04

# docker run 

docker run --gpus all -p 8080:8080 registry.cn-shenzhen.aliyuncs.com/neoneone/autowareproj:latest

## portainer

添加新的container，设置映射8080端口和env环境设置登录密码AUTHENTICATE_VIA_JUPYTER

## open browser

[http://127.0.0.1:8080/tree?](http://127.0.0.1:8080/tree?)

# 介绍

可以使用jupyter vnc vscode ssh sftp等常用工具，并且只占用一个端口。


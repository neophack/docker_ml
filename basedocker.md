# install docker and nvidia-docker2

需要好的网络

https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html#setting-up-docker

# docker image

## 20.04 with cuda 11.4

docker pull registry.cn-shenzhen.aliyuncs.com/neoneone/autowareproj:latest 

## 18.04 with cuda 11.1

docker pull registry.cn-shenzhen.aliyuncs.com/neoneone/autowareproj:ubuntu18.04

# docker run 

docker run --gpus all -p 8080:8080 registry.cn-shenzhen.aliyuncs.com/neoneone/autowareproj:latest

## open browser

http://127.0.0.1:8080/tree?

# 介绍

可以使用jupyter vnc vscode ssh sftp等常用工具，并且只占用一个端口。


# install docker and nvidia-docker2

## 安装docker

`sudo apt install docker.io`

## 设置非root账号不用sudo直接执行docker命令

创建docker组

`sudo groupadd docker`

将当前用户加入docker组

`sudo gpasswd -a ${USER} docker`

重启docker服务(生产环境请慎用)：

`sudo systemctl restart docker`

添加访问和执行权限：

`sudo chmod a+rw /var/run/docker.sock`

## nvidia docker支持

https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html#setting-up-nvidia-container-toolkit

```
distribution=ubuntu22.04 && curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg \
&& curl -s -L https://nvidia.github.io/libnvidia-container/$distribution/libnvidia-container.list | \
      sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | \
      sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list && apt update
```

pop os dont use `nvidia-container-toolkit/jammy,now 1.8.0-1pop1~1644260705~22.04~60691e5 amd64`

`sudo apt install nvidia-container-toolkit/bionic`

`sudo systemctl restart docker`

`sudo docker run --rm --gpus all nvidia/cuda:11.0.3-base-ubuntu20.04 nvidia-smi`

# docker图形化管理软件，带GPU功能

`docker run -d -p 8800:8000 -p 9900:9000 --name=portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainerci/portainer:pr4791`



## 打开portainer管理页面，设置用户信息

[http://127.0.0.1:9900](http://127.0.0.1:9900)

# 深度学习镜像

## 深度学习训练环境

可以使用jupyter vnc vscode ssh sftp等常用工具，并且只占用一个端口。

```
# Ubuntu 20.04
# Note: Container image 21.06-py3 contains Python 3.8.
# NVIDIA CUDA 11.3.1
# cuBLAS 11.5.1.109
# NVIDIA cuDNN 8.2.1
# NVIDIA NCCL 2.9.9 (optimized for NVLink™ )
# Note: Although NCCL is packaged in the container, it does not affect TensorRT nor inferencing in any way.
# rdma-core 32.1
# OpenMPI 4.1.1rc1
# OpenUCX 1.10.1
# GDRCopy 2.2
# NVIDIA HPC-X 2.8.2rc3
# Nsight Compute 2021.1.0.18
# Nsight Systems 2021.2.1.58
# TensorRT 7.2.3.4
# ml-workspace
# sogou input
# pytorch 1.11
# novnc

# bind port 8080
# bind dir /workspace
# ssh 需要先设置ml账号的密码，即可通过账号密码访问

docker pull registry.cn-shenzhen.aliyuncs.com/neoneone/ml-workspace:ros 
```
## docker run 

`docker run --gpus all -p 8080:8080 registry.cn-shenzhen.aliyuncs.com/neoneone/ml-workspace:ros

## portainer

添加新的container，设置映射8080端口和env环境设置登录密码AUTHENTICATE_VIA_JUPYTER

## open browser

[http://127.0.0.1:8080/tree?](http://127.0.0.1:8080/tree?)


## 深度学习部署环境，主要方便C++单步调试
```
# Ubuntu 20.04
# NVIDIA CUDA 11.3.1
# clion 21.2
# ros2 galactic
# chinese input

# bind port novnc 6901, clion 5678 该端口必须使用https访问
# bind dir /home/project

docker pull registry.cn-shenzhen.aliyuncs.com/neoneone/nvcr-jetbrains-clion:latest 
```




# 安装 docker and nvidia环境

## 安装docker

`sudo apt install docker.io`

### 设置非root账号不用sudo直接执行docker命令

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
      sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list && sudo apt update
```

pop os不要安装这个包 nvidia-container-toolkit/jammy,now 1.8.0-1pop1~1644260705~22.04~60691e5 amd64 会导致新建容器报错

pop os和ubuntu 安装 

`sudo apt install libnvidia-container1/bionic`

`sudo apt install nvidia-container-toolkit/bionic`


重启docker 

`sudo systemctl restart docker`

测试nvidia是否可用 

`sudo docker run --rm --gpus all nvidia/cuda:11.0.3-base-ubuntu20.04 nvidia-smi`

# docker图形化管理软件，带GPU功能

`docker run -d -p 8800:8000 -p 9900:9000 --name=portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainerci/portainer:pr4791`

其中9900为pc端口，9000为docker内服务端口

## 打开portainer管理页面，设置用户信息

[http://127.0.0.1:9900](http://127.0.0.1:9900)

# 深度学习镜像

## 深度学习训练环境

可以使用jupyter vnc vscode ssh sftp等常用工具，并且只占用一个端口。

要求显卡驱动550以上
```
# Ubuntu 22.04 including Python 3.10
# NVIDIA CUDA 12.4
# NVIDIA cuBLAS 12.4.5.8
# NVIDIA cuDNN 9.1.0.70
# NVIDIA NCCL 2.21.5
# NVIDIA RAPIDS™ 24.02
# rdma-core 39.0
# NVIDIA HPC-X 2.18
# OpenMPI 4.1.4+
# GDRCopy 2.3
# TensorBoard 2.9.0
# Nsight Compute 2024.1.0.13
# Nsight Systems 2024.2.1.38
# NVIDIA TensorRT™ 8.6.3
# Torch-TensorRT 2.3.0a0
# NVIDIA DALI® 1.36
# nvImageCodec 0.2.0.7
# MAGMA 2.6.2
# JupyterLab 2.3.2 including Jupyter-TensorBoard
# TransformerEngine 1.5
# PyTorch quantization wheel 2.1.2
# novnc

# bind port 8080 使用-p冒号前面是pc端口可以自己设置，后面是容器服务端口8080
# bind dir /workspace 使用-v冒号前面是pc的文件夹路径，后面容器内的文件夹路径
# ssh 需要先设置ml账号的密码，即可通过账号密码访问

docker pull registry.cn-shenzhen.aliyuncs.com/neoneone/ml-workspace:ros 
```
### docker run 

`docker run --gpus all --name=ml-workspace --restart=always -p 8080:8080 -v /dev/shm:/dev/shm -v 你的pc文件夹:/workspace -e AUTHENTICATE_VIA_JUPYTER=你的登录密码 -e  OPENP2P_TOKEN=你的openp2p的token registry.cn-shenzhen.aliyuncs.com/neoneone/ml-workspace:latest`

### 支持
[MobileVLM](https://github.com/Meituan-AutoML/MobileVLM)

### portainer

添加新的container，设置映射8080端口和env环境设置登录密码AUTHENTICATE_VIA_JUPYTER

### open browser

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
### docker run 

`docker run --gpus all --name=nvcr-clion --restart=always -p 6901:6901 -p 5678:5678 -v /dev/shm:/dev/shm -v 你的pc文件夹:/home/project registry.cn-shenzhen.aliyuncs.com/neoneone/nvcr-jetbrains-clion:latest`

## 注意事项

使用容器时，数据和程序放挂载的文件夹，防止系统损坏导致文件丢失，也防止过多数据导致commit的镜像巨大

--restart=always可以让容器开机启动，否则需要手动启动


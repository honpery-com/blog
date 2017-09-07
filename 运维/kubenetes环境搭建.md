# kubenetes环境搭建

> 最近在研究容器集群相关的技术，本文记录了使用kubeadm搭建kubernetes集群的全过程。

## 前言
1. 因为kubeadm使用的源多数被墙，为了避免国内镜像站的不必要麻烦，所以须自备梯子，[这里](#)有详细的搭建梯子教程（仅为学习交流使用）。
2. 在强调一遍，`fuck gfw`，请执行`curl https://www.youtube.com`看看自己网络是否已经翻墙！！

## 安装docker、kubeadm、kubelet
docker是整个架构的容器引擎。kubeadm是k8s提供的快速安装工具。kubelet用于操作节点上的docker。

docker可以直接使用官方的安装脚本，kubeadm和kubelet需要私有源进行安装，汇总如下，请按照对应操作系统复制粘贴。

```bash
# Ubuntu
apt-get update && apt-get upgrade
wget -qO- https://get.docker.com/ | sh

apt-get update && apt-get install -y apt-transport-https
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -
cat <<EOF >/etc/apt/sources.list.d/kubernetes.list
deb http://apt.kubernetes.io/ kubernetes-xenial main
EOF
apt-get update
apt-get install -y kubelet kubeadm

# centos
yum update
wget -qO- https://get.docker.com/ | sh

cat <<EOF > /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg
        https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
EOF
setenforce 0
yum install -y kubelet kubeadm
systemctl enable kubelet && systemctl start kubelet
```

## 初始化节点
k8s中的节点分为master和node，两者初始化方式相同。

```bash
kubeadm init
```

执行以上命令后，如果网络没有被墙的情况下，最新版本的初始化非常流畅，出现问题请看[这里](#)


---
title: 使用国内源部署k8s
date: 2023-12-01 13:57:39
tags:
---

## 系统
系统：ubuntu20.04
```
Linux 5.15.0-67-generic #74~20.04.1-Ubuntu SMP Wed Feb 22 14:52:34 UTC 2023 x86_64 x86_64 x86_64 GNU/Linux
```

## docker
docker version
```
Client:
 Version:           20.10.21
 API version:       1.41
 Go version:        go1.18.1
 Git commit:        20.10.21-0ubuntu1~20.04.2
 Built:             Thu Apr 27 05:56:19 2023
 OS/Arch:           linux/amd64
 Context:           default
 Experimental:      true

Server:
 Engine:
  Version:          20.10.21
  API version:      1.41 (minimum version 1.12)
  Go version:       go1.18.1
  Git commit:       20.10.21-0ubuntu1~20.04.2
  Built:            Thu Apr 27 05:37:01 2023
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.7.2
  GitCommit:
 runc:
  Version:          1.1.0-0ubuntu1~20.04.2
  GitCommit:
 docker-init:
  Version:          0.19.0
  GitCommit:
```
## 关闭swap
必须关闭，不如kubelet服务会启动失败
在 Ubuntu 系统上关闭 swap 分区可以通过以下步骤完成：

1. **查看当前的 swap 状态：**

   在终端中运行以下命令，以查看当前的 swap 状态：

   ```bash
   sudo swapon --show
   ```

   这将显示当前激活的 swap 分区或文件。

2. **关闭 swap：**

   使用以下命令关闭所有激活的 swap 分区：

   ```bash
   sudo swapoff -a
   ```

   请注意，这会立即关闭所有激活的 swap 分区，但不会在系统重新启动后生效。

3. **永久禁用 swap：**

   若要永久禁用 swap，需要编辑 `/etc/fstab` 文件。打开该文件并注释掉（在行首添加 `#` 符号）与 swap 分区相关的行。

   使用文本编辑器打开 `/etc/fstab` 文件：

   ```bash
   sudo nano /etc/fstab
   ```

   在文件中找到类似于 `/swapfile` 或 `/dev/sdXY none swap sw 0 0` 的行，并在行首添加 `#` 符号，使其成为注释行。保存并退出编辑器。

4. **删除 swap 文件（如果有）：**

   如果你的 swap 是通过文件创建的（而不是分区），你可能还需要删除 swap 文件。首先，查看 swap 文件的位置：

   ```bash
   sudo file -s /swapfile
   ```

   然后，如果是文件而不是分区，请删除它：

   ```bash
   sudo rm /swapfile
   ```

5. **重新加载配置：**

   重新加载系统配置以应用更改：

   ```bash
   sudo sysctl --system
   ```

现在，swap 已经被关闭和禁用。请注意，在某些情况下，系统可能需要 swap 分区来处理内存不足的情况。因此，请在禁用 swap 之前确保你了解系统的内存需求。如果你发现在禁用 swap 后出现内存问题，可以重新启用 swap 或考虑其他解决方案。

## 修改docker镜像源
修改/etc/docker/daemon.json 如下
```
{
  "insecure-registries" : ["192.168.2.166:8080", "blinkz.asuscomm.com:18080"],
  "registry-mirrors": ["https://registry.docker-cn.com","http://hub-mirror.c.163.com","https://docker.mirrors.ustc.edu.cn"],
  "exec-opts": ["native.cgroupdriver=systemd"]
}
```

## 使用阿里源安装k8s套件
```
# 添加并信任APT证书
curl https://mirrors.aliyun.com/kubernetes/apt/doc/apt-key.gpg | sudo apt-key add -

# 添加源地址
add-apt-repository "deb https://mirrors.aliyun.com/kubernetes/apt/ kubernetes-xenial main"

# 更新源并安装最新版 kubenetes
sudo apt update && apt install -y kubelet kubeadm kubectl
```

## 启动master节点
```
kubeadm init --image-repository='registry.cn-hangzhou.aliyuncs.com/google_containers'
```

----------------------------------

## 遇到的问题
### `kubeadm init` 报错 `cri` 相关
期间也遇到了 kubelet无法启动的问题，是相同的原因，是因为 `containerd` 服务版本问题导致的，更新 `containerd` 即可解决
```
sudo apt install containerd
```
* ` ExecStart=/usr/bin/kubelet $KUBELET_KUBECONFIG_ARGS $KUBELET_CONFIG_ARGS $KUBELET_KUBEADM_ARGS $KUBELET_EXTRA_ARGS (code=exited, status=1/FAILURE)`
* kubeadm init 遇到的错误
```
 kubeadm init --image-repository registry.aliyuncs.com/google_containers
W1201 08:57:17.177921  452826 version.go:104] could not fetch a Kubernetes version from the internet: unable to get URL "https://dl.k8s.io/release/stable-1.txt": Get "https://dl.k8s.io/release/stable-1.txt": dial tcp 34.107.204.206:443: connect: connection refused
W1201 08:57:17.177998  452826 version.go:105] falling back to the local client version: v1.28.2
[init] Using Kubernetes version: v1.28.2
[preflight] Running pre-flight checks
error execution phase preflight: [preflight] Some fatal errors occurred:
        [ERROR IsPrivilegedUser]: user is not running as root
[preflight] If you know what you are doing, you can make a check non-fatal with `--ignore-preflight-errors=...`
To see the stack trace of this error execute with --v=5 or higher
```

### 为什么使用国内源
国外源需要翻墙，非常麻烦

### 关于重新安装kubelet
需要将 `/var/lib/kubelet` 目录完全删除
```
# 卸载
sudo apt purge kubelet
sudo rm -rf /var/lib/kubelet

# 重新安装
sudo apt install kubelet kubeadm
```

* ` kubelet_volumes.go:261] "There were many similar errors. Turn up verbosity to see them." `


## Reference
https://gist.github.com/islishude/231659cec0305ace090b933ce851994a
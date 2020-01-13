# KVM入门
---
### 1.基础操作
#### ①.创建虚拟机

```shell
virt-install --name=centos-node1-0113 \
--os-variant=rhel6 --ram=4096 --vcpus=2 \
--disk /data/virtualdisk\centos-node1-0113.img,sparse=false,bus=virtio,size=100 \
--cdrom=/root/Download/CentOS-7-x86_64-Minimal-1810.iso\
--network bridge=br2 \
--vnc --vncport=5920 --vnclisten=0.0.0.0 \
--noautoconsole --force --autostart
```
#### ②.虚拟机相关操作

```shell
# 显示所有虚拟机列表
virsh list --all

# 启动指定虚拟机
virsh start <name>/<id>

# 停止指定虚拟机
virsh shutdown <name>/<id>

# 销毁指定虚拟机(强制断电)
virsh destory <name>/<id>

# 登入指定虚拟机
virsh console <name>/<id>

```
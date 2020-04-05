# KVM入门
---
### 1.基础操作
#### ①.创建虚拟机

```shell
virt-install --name=centos-node1-0113 \
--ram=4096 --vcpus=2 \
--disk /data/virtualdisk\centos-node1-0113.img,sparse=false,bus=virtio,size=100 \
--cdrom=/root/Download/CentOS-7-x86_64-Minimal-1810.iso\
--network bridge=br2 \
--vnc --vncport=5921 --vnclisten=0.0.0.0 \
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

# 为虚拟机添加网卡绑定
virsh attach-interface \
--domain centos_node2-142-5922 \    
--type bridge \
--source br0 \
--model virtio \
--config

# 分离虚拟机网卡绑定
virsh detach-interface \
centos_node2-142-5922 \
bridge \
52:54:00:aa:9c:18

# 查看虚拟机网卡绑定详细信息
virsh  domiflist centos_node2-142-5922

# 查看网桥信息(brctl:网桥管理工具)
brctl show

# 虚拟机管理工具(virt-*)
```

### ③.网桥模式(Bridge)下的网卡配置
+ 宿主机虚拟网桥配置文件详情：

    ```xml
    <!-- 配置文件在kvm-qeum下：/etc/libvirt/qemu/networks/default.xml -->
    <network>
      <name>default</name>
      <uuid>9698a2c1-c8a1-4ec3-8cc3-0a8edebab6c4</uuid>
      <!--  端口转发  -->
      <forward mode='nat'>
        <nat>
          <port start='1024' end='65535'/>
        </nat>
      </forward>
      <!--  网桥配置  -->
      <bridge name='virbr0' stp='on' delay='0'/>
      <mac address='52:54:00:c9:6a:e2'/>
      <ip address='192.168.122.1' netmask='255.255.255.0'>
        <dhcp>
          <range start='192.168.122.2' end='192.168.122.254'/>
        </dhcp>
      </ip>
    </network>
    ```
+ 宿主机网桥

    ```java
    DEVICE=br0
    HWADDR=B8:2A:72:CE:DA:75
    TYPE=Bridge
    UUID=325e2119-f293-4555-b984-ae8ae886b4de
    ONBOOT=yes
    NM_CONTROLLED=no
    BOOTPROTO=static	
    IPADDR=10.106.36.135
    NETMASK=255.255.255.128
    GATEWAY=10.106.36.1
    ```
+ 虚主机网卡

    ```java
    TYPE=Ethernet
    BOOTPROTO=none
    DEFROUTE=yes
    NAME=eth0
    DEVICE=eth0
    NM_CONTROLLED=no
    USERCTL=no
    ONBOOT=yes
    IPADDR=60.206.36.164
    GATEWAY=60.206.36.129
    NETMASK=255.255.255.128
    DNS1=114.114.114.114
    ARPCHECK=no
    ```
    
    
    
### 2.关于宿主机网桥配置
1.物理网卡配置
```shell
TYPE="Ethernet"
BOOTPROTO="no"
DEFROUTE="yes"
NAME="em1"
UUID="d4b9c4cb-b04c-4d91-9e9c-bc1b0c47a416"
DEVICE="em1"
ONBOOT="yes"
BRIDGE="br0"
```
2.网桥配置
```shell
TYPE="Bridge"
BOOTPROTO="static"
DEVICE="br0"
NAME="br0"
ONBOOT="yes"
IPADDR="10.36.24.180"
NETMASK="255.255.255.0"
GATEWAY="10.36.24.1"
NM_CONTROLLED="no"
```
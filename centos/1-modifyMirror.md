# CentOS修改系统镜像源
### 一.备份当前源
```shell
mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
```
### 二、切换阿里云镜像源
+ CentOS7 切换源

```shell
# 进入源所在路径
cd /etc/yum.repos.d/
# 下载阿里云镜像源并保存为源文件
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
```
+ CentOS6 切换源

```shell
# 进入源所在路径
cd /etc/yum.repos.d/
# 下载网易163镜像源并保存为源文件
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.163.com/.help/CentOS6-Base-163.repo
```
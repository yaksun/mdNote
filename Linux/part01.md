## 	01.`Linux`的特点

就是一款操作系统，免费、开源安全、高效、稳定、处理高并发	

## 02.主要发行版本

linux作为内核基础上二次开发 centos\Redhat\ubantu\suse\红旗linux

先有unix,再有的linux

## 03.安装步骤

* bios配置可虚拟化设备
* 分区:最少3个分区  /     /boot     swap(挂载)

## 04.实现功能

* 联网
* tar -zxvf  XXXX 解压
* windows和centos间复制粘贴(安装vmtools)
* windows和centos间做一个共享文件夹(虚拟机设置共享,默认存放/mnt/hgfs下)
## 05.文件系统

* linux世界里,一切皆文件
* 参考文档`https://www.cnblogs.com/zhuchenglin/p/8686924.html`
* linux的目录中有且只有一个根目录
* linux的各个目录存放的内容是规划好的，不要乱放文件
* linux是以文件的形式管理我们的设备
* 了解文件夹目录放什么东西,把硬件映射成文件夹
* proc 、srv、 sys，linux内核，最好别动
## 06.远程登录
*	xshell登录软件 
*	xftp文件上传下载 sftp 22
*	服务器必须开启了sshd服，该服务监听端口22， setup查看
## 07.vi和vim编辑器
* :wq 保存并退出 
* :q 查看后的退出 ,未做修改
* :q! 修改了不想保存,强制退出 
* 正常模式，插入模式，命令行模式
* 快捷键都是在正常模式下使用
* yy复制当前行   p粘贴
## 08.关机、开机和重启
*	shutdown -h now  立即关机
*	shutdown -h 1 表示1分钟后关机
*	shutdown -r now 现在重启计算机
*	halt 关机
*	reboot 重启
*	sync  把内存的数据同步到磁盘
*	不管是重启还是关机，首先要运行sync，把内存中的数据同步到磁盘
## 09.用户登录和注销
*	logout 只在远程登录的情况下使用
*	运行级别
## 10.用户管理
* useradd XXX 添加用户 (默认会添加到同名的组里面)

* useradd -d /home/tiger/ XXX 添加用户到指定组

* passwd XXX 给用户指定密码
## 11.配置自动连接网络

菜单中配置:系统->首选项->网络连接->勾选自动连接

## 12.配置固定ip

 配置好后重启网络服务

* service network restart
* 或者reboot重启机器
* vim etc/sysconfig/network-scripts/ifcfg-eth0

```
DEVICE=eth0
TYPE=Ethernet
UUID=f76e11fa-9445-485d-a4a2-a32c6e18fe94
ONBOOT=yes
NM_CONTROLLED=yes
BOOTPROTO=static
HWADDR=00:0C:29:00:9E:79
DEFROUTE=yes
PEERDNS=yes
PEERROUTES=yes
IPV4_FAILURE_FATAL=yes
IPV6INIT=no
NAME="System eth0"
IPADDR=192.168.222.110
GATEWAY=192.168.222.2
DNS1=192.168.222.2
LAST_CONNECT=1588202952
~ShareLinkModal
~                          
```

## 13.进程指令

```
// 查看父进程
ps -ef | more
// 只查看某种进程
ps -aux | grep sshd
```


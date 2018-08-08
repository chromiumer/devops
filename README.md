# wireguard


Server

1.安装包
以Centos7为例

```
curl -Lo /etc/yum.repos.d/wireguard.repo https://copr.fedorainfracloud.org/coprs/jdoss/wireguard/repo/epel-7/jdoss-wireguard-epel-7.repo
yum -y install epel-release
yum -y install wireguard-dkms wireguard-tools
yum -y update

reboot

```

验证内核模块
```
modprobe wireguard && lsmod | grep wireguard
```

参考：https://www.wireguard.com/install/


Client
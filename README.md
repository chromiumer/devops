
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

2.配置文件
```
cat <<EOF>> /etc/wireguard/wg0.conf
[Interface]
Address = 1.1.1.1/24
SaveConfig = true
PostUp = iptables -A FORWARD -i wg0 -j ACCEPT; iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE;
PostDown = iptables -D FORWARD -i wg0 -j ACCEPT; iptables -t nat -D POSTROUTING -o eth0 -j MASQUERADE;
ListenPort = 12222
PrivateKey = private-key

EOF
```
>ps: private-key 使用wg genkey生成。




参考：https://www.wireguard.com/install/


Client
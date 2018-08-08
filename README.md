
# wireguard


Server端

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
PrivateKey = server-private-key

EOF
```
>ps: server-private-key 使用wg genkey生成。

3.start&stop wg0

wg-quick up wg0
wg-quick down wg0

wg 检查服务
```
interface: wg0
  public key: 3XlABR57aaYwlSD69uAYXn93alg/qUt03wEIjGyAaD0=
  private key: (hidden)
  listening port: 12222
```



参考：https://www.wireguard.com/install/

---

Client端

1.安装 以MacOS为例
```
brew install wireguard-tools
```

2.配置文件
```
cat <<EOF>> /etc/wireguard/wg0.conf
[Interface]
PrivateKey = client-private-key
Address = 1.1.1.1/32
DNS = 8.8.8.8
DNS = 8.8.4.4
DNS = 114.114.114.114

[Peer]
PublicKey = 3XlABR57aaYwlSD69uAYXn93alg/qUt03wEIjGyAaD0=
Endpoint = server-ip:12222
AllowedIPs = 0.0.0.0/0
EOF
```
ps: client-private-key 使用wg genkey生成。

3.start&stop wg0

wg-quick up wg0
wg-quick down wg0


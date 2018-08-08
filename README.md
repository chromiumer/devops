
# wireguard

##WireGuard® is an extremely simple yet fast and modern VPN that utilizes state-of-the-art cryptography. It aims to be faster, simpler, leaner, and more useful than IPSec, while avoiding the massive headache. It intends to be considerably more performant than OpenVPN. WireGuard is designed as a general purpose VPN for running on embedded interfaces and super computers alike, fit for many different circumstances. Initially released for the Linux kernel, it is now cross-platform and widely deployable. It is currently under heavy development, but already it might be regarded as the most secure, easiest to use, and simplest VPN solution in the industry.

Server

1.Install packages(Centos7)

```
curl -Lo /etc/yum.repos.d/wireguard.repo https://copr.fedorainfracloud.org/coprs/jdoss/wireguard/repo/epel-7/jdoss-wireguard-epel-7.repo
yum -y install epel-release
yum -y install wireguard-dkms wireguard-tools
yum -y update

reboot
```

check kernel mod.
```
modprobe wireguard && lsmod | grep wireguard
```

2.config file.
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
>ps: server-private-key //using cmd  wg genkey gen。

3.start&stop wg0
```
wg-quick up wg0
wg-quick down wg0
```

wg //check wg server
```
interface: wg0
  public key: 3XlABR57aaYwlSD69uAYXn93alg/qUt03wEIjGyAaD0=
  private key: (hidden)
  listening port: 12222
```

---

Client

1.Install packages(MacOS)
```
brew install wireguard-tools
```

2.config file.
```
cat <<EOF>> /etc/wireguard/wg0.conf
[Interface]
PrivateKey = client-private-key
Address = 1.1.1.2/32
DNS = 8.8.8.8
DNS = 8.8.4.4
DNS = 114.114.114.114

[Peer]
PublicKey = 3XlABR57aaYwlSD69uAYXn93alg/qUt03wEIjGyAaD0=
Endpoint = server-ip:12222
AllowedIPs = 0.0.0.0/0
PersistentKeepalive = 30
EOF
```
ps: client-private-key  //using cmd  wg genkey gen。

3.start&stop wg0
```
wg-quick up wg0
wg-quick down wg0
```


Ref：https://www.wireguard.com/install/

# Home Server with Wireguard

Date: 2022/09/17\
Description: Understanding and installing Wireguard to setup home Ubuntu server\
Category: Wireguard\
Tag: Setup

## Wireguard Configuration Files

### 在公网的可见服务器
配置文件放在 /etc/wireguard 下，文件名为 server.conf
```editorconfig
[Interface]
# 这里写服务器自己的内网地址和端口
Address = 172.16.0.1
ListenPort = 8000
PrivateKey = 服务器自己的私钥

# packet forwarding
PreUp = sysctl -w net.ipv4.ip_forward=1

# port forwarding Linode 60012 --> server 12345
# 172.105.27.138 Linode服务器外网IP
# -t: table -d: destination -j: jump -p: protocol DNAT/SNAT: destination/source network address translation
# -A: append -D: delete 
PreUp = iptables -t nat -A PREROUTING -d 172.105.27.138 -p tcp --dport 60012 -j DNAT --to 172.16.1.1:12345
PreUp = iptables -t nat -A POSTROUTING -d 172.16.1.1 -p tcp --dport 12345 -j SNAT --to 172.16.0.1

PostDown = iptables -t nat -D  PREROUTING -d 172.105.27.138 -p tcp --dport 60012 -j DNAT --to 172.16.1.1:12345
PostDown = iptables -t nat -D  POSTROUTING -d 172.16.1.1 -p tcp --dport 12345 -j SNAT --to 172.16.0.1

[Peer]
# 这里写客户端的信息
PublicKey = 客户端的公钥
AllowedIPs = 172.16.0.0/16 # 这里写客户端的内网地址， 因为Wireguard自己连接，所以我们只要写一个范围
PersistentKeepalive = 25
```

### 在内网不可见的客户端
```editorconfig
[Interface]
# 这里写客户端自己的信息
Address = 172.16.1.1
ListenPort = 8000
PrivateKey = 客户端自己的私钥

[Peer]
PublicKey = # 可见服务器的公钥
AllowedIPs = 172.16.0.0/16
Endpoint = 172.105.27.138:8000 # 可见服务器的公网IP和端口
PersistentKeepalive = 25
```
```bash
wg-quick [up|down] server.conf
# PreUp|PostDown # 对应了在启动|关闭时执行的命令
```

[深度好文](http://xstarcd.github.io/wiki/Linux/iptables_forward_internetshare.html)
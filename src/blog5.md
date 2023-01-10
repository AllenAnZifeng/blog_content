# 网易云使用指南

Data: 2022/11/17\
Description: 使用中继服务器授权歌曲使用\
Category: Hack\
Tag: NetEase\

## Setup

IOS 设备下载[证书](https://github.com/UnblockNeteaseMusic/server/blob/enhanced/ca.crt)

ShadowRocket 小火箭配置如下，注意将节点备注改成 Netease Music
```
[Rule]
# Netease Music Advertising
DOMAIN,admusicpic.music.126.net,REJECT
DOMAIN,iadmat.nosdn.127.net,REJECT
DOMAIN,iadmusicmat.music.126.net,REJECT
DOMAIN,iadmusicmatvideo.music.126.net,REJECT

# Netease Music
DOMAIN,apm3.music.163.com,Netease Music
DOMAIN,apm.music.163.com,Netease Music
DOMAIN,interface3.music.163.com,Netease Music
DOMAIN,interface.music.163.com,Netease Music
DOMAIN,music.163.com,Netease Music
IP-CIDR,39.105.63.80/32,Netease Music
IP-CIDR,39.105.175.128/32,Netease Music
IP-CIDR,47.100.127.239/32,Netease Music
IP-CIDR,59.111.19.33/32,Netease Music
IP-CIDR,59.111.160.195/32,Netease Music
IP-CIDR,59.111.160.197/32,Netease Music
IP-CIDR,103.126.92.132/32,Netease Music
IP-CIDR,103.126.92.133/32,Netease Music
IP-CIDR,112.13.119.18/32,Netease Music
IP-CIDR,112.13.122.4/32,Netease Music
IP-CIDR,115.236.118.34/32,Netease Music
IP-CIDR,115.236.121.4/32,Netease Music
IP-CIDR,118.24.63.156/32,Netease Music
IP-CIDR,182.92.170.253/32,Netease Music
IP-CIDR,193.112.159.225/32,Netease Music
```


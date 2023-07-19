
# Debian系统 smartdns 配置教程，基于WSL
#### author: bilibili@im-cwuom | date: 2023/7/17


# 如何部署?
## 1. 配置中科大镜像源
``` shell
cat > /etc/apt/sources.list << EOF
deb http://mirrors.ustc.edu.cn/debian stable main contrib non-free non-free-firmware

# deb-src http://mirrors.ustc.edu.cn/debian stable main contrib non-free non-free-firmware
deb http://mirrors.ustc.edu.cn/debian stable-updates main contrib non-free non-free-firmware
# deb-src http://mirrors.ustc.edu.cn/debian stable-updates main contrib non-free non-free-firmware

# deb http://mirrors.ustc.edu.cn/debian stable-proposed-updates main contrib non-free non-free-firmware
# deb-src http://mirrors.ustc.edu.cn/debian stable-proposed-updates main contrib non-free non-free-firmware
EOF
```

``` shell
apt-get update
```

## 2.  安装环境
``` shell
apt install vim -y
apt install smartdns -y
apt install git -y
apt install wget -y
apt install net-tools -y
apt install dnsutils -y
```


## 3. SSL自动续期配置
``` shell
git clone https://github.com/acmesh-official/acme.sh.git
cd ./acme.sh
./acme.sh --install -m [你的邮箱]

alias acme.sh=~/.acme.sh/acme.sh
```

### 配置阿里云账号AccessKey
``` shell
export Ali_Key="sddiwjedfasSDFSFsdaf"
export Ali_Secret="jlsdsddiwjedfasSDFSFkljlfdsaklkjflsa"
```

> 阿里云账号AccessKey申请地址: https://usercenter.console.aliyun.com/#/manage/ak


### 验证域名所有权
``` shell
acme.sh --issue --dns dns_ali -d [你的域名] -d *.[你的域名] --dnssleep
```

### 成功返回示例
```
[Mon Jul 17 11:22:10 PM CST 2023] Using CA: https://acme.zerossl.com/v2/DV90
[Mon Jul 17 11:22:10 PM CST 2023] Multi domain='DNS:[你的域名],DNS:*.[你的域名]'
[Mon Jul 17 11:22:10 PM CST 2023] Getting domain auth token for each domain

[Mon Jul 17 11:22:20 PM CST 2023] Getting webroot for domain='[你的域名]'
[Mon Jul 17 11:22:20 PM CST 2023] Getting webroot for domain='*.[你的域名]'
[Mon Jul 17 11:22:20 PM CST 2023] Adding txt value: OZPZdIQyL00Cs08a9E9YDj0UoR2ZK4dtnOwYAeYFhak for domain:  _acme-challenge.[你的域名]
[Mon Jul 17 11:22:29 PM CST 2023] The txt record is added: Success.
[Mon Jul 17 11:22:29 PM CST 2023] Adding txt value: _cbQoQ-3hzVc0tzSvhoFZBSyDSrP0altPpBQAknTcM0 for domain:  _acme-challenge.[你的域名]
[Mon Jul 17 11:22:34 PM CST 2023] The txt record is added: Success.
[Mon Jul 17 11:22:34 PM CST 2023] Let's check each DNS record now. Sleep 20 seconds first.
[Mon Jul 17 11:22:55 PM CST 2023] You can use '--dnssleep' to disable public dns checks.
[Mon Jul 17 11:22:55 PM CST 2023] See: https://github.com/acmesh-official/acme.sh/wiki/dnscheck
[Mon Jul 17 11:22:55 PM CST 2023] Checking [你的域名] for _acme-challenge.[你的域名]
[Mon Jul 17 11:23:00 PM CST 2023] Domain [你的域名] '_acme-challenge.[你的域名]' success.
[Mon Jul 17 11:23:00 PM CST 2023] Checking [你的域名] for _acme-challenge.[你的域名]
[Mon Jul 17 11:23:01 PM CST 2023] Domain [你的域名] '_acme-challenge.[你的域名]' success.
[Mon Jul 17 11:23:01 PM CST 2023] All success, let's return
[Mon Jul 17 11:23:01 PM CST 2023] Verifying: [你的域名]
[Mon Jul 17 11:23:05 PM CST 2023] Processing, The CA is processing your order, please just wait. (1/30)
[Mon Jul 17 11:23:11 PM CST 2023] Success
[Mon Jul 17 11:23:11 PM CST 2023] Verifying: *.[你的域名]
[Mon Jul 17 11:23:13 PM CST 2023] Processing, The CA is processing your order, please just wait. (1/30)
[Mon Jul 17 11:23:18 PM CST 2023] Success
[Mon Jul 17 11:23:18 PM CST 2023] Removing DNS records.
[Mon Jul 17 11:23:18 PM CST 2023] Removing txt: OZPZdIQyL00Cs08a9E9YDj0UoR2ZK4dtnOwYAeYFhak for domain: _acme-challenge.[你的域名]
[Mon Jul 17 11:23:28 PM CST 2023] Removed: Success
[Mon Jul 17 11:23:28 PM CST 2023] Removing txt: _cbQoQ-3hzVc0tzSvhoFZBSyDSrP0altPpBQAknTcM0 for domain: _acme-challenge.[你的域名]

[Mon Jul 17 11:23:36 PM CST 2023] Removed: Success
[Mon Jul 17 11:23:36 PM CST 2023] Verify finished, start to sign.
[Mon Jul 17 11:23:36 PM CST 2023] Lets finalize the order.
[Mon Jul 17 11:23:36 PM CST 2023] Le_OrderFinalize='https://acme.zerossl.com/v2/DV90/order/vTAkmnaNULEm7_3jOKMw4w/finalize'
[Mon Jul 17 11:23:38 PM CST 2023] Order status is processing, lets sleep and retry.
[Mon Jul 17 11:23:38 PM CST 2023] Retry after: 15
[Mon Jul 17 11:23:54 PM CST 2023] Polling order status: https://acme.zerossl.com/v2/DV90/order/vTAkmnaNULEm7_3jOKMw4w
[Mon Jul 17 11:23:57 PM CST 2023] Downloading cert.
[Mon Jul 17 11:23:57 PM CST 2023] Le_LinkCert='https://acme.zerossl.com/v2/DV90/cert/F5qUPoVIciJSqbDncd1j1w'
[Mon Jul 17 11:24:00 PM CST 2023] Cert success.
-----BEGIN CERTIFICATE-----
MIIECjCCA5CgAwIBAgIPFnF4E352XmBwlKrjuQo4MAoGCCqGSM49BAMDMEsxCzAJ
****gsTG4khP
-----END CERTIFICATE-----
[Mon Jul 17 11:24:00 PM CST 2023] Your cert is in: /root/.acme.sh/[你的域名]_ecc/[你的域名].cer
[Mon Jul 17 11:24:00 PM CST 2023] Your cert key is in: /root/.acme.sh/[你的域名]_ecc/[你的域名].key
[Mon Jul 17 11:24:00 PM CST 2023] The intermediate CA cert is in: /root/.acme.sh/[你的域名]_ecc/ca.cer
[Mon Jul 17 11:24:00 PM CST 2023] And the full chain certs is there: /root/.acme.sh/[你的域名]_ecc/fullchain.cer
```



### 需要记下的信息
- /root/.acme.sh/[你的域名]_ecc/fullchain.cer
- /root/.acme.sh/[你的域名]_ecc/[你的域名].key


## 4. 替换配置文件
``` shell
cat > /etc/smartdns/smartdns.conf << EOF
bind-tcp [::]:53
bind-tls [::]:853
bind [::]:53


# 域名结果缓存
cache-size 32768
cache-persist yes
prefetch-domain yes
serve-expired yes
serve-expired-ttl 259200
# TCP链接空闲超时
tcp-idle-time 300

# 设置审计启用
audit-enable no

bind-cert-file [你的fullchain]
bind-cert-key-file [你的key]

server 2400:3200::1
server 8.8.8.8
server 114.114.114.114
server 119.29.29.29


server-tls 2001:4860:4860::8888 -group abroad
server-tls 1.0.0.1 -group abroad
server-tls 4.2.2.2 -group abroad
nameserver /google.com/abroad
nameserver /google.com.hk/abroad
nameserver /youtube.com/abroad
nameserver /google/abroad

nameserver /github.com/abroad
nameserver /nodeload.github.com/abroad
nameserver /api.github.com/abroad
nameserver /codeload.github.com/abroad
nameserver /raw.github.com/abroad
nameserver /training.github.com/abroad
nameserver /assets-cdn.github.com/abroad
nameserver /documentcloud.github.com/abroad
nameserver /help.github.com/abroad

nameserver /github.global.ssl.fastly.net/abroad
nameserver /raw.githubusercontent.com/abroad
nameserver /pkg-containers.githubusercontent.com/abroad
nameserver /cloud.githubusercontent.com/abroad
nameserver /gist.githubusercontent.com/abroad
nameserver /marketplace-screenshots.githubusercontent.com/abroad
nameserver /repository-images.githubusercontent.com/abroad
nameserver /user-images.githubusercontent.com/abroad
nameserver /desktop.githubusercontent.com/abroad

nameserver /wikipedia.org/abroad

# 测速模式选择
speed-check-mode ping,tcp:53
# 过期缓存服务
serve-expired no
#双栈IP优选
dualstack-ip-selection yes
dualstack-ip-selection-threshold 10
# 上游 TCP DNS
##
# 上游 UDP DNS
server 117.50.10.10 -group china
server 52.80.52.52 -group china
server 117.50.60.30 -group china
server 52.80.60.30 -group china
# 上游 加密 DNS
server-https https://1.0.0.1/dns-query -group gfw
server-https https://1.1.1.1/dns-query -group gfw
# 黑名单IP地址
blacklist-ip 113.197.104.0/23
blacklist-ip 203.208.32.0/19
# 指定域名使用server组 CHINA
nameserver /baidu.com/china
nameserver /cn.bing.com/china

# 指定域名使用server组 Apple
nameserver /a1.mzstatic.com/china
nameserver /a2.mzstatic.com/china
nameserver /a3.mzstatic.com/china
nameserver /a4.mzstatic.com/china
nameserver /a5.mzstatic.com/china
nameserver /adcdownload.apple.com.akadns.net/china
nameserver /adcdownload.apple.com/china
nameserver /appldnld.apple.com/china
nameserver /appldnld.g.aaplimg.com/china
nameserver /appleid.cdn-apple.com/china
nameserver /apps.apple.com/china
nameserver /apps.mzstatic.com/china
nameserver /cdn-cn1.apple-mapkit.com/china
nameserver /cdn-cn2.apple-mapkit.com/china
nameserver /cdn-cn3.apple-mapkit.com/china
nameserver /cdn-cn4.apple-mapkit.com/china
nameserver /cdn.apple-mapkit.com/china
nameserver /cdn1.apple-mapkit.com/china
nameserver /cdn2.apple-mapkit.com/china
nameserver /cdn3.apple-mapkit.com/china
nameserver /cdn4.apple-mapkit.com/china
nameserver /cds-cdn.v.aaplimg.com/china
nameserver /cds.apple.com.akadns.net/china
nameserver /cds.apple.com/china
nameserver /cl1-cdn.origin-apple.com.akadns.net/china
nameserver /cl1.apple.com/china
nameserver /cl2-cn.apple.com/china
nameserver /cl2.apple.com.edgekey.net.globalredir.akadns.net/china
nameserver /cl2.apple.com/china
nameserver /cl3-cdn.origin-apple.com.akadns.net/china
nameserver /cl3.apple.com/china
nameserver /cl4-cdn.origin-apple.com.akadns.net/china
nameserver /cl4-cn.apple.com/china
nameserver /cl4.apple.com/china
nameserver /cl5-cdn.origin-apple.com.akadns.net/china
nameserver /cl5.apple.com/china
nameserver /clientflow.apple.com.akadns.net/china
nameserver /clientflow.apple.com/china
nameserver /configuration.apple.com.akadns.net/china
nameserver /configuration.apple.com/china
nameserver /cstat.apple.com/china
nameserver /dd-cdn.origin-apple.com.akadns.net/china
nameserver /download.developer.apple.com/china
nameserver /gs-loc-cn.apple.com/china
nameserver /gs-loc.apple.com/china
nameserver /gsp10-ssl-cn.ls.apple.com/china
nameserver /gsp11-cn.ls.apple.com/china
nameserver /gsp12-cn.ls.apple.com/china
nameserver /gsp13-cn.ls.apple.com/china
nameserver /gsp4-cn.ls.apple.com.edgekey.net.globalredir.akadns.net/china
nameserver /gsp4-cn.ls.apple.com.edgekey.net/china
nameserver /gsp4-cn.ls.apple.com/china
nameserver /gsp5-cn.ls.apple.com/china
nameserver /gsp85-cn-ssl.ls.apple.com/china
nameserver /gspe19-cn-ssl.ls.apple.com/china
nameserver /gspe19-cn.ls-apple.com.akadns.net/china
nameserver /gspe19-cn.ls.apple.com/china
nameserver /gspe21-ssl.ls.apple.com/china
nameserver /gspe21.ls.apple.com/china
nameserver /gspe35-ssl.ls.apple.com/china
nameserver /iadsdk.apple.com/china
nameserver /icloud-cdn.icloud.com.akadns.net/china
nameserver /icloud.cdn-apple.com/china
nameserver /images.apple.com.akadns.net/china
nameserver /images.apple.com.edgekey.net.globalredir.akadns.net/china
nameserver /images.apple.com/china
nameserver /init-p01md-lb.push-apple.com.akadns.net/china
nameserver /init-p01md.apple.com/china
nameserver /init-p01st-lb.push-apple.com.akadns.net/china
nameserver /init-p01st.push.apple.com/china
nameserver /init-s01st-lb.push-apple.com.akadns.net/china
nameserver /init-s01st.push.apple.com/china
nameserver /iosapps.itunes.g.aaplimg.com/china
nameserver /iphone-ld.apple.com/china
nameserver /is1-ssl.mzstatic.com/china
nameserver /is1.mzstatic.com/china
nameserver /is2-ssl.mzstatic.com/china
nameserver /is2.mzstatic.com/china
nameserver /is3-ssl.mzstatic.com/china
nameserver /is3.mzstatic.com/china
nameserver /is4-ssl.mzstatic.com/china
nameserver /is4.mzstatic.com/china
nameserver /is5-ssl.mzstatic.com/china
nameserver /is5.mzstatic.com/china
nameserver /itunes-apple.com.akadns.net/china
nameserver /itunes.apple.com/china
nameserver /itunesconnect.apple.com/china
nameserver /mesu-cdn.apple.com.akadns.net/china
nameserver /mesu-china.apple.com.akadns.net/china
nameserver /mesu.apple.com/china
nameserver /music.apple.com/china
nameserver /ocsp-lb.apple.com.akadns.net/china
nameserver /ocsp.apple.com/china
nameserver /oscdn.apple.com/china
nameserver /oscdn.origin-apple.com.akadns.net/china
nameserver /pancake.apple.com/china
nameserver /pancake.cdn-apple.com.akadns.net/china
nameserver /phobos.apple.com/china
nameserver /prod-support.apple-support.akadns.net/china
nameserver /reserve-prime.apple.com/china
nameserver /s.mzstatic.com/china
nameserver /stocks-sparkline-lb.apple.com.akadns.net/china
nameserver /store.apple.com.edgekey.net.globalredir.akadns.net/china
nameserver /store.apple.com.edgekey.net/china
nameserver /store.apple.com/china
nameserver /store.storeimages.apple.com.akadns.net/china
nameserver /store.storeimages.cdn-apple.com/china
nameserver /support-china.apple-support.akadns.net/china
nameserver /support.apple.com/china
nameserver /swcatalog-cdn.apple.com.akadns.net/china
nameserver /swcatalog.apple.com/china
nameserver /swcdn.apple.com/china
nameserver /swcdn.g.aaplimg.com/china
nameserver /swdist.apple.com.akadns.net/china
nameserver /swdist.apple.com/china
nameserver /swscan-cdn.apple.com.akadns.net/china
nameserver /swscan.apple.com/china
nameserver /updates-http.cdn-apple.com.akadns.net/china
nameserver /updates-http.cdn-apple.com/china
nameserver /updates.cdn-apple.com/china
nameserver /valid.apple.com/china
nameserver /valid.origin-apple.com.akadns.net/china
nameserver /www.apple.com.edgekey.net.globalredir.akadns.net/china
nameserver /www.apple.com.edgekey.net/china
nameserver /www.apple.com/china
EOF
```

## 5. 重启服务
``` shell
service smartdns restart
```

----


# 如何使用？
## 1. 查看WSL的IPv4、IPv6地址
```
root@cwuom:~# ifconfig
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.1.2  netmask 255.255.255.0  broadcast 192.168.1.255
        inet6 2409:8a28:6071:ec60:e33e:4393:449c:6262  prefixlen 64  scopeid 0x0<global>
        inet6 2409:8a28:6071:ec60:e05d:e14f:fc12:5712  prefixlen 128  scopeid 0x0<global>
        inet6 fe80::1e39:50c7:2ac2:1a9f  prefixlen 64  scopeid 0xfd<compat,link,site,host>
        ether 04:42:1a:08:0e:9b  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

eth2: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 172.26.80.1  netmask 255.255.240.0  broadcast 172.26.95.255
        inet6 fe80::62ef:8f39:bb70:3837  prefixlen 64  scopeid 0xfd<compat,link,site,host>
        ether 00:15:5d:8c:44:a0  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

eth3: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.96.1  netmask 255.255.240.0  broadcast 192.168.111.255
        inet6 fe80::1a00:9ed0:37eb:3cd4  prefixlen 64  scopeid 0xfd<compat,link,site,host>
        ether 00:15:5d:19:e4:25  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 1500
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0xfe<compat,link,site,host>
        loop  (Local Loopback)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

```

## 2. 在其他设备中配置DNS并将IPv4、IPv6指向WSL
具体操作方式可自行搜索，此处不赘述。

## 常见问题
### ifconfig输出的地址不在同一网段且ping不通怎么办？
这是wsl2的特性，将wsl2降级为wsl1即可解决。
```
wsl -l -v
wsl --set-version <distribution name> 1
```

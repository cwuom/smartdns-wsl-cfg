

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

> 中科大源不是必须的，如果你网的质量比较好甚至可以用默认镜像，若其他源有更快的下载速度，可使用其他源替代。

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

### 配置阿里云账号AccessKey，其他平台购入的域名请自行检索相关页面。
``` shell
export Ali_Key="sddiwjedfasSDFSFsdaf"
export Ali_Secret="jlsdsddiwjedfasSDFSFkljlfdsaklkjflsa"
```

> 阿里云账号AccessKey申请地址: https://usercenter.console.aliyun.com/#/manage/ak
> 腾讯云请参考文章: [如何获取阿里云、腾讯云Access Key - 简书 (jianshu.com)](https://www.jianshu.com/p/ac489e7e779f)


### 验证域名所有权
``` shell
acme.sh --issue --dns dns_ali -d [你的域名] -d *.[你的域名] --dnssleep
```
> 腾讯云请参考文章: [使用acme.sh申请Let's Encrypt免费的SSL证书-腾讯云开发者社区-腾讯云 (tencent.com)](https://cloud.tencent.com/developer/article/1877928)

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
> 上述配置文件可能比较杂乱，有能力的建议自己根据使用场景整理一份。

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

### 对网络有什么要求吗？
有，使用请先行前往路由器/光猫管理界面开启IPv4/v6的支持。纯IPv4可能无法直接访问Google之类解析返回为纯IPv6地址的网站。

# 进阶
## 绕过SNI阻断
这里只是常见问题，更多信息请前往[URenko/Accesser: 🌏一个解决SNI RST导致维基百科、Pixiv等站点无法访问的工具 | A tool for solving SNI RST (github.com)](https://github.com/URenko/Accesser)查看

### 1. 我在局域网的其它设备中搭建了此项目，我该如何应用到除这个设备外的的平台？
-  在Accesser运行根目录创建一个名为pac且不带后缀的文件。
```js
var domains = {
  "apkmirror.com": 1,
  "appledaily.com": 1,
  "archiveofourown.org": 1,
  "artstation.com": 1,
  "bbc.com": 1,
  "disqus.com": 1,
  "dmc.nico": 1,
  "dropbox.com": 1,
  "dropboxapi.com": 1,
  "dropbox-dns.com": 1,
  "dw.com": 1,
  "e-hentai.org": 1,
  "epochtimes.com": 1,
  "euronews.com": 1,
  "exhentai.org": 1,
  "ftchinese.com": 1,
  "github.com": 1,
  "githubassets.com": 1,
  "githubusercontent.com": 1,
  "imgur.com": 1,
  "instagram.com": 1,
  "i.pximg.net": 1,
  "kobo.com": 1,
  "medium.com": 1,
  "mega.nz": 1,
  "nicovideo.jp": 1,
  "nyaa.si": 1,
  "nytimes.com": 1,
  "phncdn.com": 1,
  "pinterest.com": 1,
  "pixiv.net": 1,
  "pornhub.com": 1,
  "quora.com": 1,
  "redd.it": 1,
  "reddit.com": 1,
  "redditmedia.com": 1,
  "redditstatic.com":1,
  "startpage.com": 1,
  "steamcommunity.com": 1,
  "theepochtimes.com": 1,
  "thetvdb.com": 1,
  "tumblr.com": 1,
  "tumblr.co": 1,
  "uptodown.com": 1,
  "vimeo.com": 1,
  "wenxuecity.com": 1,
  "store.steampowered.com": 1,
  "wikipedia.org": 1
};

var shexps = {
  "*://api.openai.com/*": 1,
  "*://steamcommunity-a.akamaihd.net/*": 1,
  "*://steamuserimages-a.akamaihd.net/*": 1,
  "*://*.amazon.co.jp/*": 1,
  "*://*onedrive.live.com/*": 1,
  "*://*.bbc.co.uk/*": 1,
  "*://*.bbci.co.uk/*": 1,
  "*://*.japantimes.co.jp/*": 1,
  "*://*.yahoo.co.jp/*": 1,
  "*://*.cna.com.tw/*": 1,
  "*://*.discord.com/*": 1,
  "*://*.discordapp.com/*": 1,
  "*://*.discord.gg/*": 1,
  "*://media.discordapp.net/*": 1,
  "*://*.duckduckgo.com/*": 1,
  "*://*.v2ex.com/*":1,
  "*://*.twitch.tv/*":1
};

var proxy = "PROXY 192.168.1.3:7654;";

var direct = 'DIRECT;';

var hasOwnProperty = Object.hasOwnProperty;

function shExpMatchs(str, shexps) {
    for (shexp in shexps) {
        if (shExpMatch(str, shexp)) {
            return true;
        }
    }
    return false;
}

function FindProxyForURL(url, host) {
    var suffix;
    var pos = host.lastIndexOf('.');
    pos = host.lastIndexOf('.', pos - 1);
    while(1) {
        if (pos <= 0) {
            if (hasOwnProperty.call(domains, host)) {
                return proxy;
            } else if (shExpMatchs(url, shexps)) {
                return proxy;
            } else {
                return direct;
            }
        }
        suffix = host.substring(pos + 1);
        if (hasOwnProperty.call(domains, suffix)) {
            return proxy;
        }
        pos = host.lastIndexOf('.', pos - 1);
    }
}

```
- 将pac文件中的"192.168.1.3:7654"指向你局域网中正在使用此项目的服务器的IP和端口。
- 在另一个设备中安装证书，并在代理中启用"使用设置脚本"，并输入目标服务器IP和端口

### 证书问题、不安全的站点
- 请到此项目原作者写的的[FAQ · URenko/Accesser Wiki (github.com)](https://github.com/URenko/Accesser/wiki/FAQ)中查看


# 误区
请注意，部署这些≠梯子。您的IP自始至终都不会改变，上述操作只能加速国内外网站的解析速度，例如 google.com 依旧无法直接访问，www.google.com.hk 的连接稳定性也不好保证。制作这个的目的还是为了防止污染让一些本该能正常访问的境外服务能够直连，而不是用来专门看外网的工具（这就是为什么还要设置国内DNS）

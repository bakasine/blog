---
title: Clash 基础用法
date: 2023-08-29 14:43:59
tags: 
    - clash
    - cfw
categories: 
    - note
---

[安装](#ins)
[修改内核](#core)
[如何开启tun模式](#tun)
[配置模板](#template)

# <h2 id="ins">安装</h2>

`Meta内核`

[__Clash.Meta__](https://github.com/MetaCubeX/Clash.Meta/releases)

`客户端`

[__ClashX.Meta__](https://github.com/MetaCubeX/ClashX.Meta/releases)
[__Clash for Windows__](https://github.com/Fndroid/clash_for_windows_pkg/releases)
[__Clash Verge__](https://github.com/zzzgydi/clash-verge/releases)

# <h2 id="core">修改内核</h2>

__Mac上可以直接使用ClashX.Meta原生支持3种Clash Core, 如果使用Clash for Windows默认不支持Meta Core, 但是可以手动更换__

用Meta.Clash的内核更换

`resources` -> `static` -> `files` -> `win` -> `x64` -> `clash-win64.exe`

# <h2 id="tun">如何开启tun模式</h2>

`Clash for Windows`

__Clash for Windows要启动tun模式需要安装Service Mode, 但是由于他做了内核校验我们更换成Meta内核后会安装失败__
__所以可以考虑使用管理员打开应用强行启动tun模式__

`Clash Verge`

__Verge有便携版和安装包,根据issue反馈发现便携版开启tun会有一些问题,而使用安装包则不会出现问题__

# <h2 id="template">配置模板</h2>

```yaml
#---------------------------------------------------#
## 配置文件需要放置在 $HOME/.config/clash/*.yaml

## 这份文件是clashX的基础配置文件，请尽量新建配置文件进行修改。
## ！！！只有这份文件的端口设置会随ClashX启动生效

## 如果您不知道如何操作，请参阅 官方Github文档 https://github.com/Dreamacro/clash/blob/dev/README.md
#---------------------------------------------------#

# (HTTP and SOCKS5 in one port)
mixed-port: 7890
external-controller: 127.0.0.1:9090
allow-lan: true
mode: rule
log-level: silent

tun:
  enable: false
  stack: system # gvisor / lwip / system
  dns-hijack:
    - 0.0.0.0:53 # 需要劫持的 DNS
  inet4-route-address: # 启用 auto_route 时使用自定义路由而不是默认路由
    - 0.0.0.0/1
    - 128.0.0.0/1
  inet6-route-address: # 启用 auto_route 时使用自定义路由而不是默认路由
    - "::/1"
    - "8000::/1"

dns:
  enable: true
  prefer-h3: true
  listen: 0.0.0.0:53
  ipv6: false
  default-nameserver:
    - 114.114.114.114
  nameserver:
    - tls://223.5.5.5:853
    - 114.114.114.114
    - 119.29.29.29
    - 180.76.76.76
  enhanced-mode: fake-ip
  fake-ip-range: 198.18.0.1/16
  fallback:
    - tls://8.8.4.4
    - tls://1.1.1.1
  fake-ip-filter:
    - "*.lan"
    - "*.localdomain"
    - "*.example"
    - "*.invalid"
    - "*.localhost"
    - "*.test"
    - "*.local"
    - "*.home.arpa"
    - router.asus.com
    - localhost.sec.qq.com
    - localhost.ptlogin2.qq.com
    - "+.msftconnecttest.com"

proxies:
  # Demo
  - name: "Demo"
    type: trojan
    server: Demo
    port: 443
    password: Demo
    # udp: true
    # sni: example.com # aka server name
    alpn:
      - h2
      - http/1.1
    # skip-cert-verify: true

proxy-groups:
  # 代理节点选择
  - name: "PROXY"
    type: select
    proxies:
      - "Demo"

  # 白名单模式 PROXY，黑名单模式 DIRECT
  - name: "Final"
    type: select
    proxies:
      - "DIRECT"
      - "PROXY"

  - name: "Bilibili"
    type: select
    proxies:
      - "DIRECT"
      - "PROXY"

script:
  code: |
    def main(ctx, metadata):
      # Log ProcessName
      ctx.log('Process Name: ' + ctx.resolve_process_name(metadata))
      return 'DIRECT'

rule-providers:
  bilibili:
    type: http
    behavior: classical
    url: "https://raw.githubusercontent.com/blackmatrix7/ios_rule_script/master/rule/Clash/BiliBili/BiliBili.yaml"
    path: ./ruleset/bilibili.yaml
    interval: 86400

  reject:
    type: http
    behavior: classical
    url: "https://raw.githubusercontent.com/blackmatrix7/ios_rule_script/master/rule/Clash/Advertising/Advertising_Classical.yaml"
    path: ./ruleset/reject.yaml
    interval: 86400

  privacy:
    type: http
    behavior: classical
    url: "https://raw.githubusercontent.com/blackmatrix7/ios_rule_script/master/rule/Clash/Privacy/Privacy_Classical.yaml"
    path: ./ruleset/privacy.yaml
    interval: 86400

  hijacking:
    type: http
    behavior: classical
    url: "https://raw.githubusercontent.com/blackmatrix7/ios_rule_script/master/rule/Clash/Hijacking/Hijacking.yaml"
    path: ./ruleset/hijacking.yaml
    interval: 86400

  icloud:
    type: http
    behavior: classical
    url: "https://cdn.jsdelivr.net/gh/blackmatrix7/ios_rule_script@master/rule/Clash/iCloud/iCloud.yaml"
    path: ./ruleset/icloud.yaml
    interval: 86400

  apple:
    type: http
    behavior: domain
    url: "https://cdn.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/apple.txt"
    path: ./ruleset/apple.yaml
    interval: 86400

  google:
    type: http
    behavior: domain
    url: "https://cdn.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/google.txt"
    path: ./ruleset/google.yaml
    interval: 86400

  proxy:
    type: http
    behavior: domain
    url: "https://cdn.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/proxy.txt"
    path: ./ruleset/proxy.yaml
    interval: 86400

  direct:
    type: http
    behavior: domain
    url: "https://cdn.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/direct.txt"
    path: ./ruleset/direct.yaml
    interval: 86400

  private:
    type: http
    behavior: domain
    url: "https://cdn.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/private.txt"
    path: ./ruleset/private.yaml
    interval: 86400

  gfw:
    type: http
    behavior: domain
    url: "https://cdn.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/gfw.txt"
    path: ./ruleset/gfw.yaml
    interval: 86400

  greatfire:
    type: http
    behavior: domain
    url: "https://cdn.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/greatfire.txt"
    path: ./ruleset/greatfire.yaml
    interval: 86400

  tld-not-cn:
    type: http
    behavior: domain
    url: "https://cdn.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/tld-not-cn.txt"
    path: ./ruleset/tld-not-cn.yaml
    interval: 86400

  telegramcidr:
    type: http
    behavior: ipcidr
    url: "https://cdn.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/telegramcidr.txt"
    path: ./ruleset/telegramcidr.yaml
    interval: 86400

  cncidr:
    type: http
    behavior: ipcidr
    url: "https://cdn.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/cncidr.txt"
    path: ./ruleset/cncidr.yaml
    interval: 86400

  lancidr:
    type: http
    behavior: ipcidr
    url: "https://cdn.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/lancidr.txt"
    path: ./ruleset/lancidr.yaml
    interval: 86400

  applications:
    type: http
    behavior: classical
    url: "https://cdn.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/applications.txt"
    path: ./ruleset/applications.yaml
    interval: 86400

  my-direct:
    type: http
    behavior: classical
    url: "https://raw.githubusercontent.com/bakasine/rules/master/clash/my-direct.yaml"
    path: ./ruleset/my-direct.yaml
    interval: 86400

  my-proxy:
    type: http
    behavior: classical
    url: "https://raw.githubusercontent.com/bakasine/rules/master/clash/my-proxy.yaml"
    path: ./ruleset/my-proxy.yaml
    interval: 86400

  my-reject:
    type: http
    behavior: classical
    url: "https://raw.githubusercontent.com/bakasine/rules/master/clash/my-reject.yaml"
    path: ./ruleset/my-reject.yaml
    interval: 86400

  Optimization:
    type: http
    behavior: classical
    url: "https://raw.githubusercontent.com/bakasine/rules/master/clash/optimization.yaml"
    path: ./ruleset/optimization.yaml
    interval: 86400

rules:
  # REJECT
  - RULE-SET,reject,REJECT
  - RULE-SET,privacy,REJECT
  - RULE-SET,hijacking,REJECT
  - RULE-SET,my-reject,REJECT
  # CUSTOM
  - RULE-SET,my-direct,DIRECT
  - RULE-SET,bilibili,Bilibili
  - RULE-SET,Optimization,Opt
  # PROXY
  - RULE-SET,my-proxy,PROXY
  - RULE-SET,icloud,PROXY
  - RULE-SET,telegramcidr,PROXY
  - RULE-SET,proxy,PROXY
  # DIRECT
  - RULE-SET,applications,DIRECT
  - RULE-SET,private,DIRECT
  - RULE-SET,apple,DIRECT
  - RULE-SET,google,DIRECT
  - RULE-SET,direct,DIRECT
  - RULE-SET,lancidr,DIRECT
  - RULE-SET,cncidr,DIRECT
  - GEOIP,LAN,DIRECT
  - GEOIP,CN,DIRECT
  # FINAL
  - MATCH,Final
```
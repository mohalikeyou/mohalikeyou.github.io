---
title : 如何优雅的使用ClashForWindows
date : 2022/9/30
tags : 摆弄
---

本文参考了以下文章

[Introduce](https://lancellc.gitbook.io/clash/clash-config-file/proxy-groups/auto)

# 优雅姿势

- 设置匹配关键字git, github，走代理

- 开启TUN模式和Mixin模式

- Gobal + 代理节点 + TUN模式，接管系统所有流量走代理

- 每次非正常关闭clash，都需要重新打开TUN和Mixin模式

# Tun模式

    接管系统所有流量，包括终端，UWP等

# Mixin模式

    这个是部分覆盖clash的配置文件，覆盖行为发生在内存中，也就说：用来指定我们自己的路由规则；注意`缩进`关系！

- Rules规则如下：

```yaml
mixin: # object
  rule-providers:
    reject:
      type: http
      behavior: domain
      url: "https://cdn.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/reject.txt"
      path: ./ruleset/reject.yaml
      interval: 86400

    icloud:
      type: http
      behavior: domain
      url: "https://cdn.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/icloud.txt"
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



  rules:
    - DOMAIN-KEYWORD, git, Proxies
    - DOMAIN-KEYWORD, github, Proxies
    - RULE-SET,applications,DIRECT
    - DOMAIN,clash.razord.top,DIRECT
    - DOMAIN,yacd.haishan.me,DIRECT
    - RULE-SET,private,DIRECT
    - RULE-SET,reject,REJECT
    - RULE-SET,icloud,Proxies
    - RULE-SET,apple,Proxies
    - RULE-SET,google,Proxies
    - RULE-SET,proxy,Proxies
    - RULE-SET,direct,DIRECT
    - RULE-SET,lancidr,DIRECT
    - RULE-SET,cncidr,DIRECT
    - RULE-SET,telegramcidr,Proxies
    - GEOIP,LAN,DIRECT
    - GEOIP,CN,DIRECT
    - MATCH,Proxies
```

# Diff功能

    这个功能，可以在每次更新订阅时，固定对订阅中的某些部分进行修改，例如我可以把部分节点删除，那么以后每次更新，都会删除这个节点

# 规则集

规则集来自于GitHub中的[rules](https://github.com/Loyalsoldier/clash-rules)

# Proxies

这个是接管流量的分组，存在套娃的情况，我们一般都是走rule模式，除非接管全局流量走代理或这直连，才使用global模式

# 配置文件

Clash中的各节点的信息以及具体配置，都来自于这个文件，在这个文件中我们可以设置`url-test` ，`tolerance` ，`interval` 

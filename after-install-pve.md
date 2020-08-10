某些特殊情况下会优先监听IPV6地址

要使用ipv4 连接优先而不必禁用ipv6，需要修改gai.conf配置文件使其生效。

修改/etc/gai.conf，取消下面这一行的注释

````precedence ::ffff:0:0/96 100````

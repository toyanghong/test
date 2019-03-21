### 安装说明
- https://hub.docker.com/r/teddysun/shadowsocks-libev?ref=login
- ` docker run -d -p 9000:9000 -p 9000:9000/udp --restart=always --name ss-libev -v /etc/shadowsocks-libev:/etc/shadowsocks-libev teddysun/shadowsocks-libev `
- 配置文件 ("fast_open":false enble have bug )` /etc/shadowsocks-libev/config.json `
```
{
    "server":"0.0.0.0",
    "server_port":9000,
    "password":"password0",
    "timeout":300,
    "method":"chacha20",
    "fast_open":false,
    "nameserver":"8.8.8.8",
    "mode":"tcp_and_udp"
}
```

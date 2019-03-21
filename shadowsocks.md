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
- 日志查看 ` docker logs id`

### 版本

` docker run --privileged -dt --name ss -p 6443:6443 -p 6443:6443/udp -p 6500:6500/udp -e SS_CONFIG="-s 0.0.0.0 -p 6443 -m chacha20 -k test123 -u " -e KCP_MODULE="kcpserver" -e KCP_CONFIG="-t 127.0.0.1:6443 -l :6500 -mode fast2" -e KCP_FLAG="true" mritd/shadowsocks:3.2.4-20190321 -r /dev/urandom
`

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

### mritd/shadowsocks版本(3.2.4-20190321) 当前使用
`注意tag是 3.2.4-20190321 `

- 执行以下命令 无需额外配置即可连接ss 如需kcp需要kcp客户端配置

` docker run -dt --name ssserver -p 6443:6443 -p 6500:6500/udp mritd/shadowsocks:3.2.4-20190321  -m "ss-server" -s "-s 0.0.0.0 -p 6443 -m chacha20-ietf-poly1305 -k test123" -x -e "kcpserver" -k "-t 127.0.0.1:6443 -l :6500 -mode fast2"
`

-客户端安装下载exe 配置参ss.jpg
` 本地监听端口可随便设置 与ss对应即可 kcp填写的服务器端口是udp 6500 端口 《《《《《《《《《《《注意`


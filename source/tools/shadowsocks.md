# shadowsocks

refer https://www.linuxbabe.com/ubuntu/shadowsocks-libev-proxy-server-ubuntu-16-04-17-10

1. server install in ubuntu 18.04

```bash
apt install -y shadowsocks-libev
systemctl start shadowsocks-libev
systemctl enable shadowsocks-libev
```

/etc/shadowsocks-libev/config.json
```json
{
    "server":"0.0.0.0",
    "server_port":8388,
    "local_port":1080,
    "password":"PenJetmef0",
    "timeout":60,
    "method":"chacha20-ietf-poly1305"
}
```

2. client install in ubuntu 18.04

```bash
apt install -y shadowsocks-libev
systemctl start shadowsocks-libev-local@ss-server.service
```

/etc/shadowsocks-libev/ss-server.json
```json
{
 "server":"www.iefcu.cn",
 "server_port":8388,
 "local_address":"127.0.0.1",
 "local_port":1080,
 "password":"PenJetmef0",
 "timeout":60,
 "method":"chacha20-ietf-poly1305"
}
```

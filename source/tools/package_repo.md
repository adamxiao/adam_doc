# package repo mirrors

## pip

~/.pip/pip.conf
```ini
[global]
index-url = https://mirrors.aliyun.com/pypi/simple/
```

```ini
[global]
index-url = http://pypi.douban.com/simple/
trusted-host = pypi.douban.com
```

or template use
```
pip install -i https://mirrors.aliyun.com/pypi/simple/ gevent
```

## ubuntu

/etc/apt/sources.list
```
```

### 使用Alpine 安装 homeAssistant

修改源
```
sed -i 's/dl-cdn.alpinelinux.org/mirrors.ustc.edu.cn/g' /etc/apk/repositories
```


创建requirements.txt文件

```
# Home Assistant core
aiohttp==3.7.3
astral==1.10.1
async_timeout==3.0.1
attrs==19.3.0
awesomeversion==21.2.2
bcrypt==3.1.7
certifi>=2020.12.5
ciso8601==2.1.3
httpx==0.16.1
jinja2>=2.11.3
PyJWT==1.7.1
cryptography==3.2
pip>=8.0.3,<20.3
python-slugify==4.0.1
pytz>=2021.1
pyyaml==5.4.1
requests==2.25.1
ruamel.yaml==0.15.100
voluptuous==0.12.1
voluptuous-serialize==2.4.0
yarl==1.6.3


#Mobile App
PyNaCl==1.3.0
hass-nabucasa

#MySQL
mysqlclient

# MariaDB
pymysql

# homeassistant.components.frontend
home-assistant-frontend

# homeassistant.components.homekit
HAP-python==3.3.0

# homeassistant.components.http
aiohttp_cors==0.7.0

# homeassistant.components.media_player.gpmdp
websocket-client==0.54.0

# homeassistant.components.mqtt
paho-mqtt==1.5.1

# homeassistant.components.notify.html5
pywebpush==1.9.2

# homeassistant.components.cast
pychromecast==8.1.0

# homeassistant.components.zeroconf
zeroconf==0.28.8

# homeassistant.components.otp
pyotp==2.3.0

```
安装

```
apk add --no-cache git mariadb-connector-c-dev python3 py3-pip ca-certificates libffi-dev libssl1.1 libressl-dev && \
    pip3 install --upgrade --no-cache-dir pip && \
    apk add --no-cache --virtual=build-dependencies build-base linux-headers python3-dev tzdata mariadb-dev && \
    pip config set global.index-url https://mirrors.aliyun.com/pypi/simple && \
    pip3 install --no-cache-dir homeassistant && \
    pip3 install --no-cache-dir -r requirements.txt --use-feature=2020-resolver
```

安装后运行

```
hass --open-ui
```

发现报错,缺少pillow库

踩坑: 安装pillow需安装依赖

```
apk --update add libxml2-dev libxslt-dev libffi-dev gcc musl-dev libgcc openssl-dev curl
apk add jpeg-dev zlib-dev freetype-dev lcms2-dev openjpeg-dev tiff-dev tk-dev tcl-dev
pip3 install Pillow==8.1.0
```

再次运行
```
ERROR (MainThread) [homeassistant.components.dhcp] Cannot watch for dhcp packets without a functional packet filter: libpcap is not available. Cannot compile filter !
```
还缺少libpcap 安装

```
apk add libpcap
```

至此成功,卸载依赖
```
apk del build-dependencies && \
    rm -rf /tmp/* /var/tmp/* /var/cache/apk/*
```


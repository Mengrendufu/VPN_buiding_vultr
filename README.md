# VPN_buiding_vultr

About how to build a VPN via a server from vultr.

The project that uses shadowsocks with v2ray-plugin to build a VPN meets
some problem, so here we use the v2ray to build a VPN directly.

## Deploy the server from Vultr

*Type*: Shared CPU
*Price*: Choose a 10-USD/month one
*System*: Ubuntu 20.04 x64
*Others*: disable the auto-backup option

## Info aquired from Vultr server

*IP address*:
*Username*: root
*Password*:

## Deployment steps

### 1 - Conneting to the server via ssh

```
ssh root@<IP address>
password is aquired from Vultr server infomation
```

### 2 - Deploying v2ray server

#### Download the v2ray:

```
wget https://raw.githubusercontent.com/v2fly/fhs-install-v2ray/master/install-release.sh
sudo bash install-release.sh
```

---

*note*: the file *install-release.sh* can be downloaded manually from github reposity: v2fly/fhs-install-v2ray

---

Default config file is located in:

```
/usr/local/etc/v2ray/config.json
```

#### Config the v2ray config.json

copy(default overwrite) the local config.json(modified previously) to the remote server:

```
scp <path_to>/config.json root@<IP address>:/usr/local/etc/v2ray/
```

#### Restart the v2ray server

```
sudo systemctl restart v2ray
```

#### Uncomplicated Fire Wall release

```
sudo ufw allow <port>
```

### 3 - Config the v2rayN client

download *v2rayN-windows-64-SelfContained.zip* from anywhere

unzip the compress package and run the *v2rayN.exe*

*Server* --> *Adding [VMess] server*, finish the *address*, *port* and *ID*

Test the delay and speed of server, then select *System proxy* --> *auto configurate System proxy*

And done.

# APPENDIX

## Content of config.json

```json
{
    "log": {
        "access": "/var/log/v2ray/access.log",
        "error": "/var/log/v2ray/error.log",
        "loglevel": "warning"
    },
    "inbound": {
        "port": 23333,
        "listen": "0.0.0.0",
        "protocol": "vmess",
        "settings": {
            "auth": null,
            "udp": false,
            "ip": null,
            "clients": [
                {
                    "id": "0f8b2c90-eca1-4726-92ca-7daf243e1c27",
                    "alterId": 0,
                    "security": null
                }
            ]
        }
    },
    "outbounds": [
        {
            "protocol": "freedom",
            "settings": {}
        }
    ]
}
```

### Generation of clients UUID

```
sudo uuidgen
```

## Status checking cmd

Net status

```
ss -tulnp
```

v2ray status

```
systemctl status v2ray
```

## Secure Shell Host protocal commands

delete public key record of specific host in *known_hosts* file

```
ssh-keygen -R <IP address>
```

## Checking IP commands

```
curl ipinfo.io/ip
```

## Check the content of a text file in command line

```
cat <txt file>
```

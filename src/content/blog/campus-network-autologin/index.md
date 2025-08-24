---
title: 'Openwrt自动认证登录校园网'
description: '使用Openwrt运行定时任务，进行网络状态检测及校园网认证登录'
publishDate: '2025-08-23'
heroImage: { src: './thumbnail.jpg', color: '#64574D' }
tags: ['openwrt']
language: '中文'
draft: false
comment: true
---

## 前言

华北电力大学的校园网认证是向服务器发送一个POST请求，本教程使用Python代码实现这一过程。

## 准备工作

1. 安装Openwrt系统的路由器或内网服务器

2. Python2/3环境(Openwrt自带)

3. 升级pip并安装Python库

```bash
pip install --upgrade pip
pip install requests
```

## 宿舍端登录

ssh连接Openwrt

```bash
vim ./AutoLogin.py
```

写入以下代码：

```python
import requests
import socket
import uuid
import subprocess
import platform


# Network connectivity check function
def check_network_connection(host="baidu.com", timeout=5):
    """
    Check if network connection is available by pinging the specified host
    Returns True if ping is successful, False otherwise
    """
    try:
        # Determine ping command based on operating system
        system = platform.system().lower()
        if system == "windows":
            # Windows ping command: ping -n count -w timeout host
            cmd = ["ping", "-n", "1", "-w", str(timeout * 1000), host]
        else:
            # Linux/Mac ping command: ping -c count -W timeout host
            cmd = ["ping", "-c", "1", "-W", str(timeout), host]

        # Execute ping command with no output to console
        with open('nul' if system == "windows" else '/dev/null', 'w') as devnull:
            result = subprocess.call(cmd, stdout=devnull, stderr=devnull)

        # Return True if ping was successful (return code 0)
        return result == 0
    except Exception as e:
        print("Network check failed: %s" % str(e))
        return False


# Portal authentication URL after redirection
url = "http://10.99.0.16/portalAuthAction.do"

wlanuserip = 'xxx.xxx.xxx.xxx'
# mac_address = ':'.join(hex(uuid.getnode())[2:].zfill(12)[i:i + 2] for i in range(0, 12, 2))
mac_address = 'xx:xx:xx:xx:xx:xx'
# Modify to your own username
userid = 'xxxxx'
# Modify to your own password
passwd = 'xxxxx'

data = {
    'wlanuserip': wlanuserip,
    'wlanacname': 'NFV-BASE',
    'chal_id': '',
    'chal_vector': '',
    'auth_type': 'PAP',
    'seq_id': '',
    'req_id': '',
    'ssid': '',
    'vlan': '8940199',
    'mac': mac_address,
    'message': '',
    'bank_acct': '',
    'isCookies': '',
    'version': '0',
    'authkey': 'amnoon',
    'url': '',
    'usertime': '0',
    'listpasscode': '0',
    'listgetpass': '0',
    'getpasstype': '1',
    'randstr': '6284',
    'domain': '',
    'isRadiusProxy': 'false',
    'usertype': '0',
    'isHaveNotice': '0',
    'times': '12',
    'weizhi': '0',
    'smsid': '1',
    'freeuser': '',
    'freepasswd': '',
    'listwxauth': '0',
    'templatetype': '2',
    'tname': '1',
    'logintype': '0',
    'act': '',
    'is189': 'false',
    'terminalType': '',
    'checkterminal': 'true',
    'portalpageid': '1',
    'listfreeauth': '0',
    'viewlogin': '1',
    'userid': userid,
    'wisprpasswd': '',
    'twocode': '0',
    'authGroupId': '',
    'alipayappid': '',
    'wlanstalocation': '',
    'wlanstamac': '',
    'wlanstaos': '',
    'wlanstahardtype': '',
    'smsoperatorsflat': '0',
    'reason': '',
    'res': '',
    'userurl': '',
    'challenge': '',
    'uamip': '',
    'uamport': '',
    'toqrcode': '',
    'isIOSPortal': 'false',
    'listwxsysauth': '0',
    'useridtemp': userid,
    'passwd': passwd,

}

header = {
    'Host': '10.99.0.16',
    'Origin': 'http://10.99.0.16',

    'User-Agent': 'Mozilla/5.0 (Linux; Android 10; SM-G981B) AppleWebKit/537.36 (KHTML, like Gecko) '
                  'Chrome/80.0.3987.162 Mobile Safari/537.36 Edg/114.0.0.0'
}

# Check network connectivity before attempting login
print("Checking network connectivity...")
if check_network_connection():
    print("Network connection is available. No need to perform portal login.")
else:
    print("Network connection is not available. Attempting portal login...")

    # Send POST request for network login
    response = requests.post(url, data, header)
    response.encoding = 'GBK'
    print("Login response:")
    print(response.text)

```

此代码通过ping检查网络连接状态，若ping超时则触发POST登录

此代码中需修改的有：

1. 宿舍网的账号和密码，即`userid`和`passwd`

2. `url`和`header`中的服务器地址，即在浏览器输入1.1.1.1重定向后的服务器地址

3. 已登录过宿舍校园网的终端`wlanuserip`和`mac_address`，以windows笔记本为例：

   打开浏览器F12开发人员选项--网络--保留日志

   手动登录校园网后，找到`portalAuthAction.do`，查看其载荷

   将载荷中的`wlanuserip`和`mac`复制填写到代码中

## 宿舍端下线

```python
import requests
import re
import socket
import uuid

# Portal authentication URL after redirection
url = "http://10.99.0.16/portalDisconnAction.do"

wlanuserip = 'xxx.xxx.xxx.xxx'
# mac_address = ':'.join(hex(uuid.getnode())[2:].zfill(12)[i:i + 2] for i in range(0, 12, 2))
mac_address = 'xx:xx:xx:xx:xx:xx'
# 修改为自己的wlan接入点IP，此IP为必填项
wlanacIp = '10.99.0.2'
# Modify to your own username
userid = 'xxxxx'
# Modify to your own password
passwd = 'xxxxx'

data = {
    'portaltype': '',
    'wlanuserip': wlanuserip,
    'wlanacname': 'NFV-BASE',
    'chal_id': '',
    'chal_vector': '',
    'auth_type': 'PAP',
    'wlanacIp': wlanacIp,
    'seq_id': '',
    'req_id': '',
    'ssid': '',
    'vlan': '8940199',
    'mac': mac_address,
    'bank_acct': '',
    'isCookies': '',
    'version': '0',
    'authkey': 'amnoon',
    'url': 'http://1.1.1.1',
    'usertime': '299997',
    'listpasscode': '0',
    'listgetpass': '0',
    'getpasstype': '1',
    'randstr': '7316',
    'domain': '',
    'isRadiusProxy': 'false',
    'usertype': '0',
    'isHaveNotice': '0',
    'times': '12',
    'weizhi': '0',
    'smsid': '1',
    'freeuser': '',
    'freepasswd': '',
    'listwxauth': '0',
    'templatetype': '2',
    'tname': '1',
    'logintype': '0',
    'act': 'DISCONN',
    'is189': 'false',
    'terminalType': '',
    'checkterminal': 'true',
    'portalpageid': '1',
    'listfreeauth': '0',
    'viewlogin': '1',
    'userid': userid,
    'wisprpasswd': passwd,
    'twocode': '0',
    'authGroupId': '',
    'alipayappid': '',
    'wlanstalocation': '',
    'wlanstamac': '',
    'wlanstaos': '',
    'wlanstahardtype': '',
    'smsoperatorsflat': '0',
    'reason': '',
    'res': '',
    'userurl': '',
    'challenge': '',
    'uamip': '',
    'uamport': '',
    'toqrcode': '',
    'isIOSPortal': 'false',
    'listwxsysauth': '0',
    'logout': '%C0%EB%CF%DF'

}
header = {
    'Host': '10.99.0.16',
    'Origin': 'http://10.99.0.16',

    'User-Agent': 'Mozilla/5.0 (Linux; Android 10; SM-G981B) AppleWebKit/537.36 (KHTML, like Gecko) '
                  'Chrome/80.0.3987.162 Mobile Safari/537.36 Edg/114.0.0.0'
}

response = requests.post(url, data, header)
response.encoding = 'GBK'
print(response.text)

```

1. 打开浏览器F12开发人员选项--网络--保留日志

2. 手动下线校园网后，找到`portalAuthAction.do`，查看其载荷
3. 将载荷中的`wlanuserip`、`mac`、`wlanacIp`复制填写到代码中

## 设置定时任务

编辑crontab计划任务

```bash
crontab -e
```

添加如下代码

```
*/2 * * * * /usr/bin/python2 /root/AutoLogin.py
```

每两分钟检测一次，断网自动重连

> [openwrt校园网自动登录且断网重连-CSDN博客](https://blog.csdn.net/qq248606117/article/details/125144699)
>
> [贵州大学校园网自动登录-CSDN博客](https://blog.csdn.net/2301_76970642/article/details/132338314)


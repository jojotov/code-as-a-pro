# HTTP 概述

## 组件 

HTTP 主要包含 User-agent, Proxies 和 Server 组件：

![http components](https://mdn.mozillademos.org/files/13679/Client-server-chain.png)

- 客户端 User-agent： 任意一个能够为用户发起行为的工具，通常为浏览器，或者是调试使用的某个程序。
- 服务端 Server
- 代理 Proxies：主要作用为缓存、过滤、负载均衡、认证和日志记录。



## 特点

- 可扩展
- 无状态、有会话（每一次请求相互独立，但可以通过 Cookies 共享某些上下文信息）
- 基于 TCP



# HTTP 请求

一个 HTTP 请求通常包含 **3** 个部分：

- 请求行 start-line
- 请求头 headers
- 请求体 body

## 请求行

```
POST / HTTP 1.1
GET /background.png HTTP/1.0
HEAD /test.html?query=alibaba HTTP/1.1
OPTIONS /anypage.html HTTP/1.0
```

请求行主要包含了请求方法（如 `GET` 或者 `PUT`）以及目标 URL



### 请求方法


| 方法    | 描述                                                         |
| ------- | ------------------------------------------------------------ |
| PUT     | 新增某个资源文件到指定 URL                                   |
| GET     | 获取某个指定 URL 的文件                                      |
| POST    | 传输某些信息给服务端，通常会导致服务端的某些状态发生改变或产生 side effect |
| DELETE  | 删除指定的 URL                                               |
| OPTIONS | 请求某些可用选项的信息                                       |
| TRACE   | 回溯请求消息                                                 |
| CONNECT | 协议预留                                                     |
| HEAD    | 请求某个资源的头部信息，与 GET 方法的响应相同，但没有 body   |
| PATCH   | 对资源部分修改                                               |



## 请求头

```
GET / HTTP/1.1
Host: developer.mozilla.org
Accept-Language: fr
```



请求头通常也分为三个部分：

- **General headers：**通常包含 `Connectio`、`Via` 等字段。
- **Request headers：**通常包含 `User-agent`、`Accept-Type` 等字段。
- **Entity headers：**通常包含 `Content-type` 等字段。





# HTTP 响应

一个 HTTP 响应通常包含：
-  状态行 status line
-  响应头 response header
-  响应体 response body


## 状态行

```
HTTP/1.1 404 Not Found
HTTP/1.1 200 OK
```

状态行一般包含以下信息：

- 协议版本，例如 `HTTP/1.1`
- 状态码，比如 `200`，`400` 等
- 状态文本，对状态码的文本描述

### 状态码

| 响应码 | 类型       | 示例                   |
| ------ | ---------- | ---------------------- |
| 1xx    | 信息       | 已接收到请求、正在处理 |
| 2xx    | 成功       |                        |
| 3xx    | 重定向     |                        |
| 4xx    | 客户端错误 | 常见 302               |
| 5xx    | 服务端错误 |                        |


### 2xx 状态码
#### 200 OK

### 3xx 状态码
#### 301 Moved Permanently

请求的对象已经永久性移走，新的地址会在 `Location:` 字段中。

#### 302 Found

客户端执行临时重定向。

#### 304 Not Modified

客户端仍具有以前的副本，且由请求头中的If-Modified-Since或If-None-Match参数指定的版本为最新版本，那么不需要重新传输资源。

#### 305 Use Proxy

客户端必须通过代理访问。

### 4xx 状态码
#### 400 Bad Request

客户端有明显错误，例如参数错误、格式错误等。

#### 403 Forbidden

服务器理解，但拒绝此请求。

#### 404 Not Found

请求失败，资源无法找到。

#### 408 Timeout

### 5xx 状态码
#### 500 Internal Serval Error

通用服务端错误。

#### 502 Bad Gateway

网关或代理服务器错误。

#### 503 Service Unvailable

服务器目前无法处理请求，一般是由于维护或过载造成。响应中可能会包含 Retry-After 标明重试时间，如果没有这个信息，客户端可当作 500 错误处理。

#### 504 Gateway Timeout

网关或代理服务器超时。



>  更多关于状态码信息[参考维基百科](https://zh.wikipedia.org/zh-hans/HTTP状态码)



## 响应头
```
Date: Sat, 09 Oct 2010 14:28:02 GMT
Server: Apache
Last-Modified: Tue, 01 Dec 2009 20:18:22 GMT
ETag: "51142bc1-7449-479b075b2891b"
Accept-Ranges: bytes
Content-Length: 29769
Content-Type: text/html
```

- General Headers，例如 `Date`，`Connection` 等。
- Response Headers，例如 `Vary` 等。
- Entity Headers，例如 `Content-Encoding`，`Content-Type` 等。

![Response header](https://mdn.mozillademos.org/files/13823/HTTP_Response_Headers2.png)



# HTTPS

HTTPS 为了解决安全问题，使用 SSL/TSL 加密数据包进行传输。

> TSL（Transport Security Layer）是 SSL 的改进版，公布于 RFC 4346


## SSL

SSL 基于对称加密与非对称加密结合，使用对称加密时的问题是，双方用于加密的密钥在传输过程中并非安全，因此 SSL 基于 TCP 之上通过非对称加密，解决了安全传输对称加密密钥。

建立一个 SSL 连接通常需要这些步骤：
1. 建立普通的 TCP 连接，即三次握手。
2. 客户端请求服务端证书，用于后面的数据传输加密。
3. 客户端验证证书后，通过证书加密 Master Serect（MS）发送给服务端。
4. 此时，双方都可以通过 MS 进行安全数据传输。


## CA 和证书

- CA，Catificate Authority，证书颁发机构。CA 的作用是签发证书。
- Certification，证书，一般会包含一个公钥、一个 CA 的签名和附加信息。

### 证书的申请流程

1. 生成私钥和 CSR 文件
2. 上传 CSR 文件给证书颁发机构
3. 证书颁发机构通过 CSR 文件生产证书（对证书内容进行哈希生成摘要、使用私钥加密生成数字签名）

### 证书的使用流程

1. 发送者通过私钥对数据进行加密
2. 接受者首先校验证书，通过哈希得到证书内容摘要，然后再通过 CA 的公钥解密 CA 加密过的摘要，最后进行比对。
2. 接收者通过证书的公钥解密数据



## HTTPS 证书
- 有效期
- RSA 公钥
- CA 以及 CA 签名
- 域名

## HTTPS 的问题

### 中间人攻击

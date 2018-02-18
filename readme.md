

# 浏览器工作原理


## 浏览器的组成
- 人机交互部分（UI）
- 网络请求部分（Socket）
- JavaScript引擎部分（解析执行JavaScript）
- 渲染引擎部分（渲染HTML、CSS等）
- 数据存储部分（cookie、HTML5中的本地存储LocalStorage、SessionStorage）

sqlite


## 主流渲染引擎

### 介绍
1. 渲染引擎 又叫 排版引擎 或 浏览器内核。

2. 主流的 渲染引擎 有
  - **Chrome浏览器**: Blink引擎（WebKit的一个分支）。
  - **Safari浏览器**: WebKit引擎，windows版本2008年3月18日推出正式版，但苹果已于2012年7月25日停止开发Windows版的Safari。
  - **FireFox浏览器**: Gecko引擎。
  - **Opera浏览器**: Blink引擎(早期版使用Presto引擎）。
  - **Internet Explorer浏览器**: Trident引擎。
  - **Microsoft Edge浏览器**: EdgeHTML引擎（Trident的一个分支）。


### 工作原理
1. 解析HTML构建Dom树（Document Object Model，文档对象模型），DOM 是W3C组织推荐的处理可扩展置标语言的标准编程接口。


2. 构建*渲染树*，*渲染树*并不等同于*Dom树*，因为像`head标签 或 display: none`这样的元素就没有必要放到*渲染树*中了，但是它们在*Dom树*中。

3. 对*渲染树*进行布局，定位坐标和大小、确定是否换行、确定position、overflow、z-index等等，这个过程叫`"layout" 或 "reflow"`。

4. 绘制*渲染树*，调用操作系统底层API进行绘图操作。



### 渲染引擎工作原理示意图

**渲染引擎工作原理示意图**

![渲染引擎工作原理](imgs/flow.png)


**WebKit工作原理（Chrome、Safari、Opera）**

![Blink渲染引擎工作原理](imgs/webkitflow.png)


**Gecko工作原理（FireFox）**

![Gecko渲染引擎工作原理](imgs/gecko.jpg)



### 浏览器的 reflow 或 layout 过程

https://www.youtube.com/watch?v=ZTnIxIA5KGw





### 打开 Chrome 的 Rendering 功能

第一步：

![第一步](imgs/chrome_rendering1.png)

第二步：

![第二步](imgs/chrome_rendering2.png)





## 浏览器访问网站过程

> 1. 在浏览器地址栏中输入网址。

![淘宝网址](imgs/taobao_url.png)

> 2. 浏览器通过用户在地址栏中输入的URL构建HTTP请求报文。

```http
GET / HTTP/1.1
Host: www.taobao.com
Connection: keep-alive
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/57.0.2987.133 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
Accept-Encoding: gzip, deflate, sdch, br
Accept-Language: zh-CN,zh;q=0.8,en;q=0.6
Cookie: l=Ag0NWp9E8X4hgaGEtIBhOmKxnSOH6kG8; isg=AkZGLTL-Yr9tHDZbgd5bsn4Rlzwg5IphaK-1BzBvMmlEM-ZNmDfacSyDfdgF; thw=cn
```

> 3. 浏览器发起DNS解析请求，将域名转换为IP地址。

![淘宝网址](imgs/taobao_ip.png)

> 4. 浏览器将请求报文发送给服务器。

> 5. 服务器接收请求报文，并解析。

> 6. 服务器处理用户请求，并将处理结果封装成HTTP响应报文。

```http
HTTP/1.1 200 OK
Server: Tengine
Date: Thu, 13 Apr 2017 02:24:25 GMT
Content-Type: text/html; charset=utf-8
Transfer-Encoding: chunked
Connection: keep-alive
Vary: Accept-Encoding
Vary: Ali-Detector-Type, X-CIP-PT
Cache-Control: max-age=0, s-maxage=300
Via: cache8.l2cm10-1[172,200-0,C], cache13.l2cm10-1[122,0], cache3.cn206[0,200-0,H], cache6.cn206[0,0]
Age: 293
X-Cache: HIT TCP_MEM_HIT dirn:-2:-2
X-Swift-SaveTime: Thu, 13 Apr 2017 02:19:32 GMT
X-Swift-CacheTime: 300
Timing-Allow-Origin: *
EagleId: 9903e7e514920502659594264e
Strict-Transport-Security: max-age=31536000
Content-Encoding: gzip

<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="utf-8" />
<meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />
<meta name="renderer" content="webkit" />
<title>淘宝网 - 淘！我喜欢</title>
<meta name="spm-id" content="a21bo" />
<meta name="description" content="淘宝网 - 亚洲较大的网上交易平台，提供各类服饰、美容、家居、数码、话费/点卡充值… 数亿优质商品，同时提供担保交易(先收货后付款)等安全交易保障服务，并由商家提供退货承诺、破损补寄等消费者保障服务，让你安心享受网上购物乐趣！" />
<meta name="aplus-xplug" content="NONE">
<meta name="keyword" content="淘宝,掏宝,网上购物,C2C,在线交易,交易市场,网上交易,交易市场,网上买,网上卖,购物网站,团购,网上贸易,安全购物,电子商务,放心买,供应,买卖信息,网店,一口价,拍卖,网上开店,网络购物,打折,免费开店,网购,频道,店铺" />
</head>
<body>
......
</body>
</html>
```

> 7. 服务器将HTTP响应报文发送给浏览器。

> 8. 浏览器接收服务器响应的HTTP报文，并解析。

> 9. 浏览器解析 HTML 页面并展示，在解析HTML页面时遇到新的资源需要再次发起请求。

> 10. 最终浏览器展示出了页面






## HTTP请求报文和响应报文格式

![http请求报文和响应报文](imgs/HTTPMsgStructure2.png)




## DNS 解析过程

![DNS解析过程](imgs/DNS.gif)


### windows 下 hosts 文件位置

C:\Windows\System32\drivers\etc\hosts





## DOM 解析

参考代码:

```html
<html>
  <body>
    <p>Hello World</p>
    <div> <img src="example.png" alt="example"/></div>
  </body>
</html>
```

![Dom 解析工作原理](imgs/dom.png)



## Webkit CSS 解析

![CSS 解析工作原理](imgs/css_parser.png)



## How Browsers work - 浏览器是如何工作的

[How Browsers work](http://taligarsiel.com/Projects/howbrowserswork1.htm#The_browsers_we_will_talk_about)
https://www.html5rocks.com/zh/tutorials/internals/howbrowserswork/





# 删除节点

```javascript
var iframe = document.getElementsByTagName('iframe');
var child  = iframe[0];
child.parentNode.removeChild(child);
```

# js 常用常查 API

## url 编/解码函数

encodeURIComponent() 函数可把字符串作为 URI 组件进行编码

### 语法

`encodeURIComponent(URIstring)`

参数  |  描述
----- | ----
URIstring |  必需. 一个字符串, 含有 URI 组件或其他要编码的文本.

***提示：***请注意 encodeURIComponent() 函数 与 encodeURI() 函数的区别之处，前者假定它的参数是 URI 的一部分（比如协议、主机名、路径或查询字符串）。因此 encodeURIComponent() 函数将转义用于分隔 URI 各个部分的标点符号。

decodeURIComponent() 函数可对 encodeURIComponent() 函数编码的 URI 进行解码。

## 语法

`decodeURIComponent(URIstring)`

参数  |  描述
----- | ----
URIstring  | 必需. 一个字符串, 含有编码 URI 组件或其他要解码的文本.

## 字符串转整形

parseInt() 函数可解析一个字符串, 并返回一个整数.

### 语法

`parseInt(string, radix)`

参数  |  描述
----- | ----
string | 必需。要被解析的字符串。
radix  | 可选。表示要解析的数字的基数。该值介于 2 ~ 36 之间。<br />如果省略该参数或其值为 0，则数字将以 10 为基础来解析。<br />如果它以 “0x” 或 “0X” 开头，将以 16 为基数。<br />如果该参数小于 2 或者大于 36，则 parseInt() 将返回 NaN。

***提示：***如果字符串的第一个字符不能被转换为数字，那么 parseFloat() 会返回 NaN。

## 字符串转浮点数

parseFloat() 函数可解析一个字符串，并返回一个浮点数。

### 语法

`parseFloat(string)`

参数  |  描述
----- | ----
string | 必需。要被解析的字符串。

***提示：***可以通过调用 isNaN 函数来判断 parseFloat 的返回结果是否是 NaN。如果让 NaN 作为了任意数学运算的操作数，则运算结果必定也是 NaN。

# 微信开发相关

以下的方法可以简化用户在微信中下载 app 的操作

## 微信浏览器跳转到 app store

*微信在其内置浏览器对 url 做了限制,发现用户要跳转至 app store 下载 app 时,会将其屏蔽*

以下方法止前可以绕过微信的限制.  时间: 2015.01.19  [参考](http://www.ildsea.com/1781.html)

```html
<a href="http://mp.weixin.qq.com/mp/redirect?url=<?php echo urlencode('app store 中待推广下载链接'); ?>">下载并安装app</a>

@date 2015.03.30 同事反应iOS不能跳转到 app store 里面,亲测,果然微信改策略了.
```

另外一种方法目前看还有效,还同时兼容 Android 和 iOS,但必须在腾讯应用包中有记录.这种重要的渠道会有多少 app 不支推包呢?

```html
<a href="http://a.app.qq.com/o/simple.jsp?pkgname=<?php echo '待推广 app 名字'; ?>">下载并安装app</a>

@date 2015.03.30 亲测管用.
```

## 安卓在微信中下载 apk

将 url 指定为:
`http://app.qq.com/#id=detail&appid=应用id`

## js 实现从后端取微信 js-sdk 配置

以下是同步方式加载.除非真的有必要,否则建议同步加载.

```html
<script src="http://res.wx.qq.com/open/js/jweixin-1.0.0.js"></script>
<script>
var _u = window.location.href;
var _pos = _u.indexOf('#');
if (-1 == _pos)
{
    _pos = _u.length;
}
var _c_url = _u.substr(0, _pos);
var _s_url = '/wx/wxconfig?debug=0&url=' + encodeURIComponent(_c_url)  + '&p=' + (+(new Date()));
document.write('<script src="' + _s_url + '"><\/script>');
</script>
```

在开发环境中调试时,将 debug=1 传入即可,会将执行流程完整输出.

*如果确认接口不需要缓存,为了防止以缓存为借口扯皮,切记加上噪音串,代码为证.*

## 判断网页是否是在微信内嵌浏览器中打开

真实的一条 HTTP_USER_AGENT
`Mozilla/5.0 (iPhone; CPU iPhone OS 8_1_3 like Mac OS X) AppleWebKit/600.1.4 (KHTML, like Gecko) Mobile/12B466 MicroMessenger/6.1 NetType/WIFI`

```js
// 判断用户是不是用微信打开了浏览器
// @see: https://gist.github.com/anhulife/8470534
function isWeixinBrowser(){
    // 截至2014年2月12日,这个方法不能测试 windows phone 中的微信浏览器
    return (/MicroMessenger/i).test(window.navigator.userAgent);
}
```


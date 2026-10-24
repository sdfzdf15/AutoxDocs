# android\.webkit\.CookieManager API 文档（AutoX\.js / Rhino）

> 包路径：`android.webkit.CookieManager`
> 
> 作用：**管理 Android WebView 的 Cookie 存储**，WebView 内置 Cookie 管理器，全局单例。
> 
> Cookie = 网站存在你手机/电脑本地的「小型备忘录、登录凭证、身份卡片」
> 网站为了记住你，会在你设备上存一小段文本数据（Key=Value 格式），这就是 Cookie。
> 
> ⚠️ 隔离说明：仅对 `android.webkit.WebView` 生效；和 OkHttp、HttpClient、手机浏览器APP Cookie 互不通用。
> 
> 只要 WebView 收到服务器的 Set-Cookie 响应头，CookieManager 就无条件自动保存，跟你点没点"自动登录"一毛钱关系都没有。
>
```JavaScript
// 第一次：不勾自动登录，登录完立刻读 cookie
log(webView.getUrl()); // ✅ 有 cookie

// 退出 Auto.js 进程（不是只关界面，要 force stop）
// 第二次：重新打开脚本，loadUrl(论坛)
log(webView.getUrl());  // ❌ 空了（会话 cookie 随进程没了）

// ──────────────────────────────────────

// 第三次：勾上自动登录，登录完 force stop 进程
// 第四次：重新打开脚本，loadUrl(论坛)
log(webView.getUrl());  // ✅ 还有 cookie（长期 cookie 持久化了）
```
## 获取实例

静态方法 `getInstance()`：获取全局唯一 CookieManager 单例，**不能 new 创建**。

```JavaScript
var cm = android.webkit.CookieManager.getInstance();
```

---

## 全部公开方法

### 1\. setAcceptCookie\(boolean accept\)

**用途**：设置 WebView 是否允许保存、读取 Cookie。

- 参数

    - `true`：允许读写Cookie

    - `false`：拒绝Cookie，getCookie 会返回空

> 注意：部分系统默认关闭，**不开启则Cookie读写全部失效**。
> 
> 

```JavaScript
cm.setAcceptCookie(true); // 开启Cookie支持，几乎必写
```

### 2\. acceptCookie\(\)

**用途**：查询当前是否开启Cookie接收。

返回布尔值。

```JavaScript
var isAccept = cm.acceptCookie();
console.log(isAccept); // true/false
```

### 3\. setCookie\(String url, String value\)

**用途**：给指定域名注入Cookie。

- 参数

    1. `url`：目标站点URL（决定域名、安全策略，必须是站点完整url，不能只写域名）

    2. `value`：Cookie字符串，格式 `key=val; [domain=xxx]; [path=/]; [Secure]; [SameSite=Lax]`

> ⚠️ 重点坑：
> 
> 1. 不会立刻生效，最好配合 `flush()`；
> 
> 2. 高版本WebView，`Secure` 标记Cookie只能在 `https` url下设置成功；
> 
> 3. domain 写法示例 `domain=.example.com` 代表子域名共享；
> 
> 4. 不能一次性设置多条cookie，多次调用setCookie。
> 
> 

```JavaScript
cm.setCookie("https://example.com", "token=123456; path=/");
```

### 4\. getCookie\(String url\)

**用途**：读取某个url下全部Cookie，返回拼接字符串。

- 参数：url

- 返回：字符串，多条cookie用 `; ` 分隔；无Cookie返回 `null`。

示例返回：`token=123; uid=999`

```JavaScript
var cookieStr = cm.getCookie("https://example.com");
```

### 5\. flush\(\)

**用途**：强制把内存中的Cookie写入持久化存储（磁盘）。

> Android 4\.4\+ 官方说明：WebView会自动异步持久化；但AutoX场景，注入Cookie后调用flush，可降低Cookie不生效概率。
> 
> 

```JavaScript
cm.flush();
```

### 6\. removeAllCookies\(ValueCallback\<Boolean\> callback\)

- 参数：回调接口，可以传`null`；回调参数boolean代表是否删除成功。

**用途**：删除WebView全部Cookie。

```JavaScript
cm.removeAllCookies(null);
```

### 7\. removeSessionCookies\(ValueCallback\<Boolean\> callback\)

**用途**：删除**会话Cookie**（没有expires过期时间，关闭页面就失效的临时Cookie），持久Cookie保留。

```JavaScript
cm.removeSessionCookies(null);
```

### 8\. hasCookies\(\)

**用途**：判断当前是否存在任何Cookie。

返回布尔值。

```JavaScript
var exist = cm.hasCookies();
```

### 9\. setAcceptThirdPartyCookies\(WebView webview, boolean accept\)

**用途**：是否允许**第三方Cookie**（页面内iframe、跨域资源的Cookie）。Android \>= 5\.0。

- 参数

    1. webview：目标WebView实例

    2. true：允许第三方Cookie（很多登录页面iframe需要开启）

> 默认关闭；很多网站登录失败就是因为没开第三方Cookie。
> 
> 

```JavaScript
cm.setAcceptThirdPartyCookies(wv, true);
```

### 10\. acceptThirdPartyCookies\(WebView webview\)

**用途**：查询WebView是否开启第三方Cookie。

```JavaScript
var enable = cm.acceptThirdPartyCookies(wv);
```

### 11\. removeExpiredCookies\(\)

**用途**：清理过期Cookie。WebView内部会自动执行，一般不用手动调用。

```JavaScript
cm.removeExpiredCookies();
```

---

## Cookie 属性说明（setCookie value 参数内可用）

|属性|说明|
|---|---|
|`domain=xxx.com`|生效域名，`.xxx.com` 包含所有子域名|
|`path=/`|网站路径，`/` 全站生效|
|`expires=时间戳`|UTC时间，持久Cookie；不带则为会话Cookie|
|`Secure`|仅HTTPS请求携带Cookie|
|`HttpOnly`|JS无法读取该Cookie，WebView内部请求自动带上|
|`SameSite=Lax/Strict/None`|跨站策略；`None` 必须搭配Secure|

---

## 完整基础示例（AutoX）

```JavaScript
var cm = android.webkit.CookieManager.getInstance();
cm.setAcceptCookie(true);
// 开启第三方Cookie（可选，iframe登录用）
// cm.setAcceptThirdPartyCookies(wv, true);
// 设置Cookie
cm.setCookie("https://example.com", "uid=1001; path=/; domain=.example.com");
cm.flush();
// 读取Cookie
var cookies = cm.getCookie("https://example.com");
console.log("Cookies:", cookies);
// 删除全部Cookie
// cm.removeAllCookies(null);

```



```JavaScript
ui.layout(
    <vertical>
        <webview id="wv" />
    </vertical>
);
ui.wv.loadUrl("https://example.com");
var cm = android.webkit.CookieManager.getInstance();
cm.setAcceptCookie(true); // 开启Cookie支持，几乎必写
//登录后，读取Cookie，判断是否成功
var cookies = cm.getCookie("https://example.com");
console.log("Cookies:", cookies);
ui.wv.loadUrl("https://www.baidu.com");
// 读取Cookie，判断是否成功
var cookies = cm.getCookie("https://www.baidu.com");
console.log("Cookies:", cookies);
//两个域名各自维护独立的 Cookie 集合，互不相关，结果必然不同。
```

## 常见问题总结

1. getCookie返回null

    - 没有开启 `setAcceptCookie(true)`

    - url写错、域名不匹配、Secure标记用http地址

    - WebView从未加载过该域名，WebView内核未初始化域名上下文

2. setCookie设置成功，但网页拿不到Cookie

    - 缺少 `path=/`

    - 跨域/第三方Cookie未开启 `setAcceptThirdPartyCookies`

    - SameSite策略限制

3. CookieManager只作用于系统WebView内核，和其他网络库隔离。

---

## 扩展可选函数（Cookie字符串 ↔ JSON对象转换）

```JavaScript
/**
 * cookie字符串转JSON对象
 * @param {string} cookieStr getCookie返回的字符串
 * @returns {Object}
 */
function cookieToJson(cookieStr) {
    if (!cookieStr) return {};
    const obj = {};
    cookieStr.split("; ").forEach(item => {
        const arr = item.split("=");
        const key = arr.shift().trim();
        const val = arr.join("=");
        obj[key] = val;
    });
    return obj;
}
/**
 * JSON对象转cookie字符串（用于setCookie）
 * @param {Object} cookieObj
 * @returns {string}
 */
function jsonToCookie(cookieObj) {
    const arr = [];
    for (let k in cookieObj) {
        arr.push(`${k}=${cookieObj[k]}`);
    }
    return arr.join("; ");
}
```

你可以直接全选复制保存为 `CookieManager.md`。

需要我再增加**异步等待Cookie写入完成**的封装到这份md里吗？


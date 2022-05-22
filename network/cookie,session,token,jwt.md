# cookie、session、token、jwt 辨析

## cookie

* HTTP 响应头 Set-Cookie 设置。
* 浏览器请求时自动把 cookie 通过 HTTP 请求头的 Cookie 带上。

### 配置

* Domain 属性指定哪些域名要带 Cookie。没指定默认设为当前 URL 的一级域名。如果 Set-Cookie 设置的域名不属于当前域名，浏览器会拒绝这个 Cookie。
* Path 属性指定浏览器请求哪些路径会附带这个 Cookie。
* Expires 执行到期时间。 UTC 格式。没指定只在当前会话有效，关闭窗口 Cookie 失效。
* Max-Age: 指定 Cookie 从现在开始存在的秒。 Max-Age 优先级高于 Expires.
* Secure 只有 HTTPS 才发 Cookie。HTTP 指定无效。
* HttpOnly 指定 Cookie 无法通过 JS 脚本拿到。

```javascript
// 一个 Set-Cookie 头只能写一条 cookie

Set-Cookie: username=tim; domain=tim.com; path=/blog; Expires=Wed, 21 Oct 2015 07:28:00 GMT; Secure; HttpOnly
Set-Cookie: username=jimu; domain=tim.com
Set-Cookie: height=180; domain=me.tim.com
Set-Cookie: weight=80; domain=me.tim.com
```

* document.cookie: JS 操作 cookie。
* cookie 存储方案。

## 应用方案： 服务端 session

![img](cookie,session,token,jwt.assets/37adb2019d064967923a659848870771tplv-k3u1fbpfcp-watermark.awebp)

* 剪头发办会员卡，报手机号。通过手机号查询信息。
* session 最通常的实现方式是 cookie。

### Session 的存储方式

* 服务端只是给 cookie 一个 sessionId。需要存具体内容。
* Redis。
* 内存，重启没了。
* 数据库。性能不好。

### Session 过期和销毁

* 存储的销毁即可。

### Session 分布式问题

* session 集中存储。用独立的 Redis或数据库，把 session 存到一个库里。
* 相同 IP 请求在负载均衡时打到一台机器上。 如 nginx 配置 ip_hash。
* 通常用第一种，第二种不是真负载均衡，而且没有解决宕机问题。

## 应用方案：token

* session 不好维护。单独启用一套 Redis 集群。

* 身份证有姓名有住址，不需要查询。

![img](cookie,session,token,jwt.assets/a1c57a08eb204f528256f3980c721148tplv-k3u1fbpfcp-watermark.awebp)

### 客户端 token 的存储方式

* token 无状态性，有效期，使用限制都在其本身内容，对 cookie 依赖小。

### token 的过期

* 过期时间也放进 token 内容中。

### token 的实现方式

#### base64

* 使用签名防篡改。

#### JWT

* 不增加 cookie 数量。数据有规范格式。
* JSON Web Token 定义了一种传递 JSON 信息的方式。这些信息通过数字签名确保可信。

![img](cookie,session,token,jwt.assets/65b5e67305f84e9391de2d5b436600e7tplv-k3u1fbpfcp-watermark.awebp)

### Refresh Token

* 用于鉴权的 Token，称为 access token。敏感业务需要有效期足够短。用来专门生成 access token 的 token，称为 refresh token。
* access token 有效期设置短一些，refresh token 有效期设置长一些。refresh token 也可能是如 session 一样处理，通过独立服务增加安全性。

![img](cookie,session,token,jwt.assets/b764b256211b4ea182388fd92674fe70tplv-k3u1fbpfcp-watermark.awebp)





#### 信息存在 cookie 的问题

* 只有浏览器端有 cookie。

* 要防止 CSRF 攻击。

* 存在别的位置手动加上，防止 CSRF，不局限于浏览器。

#### 服务端存数据/不存数据

* 存数据： 请求只带 id，缩短认证字符串长度，减小请求体积。
* 不存数据： 不需要服务端整套解决方案和分布式处理，避免查库带来的验证延迟。



### 单点登录

* 业务多，在不同域名下。一次登录，全部通用。

#### 主域名相同

* 设置 cookie.domain 即可。

#### 主域名不同

* 独立的认证服务，称为 SSO。

![img](cookie,session,token,jwt.assets/bf2e8ba61fc94b52be164c207b9d8358tplv-k3u1fbpfcp-watermark.awebp)
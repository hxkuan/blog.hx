---
title: JWT笔记
categories: 笔记
tags:
  - other
  - 编程
  - 基础
date: 2018-07-30 17:33:37
---
首先感谢阮一峰老师，一直受益于他的分享，这次也是一样，`服务器就不保存任何 session数据了，也就是说，服务器变成无状态`，多亏于这句话，让我对JWT豁然开朗。由于惯性思维，一想到登录有关的就会想到session，所以听到token的第一反应就是服务扩充session功能，大多数的博客也没说明或者重点注明这点，以至于之前根本没注意到这一点，这也是我对之后原理一值都觉得很奇怪以至于难以理解。
<!-- more-->

## JWT
JSON Web Token（简称：JWT）由三段信息用.构成的字符串；分别为头部(header)，载荷(payload)，签证(signature)；前两段字符由JSON对象通过Base64URL算法转化而成，最后一段还需要加密处理。其实JWT的原理很简单，就是通过无法伪造的Signature部分来防止Payload和Header部分数据被篡改。

### Header
header部分用于描述该数据；JWT常用两个字段—— 
1. 属性表示签名的算法（algorithm），默认是 HMAC SHA256（写成 HS256）；
2. typ属性表示这个令牌（token）的类型（type），JWT 令牌统一写为JWT

### Payload
Payload部分用来存放实际需要传递的数据;JWT 规定了7个官方字段，供选用:
1. exp (expiration time)：过期时间
2. sub (subject)：主题
3. aud (audience)：受众
4. nbf (Not Before)：生效时间
5. iat (Issued At)：签发时间
6. jti (JWT ID)：编号

### Signature
Signature部分是对前两部分的签名，防止数据被篡改。
签名公式：
```
HMACSHA256(
  base64UrlEncode(header) + "." + base64UrlEncode(payload),
  secret)
```
#### secret的设计
> secret 应该设计成和用户相关的属性，而不是一个所有用户公用的统一值。这样可以有效的避免一些注销和修改密码时遇到的窘境。

### JWT使用方式
这个，我终结来一下，由于浏览器端存储方式的限制，发现本质上其实就3种：
1. 本地存储在localStorage，请求时放入HTTP Header中的Authorization字段里面。
> 缺点： 都可以被 javascript 访问，所以容易受到XSS攻击

2. 本地存储在Cookie中，请求时通过Cookie自动携带
> 缺点：无法跨域(cookie 无法被自动携带至其他域名)，因为Cookie的机制，容易遭受CSRF攻击。

3. 本地存储在Cookie中，请求时放入HTTP Header中的Authorization字段里面。
> 缺点：无法跨域

注意：localStorage/SessionStorage/dbXX(貌似有浏览器支持db了)在这里效果上其实是一样的。

#### 应用场景举例
1. 一次性验证，比如用户注册后需要发一封邮件让其激活账户。
2. restful api 的无状态认证

## JWT特点
- （1）JWT 默认是不加密，但也是可以加密的。生成原始 Token 以后，可以用密钥再加密一次。

- （2）JWT 不加密的情况下，不能将秘密数据写入 JWT。

- （3）JWT 不仅可以用于认证，也可以用于交换信息。有效使用 JWT，可以降低服务器查询数据库的次数。

- （4）JWT 的最大缺点是，由于服务器不保存 session 状态，因此无法在使用过程中废止某个 token，或者更改 token 的权限。也就是说，一旦 JWT 签发了，在到期之前就会始终有效，除非服务器部署额外的逻辑。

- （5）JWT 本身包含了认证信息，一旦泄露，任何人都可以获得该令牌的所有权限。为了减少盗用，JWT 的有效期应该设置得比较短。对于一些比较重要的权限，使用时应该再次对用户进行认证。

- （6）为了减少盗用，JWT 不应该使用 HTTP 协议明码传输，要使用 HTTPS 协议传输。

## Other
### 题外话

- JWT如果只是为了替换Session，除非是单纯的为了技术统一，否则还不如不用。
- JWT的最大特点就是无状态化。适合于登录状态时间较长的应用中；对于JWT的续签，我不太想使用Refresh Token这一套(那样还不如使用Oauth2)，我倾向于使用“快到期了”就刷新，但这时间比较长，相当于整体时间的1/2-1/3，但也不会大于某个值。

### 参考

- https://cloud.tencent.com/developer/article/1110587

### 补缺

- 服务器集群之间是不共享session的，所以哪怕获取到sessionId也没法获取session；如此只能对立session数据，但如此又会加大工作量，并且若是session层挂了，所有的集群都无法正常使用。(之前莫名其妙的觉得只要获得到sessionId就能获取到session，也就觉得cookie存在根域下就能解决跨域问题，无语...)
- JWT的作用不是保密数据，不是保证登录状态不泄露，而是保证数据不被更改。(至于本地数据的安全，最终还是需要使用者来保证)
- 签名(Signature)算法共同的特点是整个过程是不可逆的；加密(Encryption)是可以解密的。
- jwt token泄露了怎么办？凉拌！传统方式sessionId泄露了也一样，这就像是你掉了钥匙，捡到的人还知道怎么用这钥匙，那么你除了换门锁还有什么办法呢？所以我们要考虑的是怎么防止泄露。
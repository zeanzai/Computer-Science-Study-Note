[TOC]
参考：
http://www.jdon.com/artichect/json-web-tokens.html
http://www.jianshu.com/p/576dbf44b2ae


# 1. 简单概述
现在API越来越流行，如何安全保护这些API? JSON Web Tokens(JWT)能提供基于JSON格式的安全认证。它有以下特点：

 - JWT是跨不同语言的，JWT可以在 .NET, Python, Node.js, Java, PHP, Ruby, Go, JavaScript和Haskell中使用
 - JWT是自我包涵的，它们包含了必要的所有信息，这就意味着JWT能够传递关于它自己的基本信息，比如用户信息和签名等。
 - JWT传递是容易的，因为JWT是自我包涵，它们能被完美用在HTTP头部中，当需要授权API时，你只要通过URL一起传送它既可。

JWT易于辨识，是三段由小数点组成的字符串：
```
aaaaaaaaaa.bbbbbbbbbbb.cccccccccccc
```
这三部分含义分别是header，payload, signature

-----------------------------------------------
## 1.1 Header
头部包含了两个方面：类型和使用的哈希算法（如HMAC SHA256）：
```
{
"typ": "JWT",
"alg": "HS256" 
}
```
对这个JSON字符进行base64encode编码，我们就有了首个JWT:
```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
```
## 1.2 Payload


JWT的第二部分是payload，也称为 JWT Claims，这里放置的是我们需要传输的信息，有多个项目如注册的claim名称，公共claim名称和私有claim名称。
注册claim名称有下面几个部分：

 - iss: token的发行者 
 - sub: token的题目 
 - aud: token的客户 
 - exp: 经常使用的，以数字时间定义失效期，也就是当前时间以后的某个时间本token失效。 
 - nbf: 定义在此时间之前，JWT不会接受处理。
 - iat: JWT发布时间，能用于决定JWT年龄 
 - jti: JWT唯一标识.能用于防止JWT重复使用，一次只用一个token

公共claim名称用于定义我们自己创造的信息，比如用户信息和其他重要信息。
私有claim名称用于发布者和消费者都同意以私有的方式使用claim名称。<br />
下面是JWT的一个案例：
```
{
    "iss": "scotch.io",
    "exp": 1300819380,
    "name": "Chris Sevilleja",
    "admin": true 
}
```

## 1.3 签名
JWT第三部分最后是签名，签名由以下组件组成：

 - header
 - payload
 - 密钥

下面是我们如何得到JWT的第三部分：
``` javascript
var encodedString = base64UrlEncode(header) + "." + base64UrlEncode(payload); HMACSHA256(encodedString, 'secret');
```
这里的secret是被服务器签名，我们服务器能够验证存在的token并签名新的token。

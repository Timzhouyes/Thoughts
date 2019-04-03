---
layout:     post   				    # 使用的布局（不需要改）
title:      JWT相关				# 标题 
subtitle:   Studying JWT #副标题
date:       2019-04-03				# 时间
author:     Haiming 						# 作者
header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - 编程
    - JWT
    - 记录
    - 学习
---
After read a [toturial](http://www.ruanyifeng.com/blog/2018/07/json_web_token-tutorial.html) of Mr.Ruan, just make some notes to memory.

# 1. What is JWT ?
JWT is a way for users to save information in client side, so that in server side, we don't need to save sessions. 
# 2. The theory of JWT
After authentication of users, server sends a JSON to client, and after authentication, when users contact with server, it brings the JSON in the head of website. Preventing it from changed by users, when server generates the JWT, it will bring a *digital sign* with it.

And server can be stateless.

# 3. Datastructure of JWT
JWT contains three parts
1. Header
2. Payload
3. Signature

## 3.1 Header
In header ,it contains:
1. ALG: the algorithm of signature. Default is HS256,which means **HMAC SHA256**. 
2. TYP:type of token, in JWT, is **JWT**.

## 3.2 Payload
In payload, it contains:
1. iss (issuer)：签发人
2. exp (expiration time)：过期时间
3. sub (subject)：主题
4. aud (audience)：受众
5. nbf (Not Before)：生效时间
6. iat (Issued At)：签发时间
7. jti (JWT ID)：编号
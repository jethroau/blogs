Authentication指的是确定这个用户的身份，Authorization是确定该用户拥有什么操作权限。

认证方式一般有三种

## Basic Authentication

这种方式是直接将用户名和密码放到Header中，使用Authorization: Basic Zm9vOmJhcg==，使用最简单但是最不安全。 

## TOKEN认证

这种方式也是再HTTP头中，使用Authorization: Bearer <token>，使用最广泛的TOKEN是JWT，通过签名过的TOKEN。 

## OAuth2.0

这种方式安全等级最高，但是也是最复杂的。如果不是大型API平台或者需要给第三方APP使用的，没必要整这么复杂。 

一般项目中的RESTful API使用JWT来做认证就足够了。 

## 

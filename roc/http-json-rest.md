## HTTP协议，REST和JSON ##
**陈鹏** 2014年11月10日 19:59:35

### 问题： ###
假如两台服务器之间通信，需要安全性较高。从服务器A向服务器B发起请求，为游戏充值。请问该如何做？

示例URL：[http://domain:port/wendao/recharge](http://domain:port/wendao/recharge)
* * *
首先，解决验证问题：
- 	里边含有被充值的帐号，充值金额，订单号（根据用户名，时间，随机数等生成。32位的长度可以保证安全）。

**总结：有的是加密，有的是摘要，不可混淆！**

**基本思路：**

> 定字符串+5位随机数 MD5摘要 
cshijiel56459 
b1dac4f0952c5b1f22bbcda5338d5b69 

> 被充值的账号+:+充值金额 BASE64加密 
mercy9222:2 
bWVyY3k5MjIyOjIK
> 
> 订单号:（用户名+时间戳）MD5后ASCII取27位+5位随机数 
mercy9222141560096056549

___
PSA:OAuth2.0时序图
![OAuth2.0][1]
[1]: http://cshijiel.github.io/roc/images/OAuth2.0.jpg
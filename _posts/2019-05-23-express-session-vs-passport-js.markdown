---
layout: post
title: express-session和passportjs的工作原理
date: 2019-05-23
---

*express-session和passportjs这两个npm模块在很多项目中都会用到，主要用来做用户的登录状态的校验。这两个模块通常需要配合来完成一个完整的用户登录状态检查的功能，那么在这个过程中他们分别都做了哪些事情呢？我们今天就来聊一聊这个问题。

## session
首先，我们解释一下session的概念。我们知道，HTTP协议本身是无状态的。这里的所谓状态，指的就是服务端可以区分出本次的请求是属于哪个用户。
比如我们在淘宝买东西，那么淘宝的服务端是必须要知道是哪个用户在下订单。要给无状态的HTTP请求加上状态，一般是使用cookie。大概流程是：
   1. 服务端在收到前端发来的HTTP请求的时候，在Response Header中添加Set-Cookie的Header。cookie中的内容服务端通常会存储起来，以便在后续的请求中再去获取对应的内容。
   2. 浏览器在后续的每次请求中，都会携带这个cookie(放在Cookie Header中)，把它发给服务端。
   3. 服务端根据前端发来的Cookie，在自己的存储中找到对应的信息(如userId等)，就可以知道本次请求来自于哪个用户。

## session 和 Cookie的区别
session是个虚拟的概念，而并不是一项具体的技术。而Cookie是一项具体的技术，它是有明确的rfc文档描述的。
在实践中，我们一般使用cookie这项技术来实现session的功能(也就是使服务端区分不同请求对应到不同的用户)。

## express-session
`express-session`这个库做的事情就是上边描述的这些，它主要负责解析前端发来的请求中所携带的cookie，以及在服务端的响应中写入Cookie。但它不负责Session状态的存储。
Session状态存储的工作它是移交给像`express-mysql-session`这样的库来去做的。这样做的目的是灵活，在实践中，我们可能把session信息存储到像mysql/postgresql
这样的持久化的存储系统中，也有可能存储到memorycache/redis这样的内存存储系统中以获得更快的存取速度。在开发过程中，我们可能想直接存储到内存中，这样可以
更容易的构建开发环境，而不需要安装额外的依赖。`express-session`把存储移交给别的库处理，并可以在代码中指定使用什么样的存储，这给了我们极大的灵活性。

另外还有一点，`express-session`也是移交给别的库去处理的，就是cookie(或者说session)和用户系统的对应关系。也就是说，`express-session`只负责解析cookie，
并存储session信息到数据库(依赖`express-mysql-session`这样的库)，但它不负责解释一个cookie对应到的用户到底是哪一位。这部分的功能我们需要引入另一个库:
`passport.js`

## passport.js
根据官网的介绍，passportjs是一个`authentication middleware for Node.js`，它是专门用来做`authentication`的，也就是用户身份的鉴定。
passportjs支持很多种用户身份验证方式，比如最基本的username+passport的方式，以及很多第三方登录方式，比如github登录、facebook登录等。
国内用户最常用的微信登录等也可以非常方便的实现,在passportjs中这些不同的验证方式就叫做`strategy`。那么passportjs与express-session的关系是什么呢？

## passport.js 与 express-session
简单的说，`express-session`会注入req.session这个变量，然后passportjs在代码中会用到这个变量，并往这个变量中写入数据。这也是为什么express-session必须在
passportjs之前应用到express的middleware中的原因。

## 总结
我个人对这几个模块之间的关系一直比较模糊，想想原因可能是它们之间其实是互相依赖的关系，而在文档中表述的并不是很清楚。如果不去看它们的代码，真的很难
理清楚这几个模块之间的依赖关系。这篇文章只是一个大概的描述，至于具体细节还是需要多看代码。

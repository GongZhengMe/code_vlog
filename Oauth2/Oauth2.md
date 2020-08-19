#  Why Oauth2.0?

## Guide

本文仅作为Oauth2使用参考，如果本身已经接触过Oauth2.0,希望本篇文档能帮你打开一些思路，能够让你清晰的选择是否要使用Oauth2.0

如果没有了解过Oauth2.0，请跳转[阮一峰Oauth2.0讲解](http://www.ruanyifeng.com/blog/2019/04/oauth-grant-types.html)

## Base Q&A

### Question

如果你最近正在参与关于是否要使用Oauth2.0的会议，你是否会在会议中不断的回到三个很基础的问题:

>- 什么是Oauth2.0？
>- Oauth2.0存在的意义是什么？
>- 为什么要使用Oauth2.0?

### Answer

1. 首先我们需要明确一点Oauth2.0是协议,它包括了如下特征：

   - 授权协议:

     授权是Oauth2.0思想的核心，其目的就在于拥有了授权，就能够通过授权取得权限范围内的受保护资源

   - 委托协议:

     当用户授权第三方软件获取部分用户数据时，其本质就是资源所有者委托了第三方进行资源的使用

   - 安全协议:

     安全问题永远是互联网最为核心的问题，Oauth2.0协议本身就推荐了相当多的安全处理方案，因此说Oauth2.0也是安全协议

更需要永远记住的一个观点是，Oauth2.0不是一种**身份认证协议**，如果思维不能明确Oauth2.0的协议边界线，那么接下来的讨论我们将会不断的回到上述提到的3个问题。

2. OAuth 2.0 就是保证第三方（软件）只有在获得授权之后，才可以进一步访问授权者的数据

3. OAuth 2.0 一词中的 “Auth” 表示 “授权”，字母 “O” 是 Open 的简称，表示 “开放”。简而言之Oauth的核心思想在于开放授权，因此在使用Oauth协议时，我们也不禁需要确认自己的远大目标，是否要做一个**开放授权的系统**？

   谈到Oauth2.0就不得不提到Oauth1.0:

   > 在 OAuth 1.0 的时候，它有个 “很大的愿望” 就是想用一套授权机制来应对现实中的所有场景，比如 Web 应用场景、移动 App 应用场景、官方应用场景等等，但是这些场景并不是完全相同的。比如官方应用场景，你说还需要让用户来授权吗？如果需要，始终使用一套授权机制给用户带来的体验，是好还是坏呢？到了 OAuth 2.0 的时候，就解决了 OAuth 1.0 面临的这种“尴尬”。OAuth 2.0 不再局限于一种授权机制，它扩充了授权许可机制类型，有了授权码许可机制、客户端凭据机制、资源拥有者凭据机制和隐式许可机制。这样的 OAuth 机制就能够很灵活地适应现实中的各种场景，比如移动应用的场景、官方应用的场景，等等。

   具体的Oauth1.0 vs Oauth2.0比较可以参考下方参考文档，这里暂不赘述

   总结下来使用Oauth2.0的原因就是其改善了自身的授权机制，优化了安全处理，采用了多种授权的形式，以此来适应如今互联网在授权中会遇到的各类问题


## 4 characters 

Oauth2.0中最重要的四个角色

- 资源拥有者
- 受保护资源服务器
- 授权服务
- 第三方应用

不管我们在开放授权中的各类形式讨论，我们所有的讨论应该基于这4个角色，而不是在于使用Oauth2.0的哪种模式，因为模式的选择，其本质需要考虑所谓第三方应用是否可信，大家所处的是内部局域网，还是纯正的互联网第三方

## Todo Question

希望上述的讲解可以让大家同步的是Oauth2.0的基本问题，如同做一个SaaS平台一样，我们需要的是先讨论什么是SaaS，什么是租户隔离，而不是刚开始就讨论如何去实现SaaS设计

### Q1：Oauth2为什么不是身份认证协议？

在很多次的讨论中，我们会发现我们总是会认为Oauth2需要去实现用户的身份认证，因为获取授权的第一步，也是最重要的一步就是获取身份认证，这样我们才能判断是否要颁发access_token给请求方。既然说到了Oauth2不是一个身份认证协议，那么身份认证协议是什么呢？

### Answer1:OIDC

> OIDC是OpenID Connect的简称，OIDC=(Identity, Authentication) + OAuth 2.0(身份认证+授权协议)

身份认证协议OIDC，他是Oauth2.0的加工版，如同我们将面粉加工成了面包

在OIDC中会出现3个最为重要的角色：

- EU（End User）：代表最终用户

- RP（Relying Party）：第三方软件，即为认证服务的依赖方

- OP（OpenID Provider）：代表提供身份认证服务方。

![image-20200819160625871](/Users/gongzheng/Library/Application Support/typora-user-images/image-20200819160625871.png)

上图即为OIDC中的三角色与Oauth2.0中的四个角色的关系

#### 实现OIDC与实现Oauth2.0的区别是什么？

根据目前的Oauth2实现方案，我们在用户登录授权后会返回4个值：

- access_token
- refresh_token
- exp_time
- refreshExp_time

而基于OIDC的实现则会在返回第5个非常重要的信息:id_token

可能谈到这里大家会比较恼火，为何又多了一个从来没有听说过的东西？微信开放平台不也没有id_token这种东西吗？

基于我个人的认知，我个人认为微信开放平台是目前最为标准的Oauth2.0协议开放平台了，其实微信一直都返回了id_token

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1ghssvmpoxqj31aj0u0q73.jpg)

相信大家可以看到在微信的获取access_token返回接口中返回了一个参数：openId

其实openId的本质就是id_token,只是id_token的本质就是一个携带用户信息的jwtToken，微信在这里不过是返回形式直接返回了用户id，而非一个token的形式，毕竟如果是jwtToken我们又不得不去考虑它的自验证性的问题了，既然都是要暴露给第三方应用供其解析的，那openId自然可以明文返回



聊到这里，大家应该明白了，我们目前的access_token中，携带了类似于openId的用户唯一标识，也就是说我们将access_token的功能放大了，他变成了一个身份认证授权通过token,而不是一个单纯的授权token了

说的最为简单直白：

id_token的作用代表了它是哪个用户

access_token的作用在于可不可以访问



那么我们需要回到话题了，我们做错了吗？是否access_token就一定不能携带用户信息呢？

其实不然，在健身领域，关于健身补剂一直有一句俚语，抛开剂量谈毒性就是耍流氓

在我们的身份认证服务中，我个人认为也有相应的一句俚语，抛开环境谈实现也是耍流氓，必须要考量的环境问题一定是

大家之间是否可信？大家是否同属局域网还是互联网中？



接下来将要来今天要讨论的第二个话题了

### Q2：如何选择适合我们家的实现

首先一定会选择微服务架构，在此基础上我们将会考量如何实现OIDC，接下来的问题，参考文档皆为[微服务架构设计-Oauth2&JWT](https://www.kaper.com/?s=MICRO-SERVICES+ARCHITECTURE+WITH+OAUTH2+AND+JWT)

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1ghst6rbja0j31hc0u00x0.jpg)

这个架构的一个比较友好的地方在于，架构思路是基于k8s实现

> 名词解析：
>
> End-User: 最终用户
>
> Internet:互联网
>
> FW:防火墙
>
> Nginx:Nginx反向代理
>
> Web:部署服务器
>
> Gateway:网关
>
> IDP:Identity Provider 认证服务提供者,实现框架：Spring-security可以考量
>
> BFF: Backend for Frontend 后端For前端微服务层
>
> DomainSvc:整个微服务架构的底层。这些服务包含业务逻辑，通常有自己独立的数据库存储，还可以根据需要调用外部的服务
>
> DB:数据库
>
> 根据微服务分层原则，领域服务禁止调用其它的领域服务，更不允许反向调用 BFF 服务。这样做是为了保持微服务职责单一（Single Responsibility）和有界上下文（Bounded Context），避免复杂的领域依赖。领域服务是独立的开发、测试和发布单位。在电商领域，常见的领域服务有用户服务、商品服务、订单服务和支付服务等。

由此我们可知晓，无论是BFF还是DomainSvc都不会去存储用户的信息，因为他们都是通过JWT token中包含的用户信息来获取，而JWT token中的用户信息来源是在于IDP服务所对应的LoginSvc Customer DB中

接下来我们将会根据各个实际情况作为考量点

### Answer2-1：第一方应用 + 资源拥有者凭据模式

第一方应用，即为完全由我们开发的且处于同一局域网下的完全可信的应用

因为完全可信，我们可以选择OAuth 2.0 的资源拥有者凭据许可（Resource Owner Password Credentials Grant）

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1ghw6n3rp91j30yf0h6wg6.jpg)

相信大家看到这里一定会有了争论，这就是我们之前讨论的核心问题了，到底要不要将用户表集成进IDP中，但是我们当时都会产生一个额外的争论，就是Oauth只是一个授权服务。是的如果只看Oauth，我们永远只能得到这个结论，Oauth只是一个授权服务，可我认为我们的问题是否应该考虑，我们要做的是Oauth2.0还是OIDC呢？如果是OIDC，这一切是否就是即符合微服务架构思想，又符合OIDC协议，包含了Oauth2.0协议呢？

### Answer2-2：第三方应用+授权码模式

关于第三方应用的定义，我们如何来界定一个第三方应用，我认为这一点是相当有必要确认好的

需要判断第三方应用是否由我们开发，如果不为我们开发，则肯定是第三方应用

如果由我们开发，处于同一局域网，是否是第三方应用？不处于局域网只能通过互联网交互，是否是第三方应用？

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1ghw6ns3wsij316z0qxwih.jpg)

从上述图示，我们可以得知，用户必须要在守望有用户信息才可以通过授权码模式得到对应的授权码，从而获取access_token

以上就是关于Oauth2.0的第三方应用+授权码模式的获取Token流程

综上所述，个人认为关于access_token中是否可以携带用户信息，应该取决于是否第一方应用，如果是第一方应用，对于受保护的资源而言是相当可信的，那么我们可以将用户信息密文存入token中，如果是第三方应用，认为应该将access_token与用户openId分开返回

之后关于Oauth2.0的安全问题与防止攻击，将会在之后的文档中更新

## 参考文档:

> [PKCE代码交换证明密钥协议](https://dzone.com/articles/what-is-pkce)
>
> [OIDC解析](https://connect2id.com/assets/oidc-explained.pdf)
>
> [微服务架构设计-Oauth2&JWT](https://www.kaper.com/?s=MICRO-SERVICES+ARCHITECTURE+WITH+OAUTH2+AND+JWT)
>
> [极客时间-Oauth2.0实战课](https://time.geekbang.org/column/intro/321)
>
> [Oauth2.0 vs Oauth1.0](https://blog.csdn.net/ivolcano/article/details/90414175)


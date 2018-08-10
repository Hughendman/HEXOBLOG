---
title: API Gateway
categories: 微服务
tags: 微服务
keywords: 微服务
abbrlink: 83d1d1c
date: 2018-07-13 18:15:00
---
## API Gateway

百度介绍：

 >  API网关是一个服务器，是系统的唯一入口。从面向对象设计的角度看，它与外观模式类似。API网关封装了系统内部架构，为每个客户端提供一个定制的API。它可能还具有其它职责，如身份验证、监控、负载均衡、缓存、请求分片与管理、静态响应处理。


> API网关方式的核心要点是，所有的客户端和消费端都通过统一的网关接入微服务，在网关层处理所有的非业务功能。通常，网关也是提供REST/HTTP的访问API。服务端通过API-GW注册和管理服务。

下图解释了API Gateway的作用

![](https://raw.githubusercontent.com/Hughendman/picture/master/API/1.png)

### 为什么需要API Gateway

可以参考以下两张图片：

![](https://raw.githubusercontent.com/Hughendman/picture/master/API/2.png)

从部署结构上说，上图是不采用API Gateway的微服务部署模式，我们可以清晰看到，这种部署模式下，客户端与负载均衡器直接交互，完成服务的调用。但这是这种模式下，也有它的不足。

* 不支持动态扩展，系统每多一个服务，就需要部署或修改负载均衡器。

* 无法做到动态的开关服务，若要下线某个服务，需要运维人员将服务地址从负载均衡器中移除。

* 对于API的限流，安全等控制，需要每个微服务去自己实现，增加了微服务的复杂性，同时也违反了微服务设计的单一职责原则。

![](https://raw.githubusercontent.com/Hughendman/picture/master/API/3.png)

上图为采用API Gateway模式，我们通过上图可以看到，API Gateway做为系统统一入口，实现了对各个微服务间的整合，同时又做到了对客户端友好，屏蔽系统了复杂性和差异性。对比之前无API Gateway模式，API Gateway具有几个比较重要的优点：

* 采用API Gateway可以与微服务注册中心连接，实现微服务无感知动态扩容。

* API Gateway对于无法访问的服务，可以做到自动熔断，无需人工参与。

* API Gateway可以方便的实现蓝绿部署，金丝雀发布或A/B发布。

* API Gateway做为系统统一入口，我们可以将各个微服务公共功能放在API Gateway中实现，以尽可能减少各服务的职责。

* 帮助我们实现客户端的负载均衡策略。

### API Gateway中一些重要的功能

#### 负载均衡

![](https://raw.githubusercontent.com/Hughendman/picture/master/API/4.png)

实际的部署应用中，当应用系统面临大量访问，负载过高时，通常我们会增加服务数量来进行横向扩展，使用集群来提高系统的处理能力。此时多个服务通过某种负载算法分摊了系统的压力，我们将这种多节点分摊压力的行为称为负载均衡。

API Gateway可以帮助我们轻松的实现负载均衡，利用服务发现知道所有Service的地址和位置，通过在API Gateway中实现负载均衡算法，就可以实现负载均衡效果。

#### 服务熔断

![](https://raw.githubusercontent.com/Hughendman/picture/master/API/5.png)

在实际生产中，一些服务很有可能因为某些原因发生故障，如果此时不采取一些手段，会导致整个系统“雪崩”。或因系统整体负载的考虑，会对服务访问次数进行限制。服务熔断、服务降级就是解决上述问题的主要方式。API Gateway可以帮助我们实现这些功能，对于服务的调用次数的限制，当某服务达到上限时，API Gateway会自动停止向上游服务发送请求，并像客户端返回错误提示信息或一个统一的响应，进行服务降级。对于需要临时发生故障的服务，API Gateway自动可以打开对应服务的断路器，进行服务熔断，防止整个系统“雪崩”。

#### 灰度发布

![](https://raw.githubusercontent.com/Hughendman/picture/master/API/6.png)

服务发布上线过程中，我们不可能将新版本全部部署在生产环节中，因为新版本并没有接受真实用户、真实数据、真实环境的考验，此时我们需要进行灰度发布，灰度发布可以保证整体系统的稳定，在初始灰度的时候就可以发现、调整问题，同时影响小。API Gateway可以帮助我们轻松的完成灰度发布，只需要在API Gateway中配置我们需要的规则，按版本，按IP段等，API Gateway会自动为我们完成实际的请求分流。


### 认证和鉴权

目前在微服务中，我们还需要考虑如何保护我们的API只能被同意授权的客户调用。那么对于API的保护，目前大多数采用的方式有这么几种，分别为AppKeys，OAuth2 和 OAuth2+JWT

#### 认证方式

##### AppKeys

![](https://raw.githubusercontent.com/Hughendman/picture/master/API/7.png)


目前采用AppKeys Auth认证的公有云API Gateway和数据开放平台居多，如阿里API Gateway，聚合数据等，这种认证模式是由API Gateway颁发一个key，或者appkey+appsecret+某种复杂的加密算法生成AppKey，调用方获取到key后直接调用API。这个key可以是无任何意义的一串字符。API Gateway在收到调用API请求时，首先校验key的合法性，包括key是否失效，当前调用API是否被订阅等等信息，若校验成功，则请求上游服务，返回结果。此处上游服务不再对请求做任何校验，直接返回结果。采用AppKeys认证模式比较适合Open Service的场景。其中并不涉及到用户信息，权限信息。

##### OAuth2

![](https://raw.githubusercontent.com/Hughendman/picture/master/API/8.png)


大部分场景中，我们还是需要有知道谁在调用，调用者是否有对应的角色权限等。OAuth2可以帮助我们来完成这个工作。在OAuth2的世界中，分为以下几种角色：Resource Owner，Client，Authorization Server，Resource Sever。上图是一个简单的OAuh2流程来说明各个角色之前的关系。最终，App获得了Rory的个人信息。

##### OAuth2+JWT

![](https://raw.githubusercontent.com/Hughendman/picture/master/API/9.png)

OAuth2 + JWT流程跟OAuth2完全一致。了解OAuth2的童鞋都应该知道，OAuth2最后会给调用方颁发一个Access Token，OAuth2+JWT实际上就是将Access Token换成JWT而已。这样做的好处仅仅是减少Token校验时查询DB的次数。

## 参考

[API管理的正确姿势--API Gateway](https://yq.aliyun.com/articles/597799)
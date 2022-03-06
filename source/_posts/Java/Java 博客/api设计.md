---
title: Api设计
date: 2022-2-9 16:56:43
tags:
  - Java 常用类
categories: Java 博客
---
## API设计

任何API设计都遵循一种叫做“面向资源设计”的原则：

资源：资源是数据的一部分，例如：用户

集合：一组资源称为集合，例如：用户列表

URL：标识资源或集合的位置，例如：/user

1. **API格式黄金法则**
扁平比嵌套好
简单胜于复杂
字符串比数字好
一致性比定制更好

2. 对CRUD函数使用HTTP方法,用于解释CRUD功能
GET：检索资源的表示形式
POST：创建新的资源和子资源
PUT：更新现有资源
PATCH：更新现有资源，它只更新提供的字段，而不更新其他字段
DELETE：删除已存在的资源

3. 始终在API中使用版本控制
始终对API使用版本控制，并将其向左移动，使其具有最大的作用域。版本号应该是v1，v2等等。
应该：http://api.domain.com/v1/shops/3/products
始终在API中使用版本控制，因为如果API被外部实体使用，更改端点可能会破坏它们的功能。

4. 不要使用table_name作为资源名
不应该：
product_order
应该：
product-orders
这是因为公开底层体系结构不是你的目的

5. 监控
RESTful HTTP服务必须实现/health和/version和/metricsAPI端点。他们将提供以下信息。

/health
用200 OK状态码响应对/health的请求
/version
用版本号响应对/version的请求
/metrics
这个端点将提供各种指标，如平均响应时间
也强烈推荐使用/debug和/status端点

6. 对URL使用kebab-case（短横线小写隔开形式）
例`/system-orders`

7. 参数使用camelCase（小驼峰形式）
例`/system-orders/{orderId}`

8. 指向集合的复数名称
例`/users`

9. URL以集合开始，以标识符结束
`/shops/:shopId/或/category/:categoryId`

10. 让动词远离你的资源URL
例:`PUT /user/{userId}`

11. 对非资源URL使用动词(非crud)
例:`/alarm/245743/resend`

12. JSON属性使用camelCase驼峰形式
例:
``` Json
{
   "userName": "Mohammad Faisal",
   "userId": "1"
}
```

13. 使用API设计工具

有许多好的API设计工具用于编写好的文档，例如：
API蓝图：https://apiblueprint.org/
Swagger：https://swagger.io/

拥有良好而详细的文档可以为API使用者带来良好的用户体验。


14. 在你的响应体中包括总资源数

如果API返回一个对象列表，则响应中总是包含资源的总数。你可以为此使用total属性。

不应该：
{
  users: [ 
     ...
  ]
}

应该：
{
  users: [ 
     ...
  ],
  total: 34
}


15. 在GET操作中始终接受limit和offset参数

应该：
GET /shops?offset=5&limit=5

这是因为它对于前端的分页是必要的。


16. 获取字段查询参数
返回的数据量也应该考虑在内。添加一个fields参数，只公开API中必需的字段。
例子：
只返回商店的名称，地址和联系方式。
GET /shops?fields=id,name,address,contact
在某些情况下，它还有助于减少响应大小。
17. 不要在URL中通过认证令牌
错:GET /shops/123?token=some_kind_of_authenticaiton_token
相反，通过头部传递它们：
Authorization: Bearer xxxxxx, Extra yyyyy
`此外，授权令牌应该是短暂有效期的`。


18. 验证内容类型
服务器不应该假定内容类型。例如，如果你接受application/x-www-form-urlencoded，那么攻击者可以创建一个表单并触发一个简单的POST请求。

因此，`始终验证内容类型`，如果你想使用默认的内容类型，请使用：
content-type: application/json


19. CORS（跨源资源共享）
所有面向公共的API支持CORS（跨源资源共享）头部
考虑支持CORS允许的“*”来源，并通过有效的OAuth令牌强制授权
`避免将用户凭证与原始验证相结合`


20. 安全
在所有端点、资源和服务上实施HTTPS（tls加密）。
强制并要求所有回调url、推送通知端点和webhooks使用HTTPS。

21. 错误
客户端向服务发出无效或不正确的请求，或向服务传递无效或不正确的数据，服务拒绝该请求时，就会出现错误
当由于一个或多个服务错误而拒绝客户端请求时，`一定要返回4xx HTTP错误代码`。
考虑处理所有属性，然后`在单个响应中返回多个验证问题`。

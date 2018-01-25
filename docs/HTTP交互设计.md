# HTTP 交互设计

## 使用 token 令牌来做用户身份校验

和 Web 应用不同，REST API通常是无状态的， 也就意味着不应使用 `sessions` 或 `cookies`， 因此每个请求应附带某种授权凭证，因为用户授权状态可能没通过`sessions` 或 `cookies`维护， 常用的做法是每个请求都发送一个秘密的 `access_token` 来认证用户， 由于 `access_token` 可以唯一识别和认证用户。

身份验证做法多种多样，YII2框架针对REST有专门[授权验证](http://www.yiichina.com/doc/guide/2.0/rest-authentication)机制，可供参考。

## HTTP响应状态码的使用

状态码范围：

| 状态码 | 描述 |
| ---- | ---- |
| 2XX | 信息，请求收到，继续处理。范围保留用于底层HTTP的东西，你很可能永远也用不到。|
| 3XX | 重定向，为了完成请求，必须进一步执行的动作。|
| 4XX | 客户端错误，请求包含语法错误或者请求无法实现。范围保留用于响应客户端做出的错误，例如。他们提供不良数据或要求不存在的东西。这些  请求应该是幂等的，而不是更改服务器的状态。|
| 5XX | 范围的状态码是保留给服务器端错误用的。这些错误常常是从底层的函数抛出来的，甚至开发人员也通常没法处理，发送这类状态码的目的以确  保客户端获得某种响应。当收到5xx响应时，客户端不可能知道服务器的状态，所以这类状态码是要尽可能的避免。|

当收到5XX响应时，客户端不可能知道服务端的状态，所以这类状态码应该要尽可能避免。

常用状态码范围使用：

| 状态码 | 返回状态 | 描述 |
| ---- | ---- | ---- |
| 200 | OK | 用于一般性的成功返回，不可用于请求错误返回。|
| 201 | Created | 资源被创建。|
| 202 | Accepted | 用于 `Controller` 控制类资源异步处理的返回，仅表示请求已经收到。对于耗时比较久的处理，一般用异步处理来完成。 |
| 204 | No Content | 此状态可能会出现在 `PUT`、`POST`、`DELETE`的请求中，一般表示资源存在，但消息体中不会返回任何资源相关的状态或信息。|
| 301 | Moved Permanently | 资源的URI被转移，需要使用新的URI访问。 |
| 302 | Found | 不推荐使用，此代码在 `HTTP1.1` 协议中被 `303/307` 替代。我们目前对302的使用和最初`HTTP1.0`定义的语意是有出入的，应该只有在`GET/HEAD`方法下，客户端才能根据`Location`执行自动跳转，而我们目前的客户端基本上是不会判断原请求方法的，无条件的执行临时重定向。 |
| 303 | See Other | 返回一个资源地址`URI`的引用，但不强制要求客户端获取该地址的状态(访问该地址)。 |
| 304 | Not Modified | 有一些类似于`204`状态，服务器端的资源与客户端最近访问的资源版本一致，并无修改，不返回资源消息体。可以用来降低服务 端的压力。|
| 307 | Temporary Redirect | 目前`URI`不能提供当前请求的服务，临时性重定向到另外一个`URI`。在`HTTP1.1`中`307`是用来替代早期`HTTP1.0`中使用不当的`302`。|
| 400 | Bad Request | 用于客户端一般性错误返回, 在其它`4xx`错误以外的错误，也可以使用`400`，具体错误信息可以放在`body`中。 |
| 401 | Unauthorized | 在访问一个需要验证的资源时，验证错误。 |
| 403 | Forbidden | 一般用于非验证性资源访问被禁止，例如对于某些客户端只开放部分API的访问权限，而另外一些API可能无法访问时，可以给予`403`状态。|
| 404 | Not Found | 找不到URI对应的资源。 |
| 405 | Method Not Allowed | `HTTP`的方法不支持，例如某些只读资源，可能不支持`POST/DELETE`。但`405`的响应`header`中必须声明该`URI`所支持的方法。 |
| 406 | Not Acceptable | 客户端所请求的资源数据格式类型不被支持，例如客户端请求数据格式为`application/xml`，但服务器端只支持`application/json`。|
| 409 | Conflict | 资源状态冲突，例如客户端尝试删除一个非空的`Store`资源。|
| 412 | Precondition Failed | 用于有条件的操作不被满足时。|
| 415 | Unsupported Media | 客户所支持的数据类型，服务端无法满足。|
| 500 | Internal Server Error | 服务器端的接口错误，此错误于客户端无关。|

状态码的完整列表参考[这里](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html)

错误代码说明：

| 服务级错误（1为系统级错误） | 服务模块代码 | 具体错误代码 |
| :----: | :----: | :----: |
| 2 | 05 | 02 |

错误代码对照表：

**系统级错误代码：**

| 错误级代码 | 错误信息 | 详细描述 |
| ---- | ---- | ---- |
| 10001 | System error | 系统错误。|
| 10002 | Service unavailable | 服务暂停。|
| 10003 | Remote service error | 远程服务错误。|
| 10004 | IP limit | IP限制不能请求该资源。|
| 10005 | Permission denied, need a high level appkey | 该资源需要appKey拥有授权。|
| 10006 | Source paramter (appkey) is missing	 | 缺少source (appkey)参数。|
| 10007 | Param error, see doc for more info | 参数错误，请参考API文档。|
| 10008 | too many pending tasks, system is busy | 任务过多，系统繁忙。|
| 10009 | Job expired | 任务超时。|
| 10010 | RPC error | RPC错误。|
| 10011 | Illegal request | 非法请求。|
| 10012 | Insufficient app permissions | 应用的接口访问权限受限。|
| 10013 | Request api not found | 接口不存在。|
| 10014 | HTTP method is not suported for this request | 请求的HTTP METHOD不支持，请检查是否选择了正确的POST/GET方式。|
| 10015 | IP requests out of rate limit | IP请求频次超过上限。|
| 10016 | User requests out of rate limit | 用户请求频次超过上限。|
| 10017 | User requests for (%s) out of rate limit | 用户请求特殊接口 (%s) 频次超过上限。|

**服务级错误代码：**

| 错误级代码 | 错误信息 | 详细描述 |
| ---- | ---- | ---- |
| 20001 | IDs is null | IDs参数为空。|
| 20002 | User does not exists | 用户不存在。|
| ..... | ... | ... |

由于项目的独特性，故服务级错误在这里不描述，开发人员可根据错误代码说明进行各模块的错误描述。

接口返回格式：

```
{
   code:0,
   message: “请求成功”,
   data : [{"id" : 1, "username" : "abc"}]
}
```
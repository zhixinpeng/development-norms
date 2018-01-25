# URI 格式规范

概念理解：

- URI(Uniform Resource Identifiers)统一资源标示符
- URL(Uniform Resource Locator)统一资源定位符

URL是URI的一个子集（一种具体实现），对于REST API来说一个资源对应唯一的URI。在URI的设计中，我们会遵循一些规则，使接口看起来透明易读，方便使用者调用。

URI的格式定义如下：

```
URI = scheme "://" authority "/" path [ "?" query ] [ "#" fragment ]
```

## 分隔符 "/" 的使用

`/` 分隔符一般用来对资源层级的划分，例如：

```
http://api.example.com/shapes/polygons
```

对于REST API 来说，`/` 只是一个分隔符，并无其他含义。为了避免混淆，`/` 不应该出现在URL的末尾。如下两个地址实际表示的都是同一资源：

```
http://api.example.com/shapes/
http://api.example.com/shapes
```

REST API对URI资源的定义具有唯一性，一个资源对应一个唯一的地址。为了使接口保持清淅干净，如果访问到末尾包含 `/` 的地址，服务端应该301到没有 `/` 的地址上。这个规则只限于REST API接口访问。对于传统的WEB页面服务来说，并不一定适用于这个规则。

## URI尽量使用连字符 "-" 代替下划线 "_"

连字符 `-` 可以用来分割URI中出现的字符串（单词），来提高URI的可读性。例如：

```
http://api.example.com/v1/api/hello-api
```

## URI统一使用小写字母

根据 [RFC3986](https://tools.ietf.org/html/rfc3986) 定义，URI是对大小写敏感的，所以为了避免歧义，我们尽量用小写字符。主机名（host）与协议名称（scheme）对大小写不敏感。

## URI中不包含文件（脚本）的扩展名

例如 `.php`、`.json`等不要出现，对于接口没有任何意义。如果是对返回的数据内容格式标示。可通过 `HTTP Header` 的 `Content-Type` 返回。
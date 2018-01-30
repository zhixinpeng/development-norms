# Yii2 RESTfulAPI规范

## Yii REST规范

`Yii2`提供了一整套用来简化实现`RESTful`风格的`web Service`服务API.其中支持以下`RESTful`风格的API：

- 支持 Active Record 类的通用API的快速原型
- 涉及的响应格式（在默认情况下支持 JSON 和 XML）
- 支持可选输出字段的定制对象序列化
- 适当的格式的数据采集和验证错误
- 支持 HATEOAS
- 有适当HTTP动词检查的高效的路由
- 内置OPTIONS和HEAD动词的支持
- 认证和授权，数据缓存和HTTP缓存
- 速率限制

详情请参见[这里](http://www.yiichina.com/doc/guide/2.0/rest-quick-start)

## REST测试工具

windows平台可下载PostMAN工具

Mac 平台可下载PAW工具

## 文档生成工具apiDoc

`apiDoc`可以根据代码注释生成`web api`文档，支持大部分主流语言`java` `javascript` `php` `coffeescript` `erlang` `perl` `python` `ruby` `go`...，相对而言，web接口的注释维护起来更加方便，不需要额外再维护一份文档。

`Yii2`框架可通过`composer`管理，安装`apiDoc`，可通过以下命令：

```shell
Composer require –prefer-dist yiisoft/yii2-apidoc
```

或者在`composer.json`中添加:

```json
"yiisoft/yii2-apidoc" : "~2.0.0"
```

或者通过npm 进行安装, 具体使用, 请参见[这里](http://blog.csdn.net/soslinken/article/details/50468896)
